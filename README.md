# `flair-lms`

This repository is part of the NLP research with
[`flair`](https://github.com/flairNLP/flair), a state-of-the-art NLP
framework from [Zalando Research](https://research.zalando.com/).

This repository will include various language models (forward and backward) that
can be used with `flair`. It will be updated frequently. So please star or watch
this repository 😅

# Changelog

**January 2020**: Move repository to the new [FlairNLP](https://github.com/flairNLP) group on GitHub.

September 2019: New Multilingual Flair Embeddings trained on JW300 corpus are released.

September 2019: All Flair Embeddings that are now officially available in
Flair >= *0.4.3* are listed.

# Parameters

All Flair Embeddings are trained with a `hidden_size` of 2048 and `nlayers` of 1.

# Flair Embeddings

| Language model   | # Tokens | Forward ppl. | Backward ppl. | Flair Embeddings alias
| ---------------- | -------- | ------------ | ------------- | -----------------------------
| Arabic           | 736M     | 3.39         | 3.45          | `ar-forward` and `ar-backward`
| Bulgarian (fast) | 66M      | 2.48         | 2.51          | `bg-forward-fast` and `bg-backward-fast`
| Bulgarian        | 111M     | 2.46         | 2.47          | `bg-forward` and `bg-backward`
| Czech (v0)       | 778M     | 3.44         | 3.48          | `cs-v0-forward` and `cs-v0-backward`
| Czech            | 442M     | 2.89         | 2.90          | `cs-forward` and `cs-backward`
| Danish           | 325M     | 2.62         | 2.68          | `da-forward` and `da-backward`
| Basque (v0)      |  37M     | 2.56         | 2.58          | `eu-v0-forward` and `eu-v0-backward`
| Basque (v1)      |  37M     | 2.64         | 2.31          | `eu-v1-forward` and `eu-v1-backward`
| Basque           |  57M     | 2.90         | 2.83          | `eu-forward` and `eu-backward`
| Persian          | 146M     | 3.68         | 3.66          | `fa-forward` and `fa-backward`
| Finnish          | 427M     | 2.63         | 2.65          | `fi-forward` and `fi-backward`
| Hebrew           | 502M     | 3.84         | 3.87          | `he-forward` and `he-backward`
| Hindi            |  28M     | 2.87         | 2.86          | `hi-forward` and `hi-backward`
| Croatian         | 625M     | 3.13         | 3.20          | `hr-forward` and `hr-backward`
| Indonesian       | 174M     | 2.80         | 2.74          | `id-forward` and `id-backward`
| Italian          | 1,5B     | 2.62         | 2.63          | `it-forward` and `it-backward`
| Dutch (v0)       | 897M     | 2.78         | 2.77          | `nl-v0-forward` and `nl-v0-backward`
| Dutch            | 1,2B     | 2.43         | 2.55          | `nl-forward` and `nl-backward`
| Norwegian        | 156M     | 3.01         | 3.01          | `no-forward` and `no-backward`
| Polish           | 1,4B     | 2.95         | 2.84          | `pl-opus-forward` and `pl-opus-backward`
| Slovenian (v0)   | 314M     | 3.28         | 3.34          | `sl-v0-forward` and `sl-v0-backward`
| Slovenian        | 419M     | 2.88         | 2.91          | `sl-forward` and `sl-backward`
| Swedish (v0)     | 545M     | 2.29         | 2.27          | `sv-v0-forward` and `sv-v0-backward`
| Swedish          | 671M     | 6.82 (?)     | 2.25          | `sv-forward` and `sv-backward`
| Tamil            |  18M     | 2.23         | 4509 (!)      | `ta-forward` and `ta-backward`

## Multilingual Flair Embeddings

Multilingual Flair Embeddings were trained on the recently released
[JW300](https://www.aclweb.org/anthology/P19-1310/) corpus. Thanks to half precision support in
Flair, both forward and backward Embeddings were trained for 5 epochs for over 10 days.
The training corpus has 2,025,826,977 token.

| Language model   | # Tokens | Forward ppl. | Backward ppl. | Flair Embeddings alias
| ---------------- | -------- | ------------ | ------------- | -----------------------------
| JW300            | 2B       | 3.25         | 3.37          | `multi-forward` and `multi-backward`


It can be loaded with:

```python
from flair.embeddings import FlairEmbeddings

jw_forward = FlairEmbeddings("multi-forward")
jw_backward = FlairEmbeddings("multi-backward")
```

A detailed evaluation on various PoS tagging tasks can be found in
[this repository](https://github.com/stefan-it/flair-pos-tagging).

We would like to thank Željko Agić for providing us access to the corpus
(before it was officially released)!