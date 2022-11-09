# Ancient Greek Word2Vec

This repository contains [Jupyter](https://jupyter.org) notebooks for training [word2vec](https://en.wikipedia.org/wiki/Word2vec) and querying models for Ancient Greek.

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

Theoretically you could use just about any language data in plaintext format as input. The data is contained in the `data/corpus` subdirectory, and consists of a set of plaintext files that end with the `.txt` extension. 

The format for this data is one-sentence per line. 

The data included in this repository is lemmatized, with 19053248 lemmas (check using `cat data/corpus/*.txt | wc -w` in the repository root).

It has been extracted from [LemmatizedAncientGreekXML](https://github.com/gcelano/LemmatizedAncientGreekXML). Thank you, [Giuseppe](https://github.com/gcelano)!

TODO: There are some zero-byte files in the corpus. Presumably, no lemmas were extracted for these files. This should be investigated in order to increase the corpus size.