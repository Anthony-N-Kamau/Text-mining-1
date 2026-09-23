# Text Mining: Aspect-Based Sentiment Analysis of Computer Reviews

This repository contains an R Markdown project that walks through a text-mining
pipeline on a dataset of computer/monitor reviews annotated with aspect-based
sentiment tags. The analysis covers data loading, text preprocessing,
visualization, regular expressions, and deriving an overall sentiment score
per review.

## Overview

The dataset (`computer.txt`) contains 531 customer reviews of computers,
pre-annotated with aspect-level sentiment tags in the form:

```
screen[-1], picture quality[-1] ## When the screen wasn't contracting or glitching...
```

Each tag (e.g. `screen[-1]`) marks an aspect mentioned in the review (e.g.
"screen") and a sentiment score for that aspect (`-1` negative, `+1`
positive). The `##` delimiter separates the aspect tags from the plain-text
review itself.

The project uses this structure to:

1. Explore raw term frequencies via word clouds and bar plots
2. Clean the text (stop word removal, stemming)
3. Use regular expressions to search, extract, and parse the aspect tags
4. Aggregate the aspect-level scores into a single sentiment score and label
   (`positive` / `negative` / `neutral`) per review

## Contents

| Step | Description |
|---|---|
| **Load data** | Read `computer.txt` into a tibble with one review per row |
| **Word cloud** | Visualize raw term frequencies with `wordcloud` |
| **Frequency plot** | Tokenize reviews with `unnest_tokens` and plot the top 30 most frequent words |
| **Remove stop words** | Filter out common function words using `tidytext::stop_words` via `anti_join` |
| **Adjust stopword list** | Customize the stopword lexicon (keep domain-relevant words, add custom stop words) |
| **Stemming** | Collapse word variants to their root form using `SnowballC::wordStem` (Porter's algorithm) |
| **Regular expressions** | Search/extract/subset/match reviews using patterns (`str_detect`, `str_extract`, `str_subset`, `str_match`, `str_extract_all`) |
| **Parsing aspect tags** | Split each review into `cleaned_review` (plain text) and `aspect_sentiment` (aspect tags) using regex, including lookahead/lookbehind for a cleaner split |
| **Sentiment scoring** | Sum the `+N`/`-N` scores per review and classify each as positive, negative, or neutral |

## Key findings

- The dataset is dominated by **monitor/screen quality** complaints, along
  with frequent mentions of **price**, **Acer**, and **computer** — this is
  visible in both the word cloud and frequency plots even before cleaning.
- Out of 531 reviews: **105** mention "monitor", **5** mention "memory", and
  **3** mention "delivery" — monitors are clearly the dominant topic.
- The most positive review in the dataset (by summed sentiment score) is
  about an Acer Netbook Aspire One, praising its build quality, screen, and
  keyboard despite a delayed delivery.

## Requirements

This project uses R with the following packages:

```r
install.packages(c("tidyverse", "tidytext", "tm", "wordcloud", "SnowballC"))
```

- `tidyverse` — data wrangling and plotting (`dplyr`, `ggplot2`, `readr`, `purrr`, `stringr`)
- `tidytext` — tokenization and the built-in stop word lexicon
- `tm` — text mining utilities
- `wordcloud` — word cloud visualization
- `SnowballC` — Porter stemming algorithm

## Repository structure

```
.
├── text-mining.Rmd      # R Markdown source
├── computer.txt          # Raw review dataset (not included / add locally)
└── README.md
```

> **Note:** `computer.txt` is expected in the working directory when knitting.
> If it isn't included in this repo, add it locally before running the
> analysis.

## Usage

Open `text-mining.Rmd` in RStudio and knit to PDF or HTML:

```r
rmarkdown::render("text-mining.Rmd")
```
