# Ancient Greek Word2Vec

This repository contains [Jupyter](https://jupyter.org) notebooks for training [word2vec](https://en.wikipedia.org/wiki/Word2vec) and querying models for Ancient Greek.

## Dependencies

You will need to install [Gensim](https://radimrehurek.com/gensim/models/word2vec.html) to use the notebooks in this repo. Both notebooks attempt to install it in case it is missing:

```python
try:
    from collections.abc import Mapping
    from gensim.models.word2vec import Word2Vec
except:
    !pip install -I gensim
    from collections.abc import Mapping
    from gensim.models.word2vec import Word2Vec
```

Before running these scripts, therefore, you should ensure you are working in a virtual env using [Conda](https://docs.conda.io/en/latest/) or some alternative.

## Get most similar terms

The notebook `query_w2v_model.ipynb` contains code to load up an existing model and query it. Once the code runs successfully through, you can query additional terms by imitating the example code.

For example, querying 'ἀρχιερεύς' ('high priest') using `model.most_similar('ἀρχιερεύς', topn=10)` generates the following results:

```python
[('ἱερεύς', 0.6741834878921509), # 'priest' is 67% similar
 ('πατριάρχης', 0.6512661576271057), # 'patriarch' is 65% similar
 ('διάκονος', 0.6433295011520386), # 'deacon'/'servant' is 64% similar
 ('ἰούδας', 0.6175312399864197), # 'Jude'/'judas' is 61% similar, etc.
 ('ἀρχιερωσύνη', 0.6078990697860718),
 ('χρηματίζω', 0.5876879096031189),
 ('χειροτονέω', 0.577594518661499),
 ('ἀαρών', 0.5753183364868164),
 ('πιλᾶτος', 0.572640061378479),
 ('ἀναγορεύω', 0.5716601610183716)]
```

## Train your own model

The notebook `build_greek_w2v_model.ipynb` contains code to train your own model, including specifying a data directory and setting hyperparameters such as vector size, context window size, etc.

You can also change the neural network type (either skip-gram or continuous bag-of-words). Anecdotally, I find the CBOW to work better for lemmatized comparisons. For example, for the term λόγος ('word'/'reason'/'account'/etc.), compare the following results:

```python
# Skip-gram

[('ἀξιομνημόνευτος', 0.6042230129241943),
 ('ἐφεκτέον', 0.6000658869743347),
 ('ψευδοδοξία', 0.589237630367279),
 ('λογοποιία', 0.5858883261680603),
 ('ἐξεταστικός', 0.5815737247467041),
 ('ἀκριβολογία', 0.5795413851737976),
 ('ὑφήγησις', 0.5784241557121277),
 ('ἀναμφίλεκτος', 0.5765267014503479),
 ('ἰσοσθένεια', 0.5729403495788574),
 ('εἰσαγωγικός', 0.5722008347511292)]

# CBOW

[('ἑρμηνεία', 0.5288925766944885),
 ('ἐξήγησις', 0.5065293312072754),
 ('θεολογία', 0.4956413805484772),
 ('διδασκαλία', 0.48970848321914673),
 ('ὑπόληψις', 0.47546130418777466),
 ('διήγησις', 0.47397667169570923),
 ('σκέψις', 0.47282832860946655),
 ('θεωρία', 0.47240814566612244),
 ('διαλέγω', 0.4657094478607178),
 ('φυσιολογία', 0.4602492153644562)]

```

The skip-gram approach may work better with a higher `min_count_input` (10? 100?) to exclude rare lemmas.

Theoretically you could use just about any language data in plaintext format as input. The data is contained in the `data/corpus` subdirectory, and consists of a set of plaintext files that end with the `.txt` extension. 

The format for the input data is one-sentence per line. 

## Corpora

The data included in this repository is lemmatized, with 19,053,248 lemmas in `data/corpus` (check using `cat data/corpus/*.txt | wc -w` in the repository root).

The files under `data/papyri` includes 1,759,488 lemmas (check using `find data/papyri -type f -name "*.txt" -exec cat {} + | wc -w` to avoid the 'argument list too long' error).

This data has been extracted from [LemmatizedAncientGreekXML](https://github.com/gcelano/LemmatizedAncientGreekXML) and [MALP](https://github.com/gcelano/MALP). Thank you, [Giuseppe](https://github.com/gcelano)!

TODO: There are some zero-byte files in the corpus. Presumably, no lemmas were extracted for these files. This should be investigated in order to increase the corpus size.

TODO: Add [ngram](https://radimrehurek.com/gensim/models/word2vec.html#embeddings-with-multiword-ngrams) detection preprocessing option

TODO: Add FastText sub-word model