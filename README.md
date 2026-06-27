# NLP Analysis of Social Text — r/ChangeMyView

**Author:** Pei Zheng · **Pathway:** Basic NLP Analysis

An NLP study of discourse on Reddit's [r/ChangeMyView](https://www.reddit.com/r/changemyview/) (CMV), a community where people post opinions they're willing to have challenged. This project compares the language of original posts (arguments) against the comments (responses) and analyzes sentiment patterns across both.

## Overview

The analysis uses two linked datasets from CMV and applies a full basic-NLP pipeline: data exploration, text cleaning, tokenization, comparative word-frequency analysis, and VADER sentiment analysis, with five visualizations and a written interpretation.

| Dataset | File | Records | Key fields |
|---------|------|---------|------------|
| Posts | `data/cmv_posts.csv` | ~5,000 | `title`, `selftext`, `score`, `num_comments`, `id`, `created_utc` |
| Comments | `data/cmv_comments.csv` | ~12,106 | `body`, `score`, `link_id` |

Comments link back to their parent post via `link_id` → `posts.id`.

## Repository Structure

```
hw5-nlp-analysis-social-text/
├── README.md                       # This file
├── requirements.txt                # Python dependencies
├── data/
│   ├── cmv_posts.csv               # CMV posts dataset
│   └── cmv_comments.csv            # CMV comments dataset
├── notebooks/
│   └── nlp_analysis.ipynb          # Full analysis notebook
├── output/
│   └── analysis_template.md        # Written interpretation template
└── example_approach/               # Optional RAG/LLM reference (not used in this pathway)
    ├── 1_cmv_scrape.py
    └── 2_llm_eval.ipynb
```

## Methods

The notebook (`notebooks/nlp_analysis.ipynb`) runs through five stages:

1. **Data loading & exploration** — shapes, columns, missing values, and basic statistics (average post and comment length in characters and words), plus distribution plots for post scores and comment lengths.
2. **Text preprocessing** — a `clean_text` function that lowercases, strips URLs, markdown links, HTML entities, numbers, and punctuation; then NLTK tokenization with English stopword removal.
3. **Comparative analysis** — top-20 word frequencies for posts vs. comments, side-by-side word clouds, vocabulary size and average word length, and set operations to find words unique to each corpus.
4. **Sentiment analysis** — NLTK's VADER compound scores applied to posts and comments, with distribution comparison, most positive/negative examples, sentiment-vs-engagement, and average sentiment over time.
5. **Interpretation** — written findings, an in-depth pattern discussion, social-science applications, and documented challenges.

## Key Findings

**Posts vs. comments use different vocabulary.** Words unique to posts skew toward proper nouns, technical terms, policies, and place names (e.g. specific drugs, public figures, foreign laws), reflecting that posts frame specific, often specialized opinions. Comments lean toward slang, informal expressions, and action words — the more immediate, conversational register people use when challenging a view.

**Sentiment is bimodal and slightly positive.** Most posts and comments cluster near the extremes (compound ≈ −1 or +1) or at neutral (0), rather than spreading evenly. Average tone leans neutral-to-positive, consistent with CMV's moderation norms for civil discussion. In short, when users hold a clear opinion they tend to express it strongly.

**Collective sentiment shifts over time.** Average monthly post sentiment is not static — it dips into more negative territory in some periods while generally staying above 0. This suggests aggregate mood on the subreddit tracks the broader social context (e.g. periods of economic, health, or political stress) rather than being purely random or individual.

## Visualizations

The notebook produces five figures:

1. Distributions of post scores and comment lengths (characters and words)
2. Side-by-side word clouds for posts and comments
3. Sentiment score distribution: posts vs. comments
4. Post sentiment vs. post score (engagement)
5. Average post sentiment over time (monthly)

## Social Science Relevance

This kind of analysis can surface shared social concerns and shifts in popular language over time. Comparing sentiment and vocabulary between initial posts and successful counter-arguments offers a way to study "what a persuasive argument looks like." More broadly, it shows that online communities can be traced and analyzed at a macro level — which is useful for research, but also raises questions about how such insights could be used to influence audiences.

## Setup & Usage

```bash
# (recommended) create an environment
conda create -n nlp-hw5 python=3.9 && conda activate nlp-hw5

# install dependencies
pip install -r requirements.txt

# launch the notebook
jupyter notebook notebooks/nlp_analysis.ipynb
```

The notebook downloads the required NLTK resources (`punkt`, `stopwords`, `vader_lexicon`) automatically on first run.

## Challenges & Notes

The main challenge was analytical rather than technical: moving past descriptive statistics to find patterns and connect them to a wider social context, which led to the collective-sentiment-over-time insight. A secondary decision was whether to log-transform the data; after inspecting the raw distributions, a log transform was deemed unnecessary, keeping the sentiment scores on their original scale for clarity.

Built with `pandas`, `numpy`, `matplotlib`, `seaborn`, `nltk`, `scikit-learn`, and `wordcloud`.

## References

- CMV dataset paper: https://arxiv.org/abs/1602.01103
- NLTK VADER sentiment: https://www.nltk.org/
- r/ChangeMyView: https://www.reddit.com/r/changemyview/
