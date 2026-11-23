# Consumer Complaints 

Dieses Repository gehört zum Portfolio im Kurs „Projekt: Data Analysis" (IU). Ziel ist es, aus einer großen Menge unstrukturierter Verbraucherbeschwerden mit NLP-Techniken automatisch die am häufigsten angesprochenen Themen zu extrahieren. So soll man einen schnellen Überblick über die dringlichsten Probleme bekommen, ohne die Texte einzeln lesen zu müssen.

**Status:** 
Die Vorverarbeitung ist abgeschlossen. Als Nächstes folgen Vektorisierung und Themenextraktion.

## Daten

Verwendet wird die Consumer Complaint Database von Kaggle. Es handelt sich um Beschwerden von US-Verbrauchern über Finanzprodukte, die ursprünglich von einer US-Bundesbehörde veröffentlicht wurden (Lizenz laut Kaggle: U.S. Government Works). Ausgewertet wird die Spalte „Consumer complaint narrative".

| Schritt | Anzahl Beschwerden |
|---|---|
| Datensatz gesamt | 1.282.355 |
| Beschwerdetext vorhanden | 383.564 |
| nach Entfernen von Duplikaten | 366.945 |
| Stichprobe für die Analyse | 20.000 |
| nach der Vorverarbeitung | 19.807 |

Die meisten Einträge haben keinen Text, vermutlich weil Verbraucher der Veröffentlichung ihrer Schilderung zustimmen müssen. Die Texte sind anonymisiert: Namen und Daten sind durch Platzhalter ersetzt. Die Länge der Beschwerden schwankt stark, und die Texte enthalten Tippfehler und Wiederholungen. Die Stichprobe wird mit random_state=42 gezogen und ist reproduzierbar.

## Vorverarbeitung 

Aus den Beschwerden werden Wortlisten aus Grundformen. Dazu werden Platzhalter mit regulären Ausdrücken entfernt und die Texte mit spaCy zerlegt. Es bleiben nur Nomen, Verben und Adjektive ohne Stoppwörter mit mindestens drei Buchstaben. Eine Korrektur-Tabelle behebt fehlerhafte Grundformen und einen häufigen Tippfehler. Wörter, die in weniger als 10 Beschwerden vorkommen, und Beschwerden mit weniger als 4 Wörtern werden entfernt.

| Kennzahl | Wert |
|---|---|
| Beschwerden | 19.807 |
| Vokabular | 3.571 Wörter |
| Wörter insgesamt | 1.240.778 |
| Median Wörter pro Beschwerde | 44 |

Die Begründungen für die einzelnen Entscheidungen stehen im Notebook `02_vorverarbeitung.ipynb`.

## Inhalt des Repositories

| Datei / Ordner | Inhalt |
|---|---|
| `01_analyse.ipynb` | Daten laden, auf Beschwerden mit Text einschränken, Duplikate entfernen, Stichprobe ziehen |
| `02_vorverarbeitung.ipynb` | Bereinigung, Lemmatisierung mit spaCy, Filter, Ergebnis speichern |
| `sample/sample_500.csv` | kleine Beispielstichprobe (500 Beschwerden) |
| `requirements.txt` | Python-Pakete mit Versionen |

Nicht im Repository (siehe `.gitignore`): der Ordner `data/` mit dem Originaldatensatz (rund 175 MB), der großen Stichprobe und dem Ergebnis der Vorverarbeitung (`data/tokens_final.pkl`).

## Ausführen

Die Notebooks bauen aufeinander auf und werden in dieser Reihenfolge ausgeführt: `01` → `02` → (folgt: `03_vektorisierung.ipynb`). Jedes Notebook liest die Ausgabe des vorigen als Datei ein.

1. Datensatz von Kaggle herunterladen und als `data/rows.csv` ablegen. Ohne diese Datei läuft `01_analyse.ipynb` nicht.
2. Die Umgebung einrichten (siehe unten).
3. `01_analyse.ipynb` ausführen. Es erzeugt `data/sample_20000.csv`.
4. `02_vorverarbeitung.ipynb` ausführen. Der spaCy-Lauf dauert für 20.000 Texte etwa vier Minuten.

Fehlt `data/sample_20000.csv`, verwendet `02_vorverarbeitung.ipynb` stattdessen `sample/sample_500.csv`. Das ist zum Ausprobieren gedacht: Mit nur 500 Texten ist die Untergrenze für seltene Wörter (10 Beschwerden) sehr streng, und die Ergebnisse weichen von denen mit 20.000 Texten ab.

## Einrichtung

Getestet mit Python 3.12. Neuere Python-Versionen werden von spaCy 3.8 laut Paketangaben derzeit nicht unterstützt.


## Geplantes Vorgehen

- Vektorisierung: Bag-of-Words und TF-IDF (scikit-learn), mit Vergleich
- Themenextraktion: LDA (gensim) und NMF (scikit-learn)
- Bestimmung der Themenanzahl über den Coherence Score 