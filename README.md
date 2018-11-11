# `flair-lms`

This repository is part of my NLP research with
[`flair`](https://github.com/zalandoresearch/flair), a state-of-the-art NLP
framework from [Zalando Research](https://research.zalando.com/).

This repository will include various language models (forward and backward) that
can be used with `flair`. It will be updated frequently. So please star or watch
this repository 😅

# Bulgarian

The Bulgarian language model was trained on various sources like Europarl,
Wikipedia or SETimes corpus. The training corpus is relatively small and has
a total size of 742M (66,701,618 token).

The training paramers were:

| Parameter         | Value
| ----------------- | -----
| `hidden_size`     | 2048
| `nlayers`         | 1
| `sequence_length` | 250
| `mini_batch_size` | 100

The forward model was trained for 30 epochs, resulting in a ppl of 2.48.
The backward model was trained for 31 epochs, resulting in a ppl of 2.51.

Downloads: [forward model](https://schweter.eu/cloud/flair-lms/lm-bg-small-forward-v0.1.pt) - [backward model](https://schweter.eu/cloud/flair-lms/lm-bg-small-backward-v0.1.pt).

# Slovenian

The Slovenian language model was trained on various sources like Europarl,
Wikipedia and OpenSubtitles2018. The training corpus is large and has a total
size of 1.8G (314,973,528 token).

| Parameter         | Value
| ----------------- | -----
| `hidden_size`     | 2048
| `nlayers`         | 1
| `sequence_length` | 250
| `mini_batch_size` | 100

The forward model was trained for 8 epochs, resulting in a ppl of 3.28.
The backward model was trained for 4 epochs, resulting in a ppl of 3.34.

Downloads: [forward model](https://schweter.eu/cloud/flair-lms/lm-sl-large-forward-v0.1.pt) - [backward model](https://schweter.eu/cloud/flair-lms/lm-sl-large-backward-v0.1.pt).

# Training tips

Useful tips for training a language model with `flair` will be coming soon!
