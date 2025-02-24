# Name Language of Origin

This project predicts the language of origin of a given name. The long-term goal was to enable DDIQ, Exiger’s AI-Based due diligence solution, to more accurately identify parts of names. This would improve name recognition and lessen false positives and negatives, helping Exiger better assess risks for clients.

The following delineates a high-level summary. A more detailed report is included [here](report.pdf).

## Installation

1. Clone the repo:

```bash
git clone git@github.com:shermanto24/name-language-of-origin.git
```

2. Set up and activate the environment:

```bash
conda env create -f environment.yml
```

## Data Overview

We primarily used the datasets provided by Exiger, which included Chinese, Indonesian, Japanese, Korean, Malay, Turkish, and Vietnamese names. We also used `company_person_name_dataset.csv`, which we obtained from Kaggle. Overall, we worked with **17** languages, totaling to over **292,000** names. Within the 17 languages, romanized and character-based names for languages like Chinese, Korean, and Japanese (CJK) were treated as separate. 64% of our samples were Japanese and 7.8% were English. 

## Methodology

The features we engineered include the following:
- `alphabet`
- `unigrams`, `bigrams`, `trigrams`
- `name_length`
- `avg_token_length`
- `num_tokens`
- `num_alphabets`
- `edit_distance`
- `accent_count`
- `transliteration`
- `period_freq`, `dash_freq`, `apostrophe_freq`, `space_freq`
- `lang_unigrams_cosine_sim`

Next, we employed 5 models:
1. Random forest
2. K-nearest neighbors
3. Multinomial Naive Bayes
4. Logistic regression
5. Gradient boosting

We chose **precision** as our primary method of evaluation to focus on reducing false positives, which is essential to our business context.

Lastly, the libraries we used included:
- `pandas`
- `scikit-learn`
- `numpy`
- `nltk`
- `unidecodedata`
- `matplotlib`
- `seaborn`

## Results

The languages with the highest precision were consistently **CJK**, followed by English, Turkish, and Vietnamese. In general, sample size correlated with performance, but there were exceptions. In particular, Vietnamese made up less than 1% of our data but performed similarly to Turkish, whose sample size was six times larger. We believe this is due to our `edit_distance` feature—the minimum number of operations needed to get from a name to its transliteration—having the most impact, at least on the random forest model.

K-nearest neighbors was our best performing model, with a **weighted average** test set precision of **90%**, demonstrating correlation between sample size and performance. In terms of **macro average** test set precision, however, random forest and logistic regression had the highest scores of **75%** and **74%** respectively. This highlights room for improvement for the languages with fewer samples.