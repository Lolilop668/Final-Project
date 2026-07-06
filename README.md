# Predicting Airbnb Listing Prices in Major US Cities

CPE393: Introduction to Data Science with Python | Summer 2026 | KMUTT

## Summary

This project predicts the nightly price of an Airbnb listing from its property and location features. We use a public Kaggle dataset of listings across six major US cities and treat the task as a regression problem, where the target is `log_price` (the natural logarithm of the price). We explore the training data, clean and encode the features, and then train and compare four models: Linear Regression, Ridge Regression, Random Forest, and Gradient Boosting. The Random Forest works best and explains about two thirds of the price variation on a validation set. We then use it to generate price predictions for the unlabeled test file.

## Group

Group 15. Members: Mathieu, Serghei, and Samy. We are exchange students from ESIEA taking CPE393 at KMUTT.

Instructors: Ajan Aye Hninn Khine and Ajan Naveed Sultan.

## Dataset

AirBnB Listings in Major US Cities (Deloitte ML Competition), from Kaggle:
https://www.kaggle.com/datasets/rudymizrahi/airbnb-listings-in-major-us-cities-deloitte-ml

The dataset comes as two files. `train.csv` has 74,111 rows and includes the target `log_price`. `test.csv` has 25,458 rows and does not include the target, so it is used only to generate predictions, not to score the model. The data covers six cities: New York, Los Angeles, San Francisco, Washington DC, Chicago, and Boston. Each row is one listing, with numeric features (capacity, bedrooms, bathrooms, beds, reviews), categorical features (room type, property type, cancellation policy, city), booleans, and the latitude and longitude. The data is a snapshot from around 2017 and 2018, so the prices are not current.

## Repository structure

```
CPE_Project_Group15/
  README.md
  project_notebook.ipynb      # the full pipeline
  project_summary.pdf         # the 15-20 page report
  presentation_slides.pptx    # slides for the presentation
  requirements.txt
  data_link.txt               # download link for the dataset
  airbnb_predictions.csv       # model predictions on test.csv (created by the notebook)
```

## How to run the notebook

1. Download `train.csv` and `test.csv` from the Kaggle link above (a free account is needed) and place both in the same folder as the notebook. The files are too large to store in the repository.
2. Install the libraries (see below).
3. Open `project_notebook.ipynb` in Jupyter or Google Colab and run the cells from top to bottom.

The notebook loads both files, explores `train.csv`, cleans and encodes the data through one shared function, splits `train.csv` 80/20 into training and validation, trains and compares the four models, and finally applies the chosen model to `test.csv`. It saves the predictions to `airbnb_predictions.csv`. Everything is reproducible because the random state is fixed to 42.

If you use Colab, upload both CSV files to the session first. The loading cell checks a couple of common paths, so it should find them without any change.

Note: the notebook uses Folium for an interactive map. If Folium is not installed, run `pip install folium` or use the `requirements.txt` below.

## Requirements

Python 3 with the following libraries:

- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- folium

Install them with:

```bash
pip install -r requirements.txt
```

## Main results

We compared four models on a validation split taken from `train.csv`.

| Model | MAE | RMSE | R2 |
|-------|-----|------|-----|
| Random Forest | 0.285 | 0.396 | 0.695 |
| Gradient Boosting | 0.303 | 0.413 | 0.667 |
| Linear Regression | 0.368 | 0.489 | 0.535 |
| Ridge Regression | 0.368 | 0.489 | 0.535 |

The Random Forest is the best, with Gradient Boosting close behind and the two linear models tied. We kept the Random Forest as the final model because it had the best scores and it reports feature importances, which help explain the predictions. The features it relies on most are room type, the location coordinates, and the number of bathrooms.

Because `test.csv` has no true prices, we cannot compute metrics on it. We evaluate on the validation split, and we use the test file only to produce predicted prices, saved to `airbnb_predictions.csv`.

## Limitations

- The prices are from 2017 and 2018, so the model should not be used to set real prices today.
- The data covers only six large US cities, and NYC and LA make up about three quarters of it, so the model is weaker for the smaller cities and does not transfer elsewhere.
- An error of about 0.29 in log units means predictions are typically within roughly 30 percent of the true price. That is useful as a rough guide, not as an automatic pricing tool.
- We did not use the text columns (`amenities`, `description`), and we dropped the neighbourhood name in favor of the raw coordinates. Both are listed as future work.

## Responsible AI note

We removed host-related and free-text columns, so the model does not learn from personal data. We fitted all cleaning steps and encoders on the training data only, to avoid leakage, and we report metrics on a validation set the model was not trained on. We do not invent metrics for the unlabeled test file. AI assistants were used for debugging, code explanation, and grammar, and we can explain every step ourselves.
