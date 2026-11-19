# Consumer Complaints – Topic Modeling

Portfolio-Projekt im Kurs „Projekt: Data Analysis" (IU).

## Datengrundlage

[Consumer Complaint Database (Kaggle)](https://www.kaggle.com/datasets/selener/consumer-complaint-database), Textspalte „Consumer complaint narrative".


## Methoden

- Vorverarbeitung: pandas, spaCy
- Vektorisierung: Bag-of-Words und TF-IDF 
- Themenextraktion: LDA  und NMF 
- Bestimmung der Themenanzahl: Coherence Score

## Einrichtung

Python 3.12 (spaCy unterstützt neuere Versionen  derzeit nicht).

```
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

## Status

Phase 2 in Bearbeitung