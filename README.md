# Exploratory Text Analytics of Gothic and Science Fiction Literature

Abraham Tedla (wqp7qy@virginia.edu)
DS 5001 — Final Project
April 2026

## Overview

This project builds a digital critical edition of 12 novels from Project Gutenberg, spanning Gothic fiction and early science fiction. The corpus is processed through a full text analytics pipeline (F0→F5) and explored using statistical and visualization methods to uncover cultural patterns around emotion, thematic structure, and lexical identity.

## Corpus

| Genre | Title | Author | Gutenberg ID |
|---|---|---|---|
| Gothic | Frankenstein | Shelley, Mary | 84 |
| Gothic | Dracula | Stoker, Bram | 345 |
| Gothic | Dr. Jekyll and Mr. Hyde | Stevenson, Robert Louis | 42 |
| Gothic | Wuthering Heights | Brontë, Emily | 768 |
| Gothic | The Picture of Dorian Gray | Wilde, Oscar | 174 |
| Gothic | The Castle of Otranto | Walpole, Horace | 696 |
| SciFi | The Time Machine | Wells, H. G. | 35 |
| SciFi | The War of the Worlds | Wells, H. G. | 36 |
| SciFi | The Island of Doctor Moreau | Wells, H. G. | 159 |
| SciFi | Twenty Thousand Leagues Under the Sea | Verne, Jules | 164 |
| SciFi | A Princess of Mars | Burroughs, Edgar Rice | 62 |
| SciFi | The Lost World | Doyle, Arthur Conan | 139 |

Source texts are hosted on [UVA Box](https://virginia.box.com/s/n9ojz511cikug8db50y2pkw74522cvdw). See `MANIFEST.md` for full provenance details.

## Requirements

```
pip install pandas numpy matplotlib seaborn nltk scikit-learn gensim requests scipy
```

NLTK data downloads are handled automatically within the notebooks.

## How to Run

Run the notebooks **in order** — each one reads outputs from the previous step.

| Notebook | Step | Description |
|---|---|---|
| `00_eda_gutenberg.ipynb` | EDA | Explore the Gutenberg SQLite catalog, select corpus candidates |
| `01_acquire_corpus.ipynb` | F0/F1 | Download texts, strip boilerplate, parse into MLCF, build `LIBRARY.csv` |
| `02_tokenization.ipynb` | F2 | Tokenize into words, POS tag, build `TOKEN.csv` and `VOCAB.csv` |
| `03_annotation.ipynb` | F3 | Add stopwords, stems, lemmas, NRC sentiment to TOKEN and VOCAB |
| `04_tfidf.ipynb` | F4 | Compute TF, IDF, TFIDF; add to TOKEN and VOCAB |
| `05_models.ipynb` | F5 | PCA, LDA, word2vec; produce component/topic/embedding tables |
| `06_exploration.ipynb` | F6 | Visualizations, numerical summary, save report figures |

**Notes:**
- Notebook 01 downloads texts from Gutenberg (requires internet). Files are cached in `source_texts/` — subsequent runs skip the download.
- Notebook 05 trains word2vec with `workers=1` for reproducibility. This is slower but ensures identical results across runs.
- All notebooks use `np.random.seed(42)` for reproducibility.
- The last cell of notebook 06 saves all report figures to `figures/`.

## Project Structure

```
├── 00_eda_gutenberg.ipynb       # EDA on Gutenberg catalog
├── 01_acquire_corpus.ipynb      # Corpus acquisition (F0/F1)
├── 02_tokenization.ipynb        # Tokenization & STADM (F2)
├── 03_annotation.ipynb          # NLP annotation & sentiment (F3)
├── 04_tfidf.ipynb               # TFIDF vectorization (F4)
├── 05_models.ipynb              # PCA, LDA, word2vec (F5)
├── 06_exploration.ipynb         # Exploration & visualization (F6)
├── final_report.md              # 2–4 page final report
├── MANIFEST.md                  # Source file manifest
├── README.md                    # This file
├── gutenberg.db                 # Project Gutenberg catalog (SQLite)
├── source_texts/                # Downloaded plaintext novels
├── output/                      # All CSV data tables
│   ├── LIBRARY.csv
│   ├── CORPUS_F1.csv
│   ├── TOKEN.csv
│   ├── VOCAB.csv
│   ├── TFIDF.csv
│   ├── DOC_COMPONENTS.csv
│   ├── LOADINGS.csv
│   ├── DOC_TOPICS.csv
│   ├── TOPIC_TERMS.csv
│   ├── WORD2VEC.csv
│   └── DOC_SENTIMENT.csv
└── figures/                     # Report figures (PNG)
```

## Output Tables

| Table | Description |
|---|---|
| `LIBRARY.csv` | Metadata for each book (title, author, genre, source URL) |
| `CORPUS_F1.csv` | MLCF: sentences indexed by (book_id, chapter, para_num, sent_num) |
| `TOKEN.csv` | Tokens with POS, stopword flag, stem, lemma, NRC emotions, TFIDF |
| `VOCAB.csv` | Unique terms with count, df, IDF, TFIDF stats, PCA loadings, NRC scores |
| `TFIDF.csv` | Document-term TFIDF matrix (books × terms) |
| `DOC_COMPONENTS.csv` | PCA: documents × 10 principal components |
| `LOADINGS.csv` | PCA: terms × component loadings |
| `DOC_TOPICS.csv` | LDA: documents × 10 topic concentrations |
| `TOPIC_TERMS.csv` | LDA: topics × term probabilities |
| `WORD2VEC.csv` | Word2vec: terms × 50-dimensional embeddings |
| `DOC_SENTIMENT.csv` | Per-book average NRC emotion and polarity scores |

## Visualizations

The exploration notebook produces 10 figures covering all required types:

1. Hierarchical cluster dendrogram (TFIDF cosine distance)
2. Heatmap: NRC emotion scores by novel
3. PCA scatter plot (PC0 vs PC1, colored by genre)
4. KDE: chapter-level polarity by genre
5. t-SNE of word2vec embeddings
6. LDA topic concentration heatmap
7. Scatter: fear vs trust by novel
8. Sentiment arc: Dracula
9. Sentiment arc: Frankenstein
10. Dispersion plot: Dracula
