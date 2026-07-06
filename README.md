# Predicting Airbnb Listing Prices in Major US Cities

CPE393: Introduction to Data Science with Python | Summer 2026 | KMUTT

## Summary

This project predicts the nightly price of an Airbnb listing from its property and location features. We use a public Kaggle dataset of 74,111 listings across six major US cities and treat the task as a regression problem, where the target is `log_price` (the natural logarithm of the price). We clean the data, explore it, encode and scale the features, and then train and compare three models: Linear Regression, Ridge Regression, and a Random Forest. The Random Forest works best and explains about two thirds of the price variation on data it has never seen.

## Group

Group 15 K/B. Members: Mathieu, Serghei, and Samy.

Professors : Ajan Aye Hninn Khine and Ajan Naveed Sultan.

## Dataset

AirBnB Listings in Major US Cities (Deloitte ML Competition), from Kaggle:
https://www.kaggle.com/datasets/rudymizrahi/airbnb-listings-in-major-us-cities-deloitte-ml

The file is a single CSV with 74,111 rows and 29 columns. It covers six cities: New York, Los Angeles, San Francisco, Washington DC, Chicago, and Boston. Each row is one listing, with numeric features (capacity, bedrooms, bathrooms, beds, reviews), categorical features (room type, property type, cancellation policy, city), booleans, and the latitude and longitude. The target is `log_price`. The data is a snapshot from around 2017 and 2018, so the prices are not current.

## Repository structure

```
CPE_Project_Group15/
  README.md
  project_notebook.ipynb      # the full pipeline
  project_summary.pdf         # the 15-20 page report
  presentation_slides.pdf     # slides for the presentation
  requirements.txt
  data_link.txt               # download link for the dataset
```

## How to run the notebook

1. Download `train.csv` from the Kaggle link above (a free account is needed) and place it in the same folder as the notebook. The dataset is too large to store in the repository, so it is not uploaded here.
2. Install the libraries (see below).
3. Open `project_notebook.ipynb` in Jupyter or Google Colab and run the cells from top to bottom.

The notebook loads `train.csv`, splits it 80/20 into training and test sets with a fixed random seed, saves those two files, and then runs the cleaning, EDA, modeling, and evaluation. Everything is reproducible because the random state is fixed.

If you use Colab, upload `train.csv` to the session first. The loading cell looks for the file in a few common locations, so it should find it without any change.

## Requirements

Python 3 with the following libraries:

- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn

Install them with:

```bash
pip install -r requirements.txt
```

or directly:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```

## Main results

We compared three models on a held-out test set that the model never saw during training.

| Model | MAE | RMSE | R2 |
|-------|-----|------|-----|
| Random Forest (test) | 0.287 | 0.399 | 0.691 |
| Ridge Regression | 0.368 | 0.491 | 0.539 |
| Linear Regression | 0.368 | 0.491 | 0.539 |

The Random Forest is the clear winner. The features it relies on most are room type, the location coordinates, and the number of bathrooms, which matches what the exploratory analysis showed. The two linear models land in almost the same place, because with scaled features the Ridge penalty barely changes the fit.

## Limitations

- The prices are from 2017 and 2018, so the model should not be used to set real prices today.
- The data covers only six large US cities, and NYC and LA make up about three quarters of it, so the model is weaker for the smaller cities and does not transfer elsewhere.
- An error of about 0.29 in log units means predictions are typically within roughly 30 percent of the true price. That is useful as a rough guide, not as an automatic pricing tool.
- We did not use the text columns (`amenities`, `description`), and we dropped the neighbourhood name in favor of the raw coordinates. Both are listed as future work.

## Responsible AI note

We removed host-related and free-text columns, so the model does not learn from personal data. We kept the test set separate before making any cleaning decision to avoid data leakage, and we report the metrics honestly rather than showing only the best score. AI assistants were used for debugging, code explanation, and grammar, and we can explain every step ourselves.
