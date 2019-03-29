# simon

This repository contains additional material for the paper "A Semantic Similarity-Based Perspective of Affect Lexicons for Sentiment Analysis".

## Additional material
The additional material for the paper can be found [here](additional_material.pdf).
It contains the results on all the datasets for the different WordNet-based similarity metrics considered.

## Implementation

The implementation of SIMON is included in the [_gsitk_ package](https://github.com/gsi-upm/gsitk).
An example of use follows.

_gsitk_ includes the implementation of the SIMON feature extractor.
To use it, two things are needed:
- A sentiment lexicon
- A word embeddings model that is _gensim_ compatible.

For example, using only the lexicon from [Bing Liu](https://dl.acm.org/citation.cfm?id=1014073) and a [embeddings model](https://code.google.com/archive/p/word2vec/) that is in the current directory:

```python
from gsitk.features import simon
from nltk.corpus import opinion_lexicon
from gensim.models.keyedvectors import KeyedVectors

lexicon = [list(opinion_lexicon.positive()), list(opinion_lexicon.negative())]

embedding_model = KeyedVectors.load_word2vec_format('GoogleNews-vectors-negative300.bin', binary=True)

simon_transformer = simon.Simon(lexicon=lexicon, n_lexicon_words=200, embedding=embedding_model)

# simon_transformer has the fit() and transform() methods, so it can be used in a Pipeline
```

To enhance performance, it is recommendable to use a more complete scikit-learn pipe that implements normalization and feature selection in conjuction with the SIMON feature extraction.

```python
from gsitk.features import simon

simon_model = simon.Simon(lexicon=lexicon, n_lexicon_words=200, embedding=embedding_model)
model = simon.simon_pipeline(simon_transformer=simon_model, percentile=25)

# model also implemtens fit() and transform()
```

## Cite

If you use this work, please cite the following paper:
Oscar Araque, Ganggao Zhu, Carlos A. Iglesias,
A semantic similarity-based perspective of affect lexicons for sentiment analysis,
Knowledge-Based Systems,
Volume 165,
2019,
Pages 346-359,
ISSN 0950-7051,
https://doi.org/10.1016/j.knosys.2018.12.005.
(http://www.sciencedirect.com/science/article/pii/S0950705118305926)
Keywords: Sentiment analysis; Sentiment lexicon; Semantic similarity; Word embeddings

```
@article{ARAQUE2019346,
title = "A semantic similarity-based perspective of affect lexicons for sentiment analysis",
journal = "Knowledge-Based Systems",
volume = "165",
pages = "346 - 359",
year = "2019",
issn = "0950-7051",
doi = "https://doi.org/10.1016/j.knosys.2018.12.005",
url = "http://www.sciencedirect.com/science/article/pii/S0950705118305926",
author = "Oscar Araque and Ganggao Zhu and Carlos A. Iglesias",
keywords = "Sentiment analysis, Sentiment lexicon, Semantic similarity, Word embeddings",
}
```
