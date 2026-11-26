# NLP Techniken zur Analyse einer Textsammlung

Dieses Repository gehört zum Portfolio im Kurs „Projekt: Data Analysis". Ziel ist es, aus einer großen Menge unstrukturierter Verbraucherbeschwerden mit NLP-Techniken automatisch die am häufigsten angesprochenen Themen zu extrahieren. So soll man einen schnellen Überblick über die dringlichsten Probleme bekommen, ohne die Texte einzeln lesen zu müssen.

## Daten

Verwendet wird die Consumer Complaint Database (https://www.kaggle.com/datasets/selener/consumer-complaint-database) von Kaggle. Es handelt sich um Beschwerden von US-Verbrauchern über Finanzprodukte, die ursprünglich von einer US-Bundesbehörde veröffentlicht wurden. Ausgewertet wird die Spalte „Consumer complaint narrative".

| Schritt | Anzahl Beschwerden |
|---|---|
| Datensatz gesamt | 1.282.355 |
| Beschwerdetext vorhanden | 383.564 |
| nach Entfernen von Duplikaten | 366.945 |
| Stichprobe für die Analyse | 20.000 |
| nach der Vorverarbeitung | 19.807 |

Die meisten Einträge haben keinen Text, vermutlich weil Verbraucher der Veröffentlichung ihrer Schilderung zustimmen müssen. Die Texte sind anonymisiert, Namen und Daten sind durch Platzhalter ersetzt. Die Länge der Beschwerden schwankt stark, und die Texte enthalten Tippfehler. Die Stichprobe wird mit `random_state=42` gezogen und ist reproduzierbar.

## Vorgehen und Ergebnisse

1. **Analyse**: Auf Beschwerden mit Text einschränken (383.564), Duplikate entfernen (366.945) und eine Stichprobe von 20.000 Beschwerden ziehen (siehe Abschnitt „Daten").
2. **Vorverarbeitung**: Platzhalter entfernen (reguläre Ausdrücke), Texte auf Grundformen bringen, nur Nomen, Verben und Adjektive ohne Stoppwörter mit mindestens drei Buchstaben behalten, fehlerhafte Grundformen und einen häufigen Tippfehler über eine Korrektur-Tabelle beheben, Wörter in weniger als 10 Beschwerden und Beschwerden mit weniger als 4 Wörtern entfernen. Ergebnis: 19.807 Beschwerden, 3.571 verschiedene Wörter, 1.240.778 Wörter insgesamt, im Median 44 Wörter pro Beschwerde.
3. **Vektorisierung**: Bag-of-Words und TF-IDF auf demselben Vokabular (Matrix 19.807 × 3.571), im Notebook verglichen. Dazu Wörterbuch und Korpus im gensim-Format.
4. **Themenmodellierung**: Vorversuch mit LDA (gensim, Bag-of-Words) und NMF (scikit-learn, TF-IDF) bei vorläufig k = 10.
5. **Themenanzahl**: Coherence Score (c_v) für k = 5, 7, 10, 12, 15 und 20, mit Diagramm. Gewählt: LDA mit k = 12 (Coherence 0,550) und NMF mit k = 15 (0,568). Die Unterschiede zu den Nachbarwerten sind klein (rund 0,004), die Wahl stützt sich deshalb auch auf die inhaltliche Prüfung der Top-Wörter.
6. **Auswertung**: Finale Modelle mit diesen Themenzahlen. Beide Verfahren finden dieselben Kernbereiche. Fasst man ähnliche Themen zusammen, ist die Kreditauskunft der größte Bereich (rund 23 % bei NMF bis 31 % bei LDA der Beschwerden), danach folgen Inkasso (je rund 10 %), Hypothek, Studienkredit und Bankkonto. Für 10 von 12 LDA-Themen gibt es ein NMF-Thema mit mindestens 5 gemeinsamen Top-Wörtern.

## Einrichtung

Getestet mit Python 3.12. Neuere Python-Versionen werden von spaCy 3.8 laut Paketangaben derzeit nicht unterstützt.

## Information

Nicht im Repository (siehe `.gitignore`): der Ordner `data/` mit dem Originaldatensatz (rund 175 MB), der großen Stichprobe und dem Ergebnis der Vorverarbeitung (`data/tokens_final.pkl`).