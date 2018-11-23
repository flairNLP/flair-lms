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

Downloads:

```bash
wget https://schweter.eu/cloud/flair-lms/lm-bg-small-forward-v0.1.pt
wget https://schweter.eu/cloud/flair-lms/lm-bg-small-backward-v0.1.pt
```

# Slovenian

The Slovenian language model was trained on various sources like Europarl,
Wikipedia and OpenSubtitles2018. The training corpus is large and has a total
size of 1.8G (314,973,528 token).

The training paramers were:

| Parameter         | Value
| ----------------- | -----
| `hidden_size`     | 2048
| `nlayers`         | 1
| `sequence_length` | 250
| `mini_batch_size` | 100

The forward model was trained for 8 epochs, resulting in a ppl of 3.28.
The backward model was trained for 4 epochs, resulting in a ppl of 3.34.

Downloads:

```bash
wget https://schweter.eu/cloud/flair-lms/lm-sl-large-forward-v0.1.pt
wget https://schweter.eu/cloud/flair-lms/lm-sl-large-backward-v0.1.pt
```

# Dutch

The Dutch language model was trained on various sources like Europarl,
Wikipedia and OpenSubtitles2018. The training curpus is large and has a
total size of 4.9G (897,298,291 token).

The training paramers were:

| Parameter         | Value
| ----------------- | -----
| `hidden_size`     | 2048
| `nlayers`         | 1
| `sequence_length` | 250
| `mini_batch_size` | 200 (different to Bulgarian and Slovenian)

The forward model was trained for 2 epochs, resulting in a ppl of 2.78.
The backward model was trained for 2 epochs, resulting in a ppl of 2.77.

Downloads:

```bash
wget https://schweter.eu/cloud/flair-lms/lm-nl-large-forward-v0.1.pt
wget https://schweter.eu/cloud/flair-lms/lm-nl-large-backward-v0.1.pt
```

# Swedish

The Swedish language model was trained on various sources like Europarl,
Wikipedia and OpenSubtitles2018. The training curpus is large and has a
total size of 3.3G (545,749,244 token).

The training paramers were:

| Parameter         | Value
| ----------------- | -----
| `hidden_size`     | 2048
| `nlayers`         | 1
| `sequence_length` | 250
| `mini_batch_size` | 200 (like Dutch)

The forward model was trained for 2 epochs, resulting in a ppl of 2.29.
The backward model was also trained for 2 epochs, resulting in a ppl of
2.27.

Downloads:

```bash
wget https://schweter.eu/cloud/flair-lms/lm-sv-large-forward-v0.1.pt
wget https://schweter.eu/cloud/flair-lms/lm-sv-large-backward-v0.1.pt
```

# Training tips

This sections covers data collection for training a language model. The
next section list some famous corpora and corresponding extraction
steps to build a "clean" text corpus.

## Corpora

### Europarl: http://www.statmt.org/europarl

The Europarl corpus consists of translated text from the European
parliament in several languages. It is very popular in the machine
translation community.

Texts for several languages can be found under <http://www.statmt.org/europarl>.
An extracted text can directly be used for training a language model.

### Wikipedia

Wikipedia dumps can be found under: <https://dumps.wikimedia.org>. Just
download the `<language>wiki-latest-pages-articles.xml.bz2` dump. Sadly
it is a xml based dump (and no one really likes xml).

Thus, an extractor tool is needed to extract Wikipedia articles from
stupid xml to plain text. I recommend the wiki extractor from
<https://github.com/attardi/wikiextractor>. Just clone that repository
or download the tool directly via:

```bash
wget https://raw.githubusercontent.com/attardi/wikiextractor/master/WikiExtractor.py
```

Then run the `WikiExtractor` using:

```bash
python3 WikiExtractor.py -c -b 25M -o extracted <language>wiki-latest-pages-articles.xml.bz2
```

This will create `bz2` archives with plain-text wikipedia articles.

Do extract these smaller chunks just use:

```bash
find extracted -name '*bz2' \! -exec bzip2 -k -c -d {} \; > <language>wiki.xml
```

All wikipedia articles are then stored in `<language>wiki.xml`. Only
four preprocessing steps are needed after extraction:

* Strip some stupid xml tags with: `sed -i 's/<[^>]*>//g' <language>wiki.xml`
* Remove empty lines with: `sed -i '/^\s*$/d' <language>wiki.xml`
* Remove the temporarily created extraction folder: `rm -rf extracted`
* Rename the corpus from xml to txt: `mv <language>wiki.xml <language>wiki.txt`

Your final cleaned plain-text corpus is then located under `<language>wiki.txt`.

### OpenSubtitles2018

Many subtitles from various movies in several languages could be found
on the [OPUS](http://opus.nlpl.eu/) webpage.

Just find out the language code for your desired language and download
an open subtitles dump:

```bash
wget "http://opus.nlpl.eu/download.php?f=OpenSubtitles2018%2F<language>.raw.tar.gz"
```

Unfortunately, all subtitles are stored in a stupid xml format. But first,
extract the archives with:

```bash
find . -name '*.gz' -exec gunzip '{}' \;
```

Then concatenate all xml files into one large xml file:

```bash
find OpenSubtitles2018/raw/<language>/ -name *.xml -exec cat {} + > opensubtitles-combined.xml
```

Instead of using some stupid xml parsing, just throw away all stupid
xml tags:

```bash
cat opensubtitles-combined.xml | grep -v "<" > opensubtitles-combined.txt
```

In the final step, just delete the old xml file:

```bash
rm opensubtitles-combined.xml
```

### SETimes

The SETimes corpus contains newspaper articles that have been translated
into several Balkan languages. The corpus can be found under
<http://nlp.ffzg.hr/resources/corpora/setimes/>. Just extract them
and they can be used directly to train a language model.

Just make sure that you have downloaded the plain text corpus and
**not** the Moses tokenized texts.

## Tokenization

Notice: **do not** use tokenized or preprocessed corpora, and **do not**
tokenize or preprocess the data. Plain text is all you need :)

## Preparations for training

After you collected all dataset, concatenate it like e.g. `cat *.txt >> <language>.dataset.txt`.

It is also very important to **shuffle** your data! This can be done
with the `shuf` command:

```bash
cat <language>.dataset.txt | shuf > <language>.dataset.shuffled.txt
```

## Dataset split

One strategy for splitting the shuffled dataset into training,
development and test sets could be the following:

* Count the number of lines in your shuffled dataset with: `wc -l <language>.dataset.shuffled.txt`
* Use 1/500 as development and 1/500 as test set
* Use `split` command with `-l` option
* The folder structure must be:

  ```
  .
  ├── test.txt
  ├── train
  │   ├── xaa
  │   ├── ...
  │   └── xzz
  └── valid.txt
  ```
