# Consumer Complaints 

Dieses Repository gehört zum Portfolio im Kurs „Projekt: Data Analysis" . Ziel ist es, aus einer großen Menge unstrukturierter Verbraucherbeschwerden mit NLP-Techniken automatisch die am häufigsten angesprochenen Themen zu extrahieren. Somit soll ein schneller Überblick über die dringlichsten Probleme gewährleistet sein, ohne dass man die  Texte einzeln lesen  muss.

## Daten

Verwendet wird die Consumer Complaint Database (https://www.kaggle.com/datasets/selener/consumer-complaint-database) von Kaggle. Es handelt sich um Beschwerden von US-Verbrauchern über Finanzprodukte, die ursprünglich von einer US-Bundesbehörde veröffentlicht wurden. Lizenz laut Kaggle: U.S. Government Works. Ausgewertet wird die Spalte „Consumer complaint narrative".

| Schritt | Anzahl Beschwerden |
|---|---|
| Datensatz gesamt | 1.282.355 |
|  Beschwerdetext vorhanden | 383.564 |
| nach Entfernen von Duplikaten | 366.945 |
| Stichprobe für die Analyse | 20.000 |

Die meisten Einträge haben keinen Text, weil Verbraucher der Veröffentlichung ihrer Schilderung zustimmen müssen. Die Texte sind anonymisiert, dabei ersetzt der Datensatz Namen und Daten durch Platzhalter. Die Länge der Beschwerden schwankt stark, und die Texte enthalten Tippfehler und Wiederholungen. Das muss berücksichtigt werden.

Die Stichprobe wird mit `random_state=42` gezogen und ist reproduzierbar.

##  Repository

| Datei / Ordner | Inhalt |
|---|---|
| `analyse.ipynb` | Notebook: Laden, Filtern, Duplikate entfernen, Stichprobe |
| `sample/sample_500.csv` | kleine Beispielstichprobe (500 Texte) |
| `requirements.txt` | Python-Pakete mit Versionen |

Der Originaldatensatz (rund 175 MB) liegt nicht im Repository. Zum Ausführen des Notebooks muss er von Kaggle heruntergeladen und als `data/rows.csv` abgelegt werden.

## Einrichtung

Getestet mit Python 3.12. Neuere Versionen werden von spaCy derzeit nicht unterstützt.

```
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

## Geplantes Vorgehen

- Vorverarbeitung: pandas, spaCy
- Vektorisierung: Bag-of-Words und TF-IDF (scikit-learn)
- Themenextraktion: LDA (gensim) und NMF (scikit-learn)
- Bestimmung der Themenanzahl über den Coherence Score

