# `flair-lms`

This repository is part of my NLP research with
[`flair`](https://github.com/zalandoresearch/flair), a state-of-the-art NLP
framework from [Zalando Research](https://research.zalando.com/).

This repository will include various language models (forward and backward) that
can be used with `flair`. It will be updated frequently. So please star or watch
this repository 😅

# Changelog

**September 2019**: All Flair Embeddings that are now officially available in
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

# Training tips

This sections covers data collection for training a language model. The
next section list some famous corpora and corresponding extraction
steps to build a "clean" text corpus.

## Corpora

### Leipzig Corpora Collection

The [Leipzig Corpora Collection](https://wortschatz.uni-leipzig.de/en/download)
provides sentence-segmented corpora for various domains (News, Webcrawl, Wikipedia).
Awesome resource!

### Europarl: http://www.statmt.org/europarl

The Europarl corpus consists of translated text from the European
parliament in several languages. It is very popular in the machine
translation community.

Texts for several languages can be found under <http://www.statmt.org/europarl>.
An extracted text can directly be used for training a language model.

### Wikipedia

Wikipedia dumps can be found under: <https://dumps.wikimedia.org>. Just
download the `<language>wiki-latest-pages-articles.xml.bz2` dump:

```bash
wget http://download.wikimedia.org/<language>wiki/latest/<language>wiki-latest-pages-articles.xml.bz2
```

Sadly it is a xml based dump (and no one really likes xml).

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
