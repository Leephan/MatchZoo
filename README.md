<div align='center'>
<img src="./artworks/matchzoo-logo.png" width = "400"  alt="logo" align="center" />
</div>

# Matchzoo

> MatchZoo is a toolkit for text matching. It was developed with a focus on facilitating the designing, comparing and sharing of deep text matching models.

[![Python 3.6](https://img.shields.io/badge/python-3.6-blue.svg)](https://www.python.org/downloads/release/python-360/)
[![Documentation Status](http://readthedocs.org/projects/matchzoo/badge/?version=2.0)](https://matchzoo.readthedocs.io/en/2.0/?badge=2.0)
[![Build Status](https://travis-ci.org/faneshion/MatchZoo.svg?branch=master)](https://travis-ci.org/faneshion/MatchZoo/)
[![codecov](https://codecov.io/gh/faneshion/MatchZoo/branch/2.0/graph/badge.svg)](https://codecov.io/gh/faneshion/MatchZoo)
[![License](https://img.shields.io/badge/License-Apache%202.0-yellowgreen.svg)](https://opensource.org/licenses/Apache-2.0)
[![Requirements Status](https://requires.io/github/faneshion/MatchZoo/requirements.svg?branch=2.0)](https://requires.io/github/faneshion/MatchZoo/requirements/?branch=2.0)
---

## Get Started in 60 Seconds

First, import modules and prepare input data.

```python
from matchzoo import preprocessor
from matchzoo import generators
from matchzoo import models

train = [
    ("id0", "id1", "beijing", "Beijing is capital of China", 1),
    ("id0", "id2", "beijing", "China is in east Asia", 0),
    ("id0", "id3", "beijing", "Summer in Beijing is hot.", 1)
]
test = [
    ("id0", "id4", "beijing", "I visted beijing yesterday.")
]
```

Preprocess your input data in three lines of code, keep track parameters to be passed into the model.

```python
dssm_preprocessor = preprocessor.DSSMPreprocessor()
processed_tr = dssm_preprocessor.fit_transform(train, stage='train')
processed_te = dssm_preprocessor.fit_transform(test, stage='test')
# DSSM expect dimensionality of letter-trigrams as input shape.
# The fitted parameters has been stored in `context` during pre-processing.
dim_triletter = processed_train.context['dim_triletter']
```

Use Matchzoo `generators` module to generate `point-wise`, `pair-wise` or `list-wise` inputs into batches.

```python
generator_tr = generators.PointGenerator(processed_tr)
generator_te = generators.PointGenerator(processed_te)
# Example, train & test with the first batch
X_tr, y_tr = generator_tr[0]
X_te, y_te = generator_te[0]
```

Train a [Deep Semantic Structured Model](https://www.microsoft.com/en-us/research/project/dssm/), make predictions on test data.

```python
dssm_model = models.DSSMModel()
dssm_model.params['input_shapes'] = [(dim_triletter, ), (dim_triletter, )]
dssm_model.guess_and_fill_missing_params()
dssm_model.build()
dssm_model.compile()
dssm_model.fit([X_tr.text_left, X_tr.text_right], y_tr)
# Make predictions
predictions = dssm_model.predict([X_te.text_left, X_te.text_right])
```

For detailed usage, such as model persistence, evaluation, please check our documention: [English](https://matchzoo.readthedocs.io/en/2.0/?badge=2.0) [中文](https://matchzoo.readthedocs.io/zh/latest/)

If you're interested in the cutting-edge research progress, please take a look at [awaresome neural models-for semantic match](https://github.com/NTSC-Community/awaresome-neural-models-for-semantic-match).
