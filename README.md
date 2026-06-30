# Melbourne Housing Snapshot

**Link:** https://www.kaggle.com/datasets/dansbecker/melbourne-housing-snapshot

## About Dataset

Snapshot of Tony Pino's Melbourne Housing Dataset.

### Content

Scraped from publicly available results posted every week from Domain.com.au.
The dataset includes Address, Type of Real estate, Suburb, Method of Selling,
Rooms, Price, Real Estate Agent, Date of Sale, and distance from CBD.

### Notes on Specific Variables

- **Rooms**: Number of rooms
- **Price**: Price in dollars
- **Method**: S - property sold; SP - property sold prior; PI - property
  passed in; PN - sold prior not disclosed; SN - sold not disclosed;
  NB - no bid; VB - vendor bid; W - withdrawn prior to auction;
  SA - sold after auction; SS - sold after auction price not disclosed;
  N/A - price or highest bid not available.
- **Type**: br - bedroom(s); h - house, cottage, villa, semi, terrace;
  u - unit, duplex; t - townhouse; dev site - development site;
  o res - other residential.
- **SellerG**: Real Estate Agent
- **Date**: Date sold
- **Distance**: Distance from CBD
- **Regionname**: General Region (West, North West, North, North East, etc.)
- **Propertycount**: Number of properties that exist in the suburb
- **Bedroom2**: Scraped # of Bedrooms (from a different source)
- **Bathroom**: Number of Bathrooms
- **Car**: Number of carspots
- **Landsize**: Land Size
- **BuildingArea**: Building Size
- **CouncilArea**: Governing council for the area

## Project Overview

The goal of this project was to build a machine learning model that predicts
house prices using the Melbourne Housing Snapshot dataset, and to identify
which property characteristics have the strongest influence on price.

## Data Cleaning

During data preparation, the following steps were completed:

- Removed unnecessary technical columns
- Converted the sale date to the datetime format
- Replaced invalid zero values in area-related columns with missing values
- Replaced unrealistic values in the `year_built` column with missing values
- Checked for and removed duplicate records
- Checked for missing values in the target variable (`price`) before
  splitting the data
- Created new features (sale year, sale month, house age, missing value
  indicators, and suburb frequency encoding)

**Note on data leakage:** suburb frequency encoding is calculated *after*
the train/test split, using only the training data, and then mapped onto
both sets. This avoids leaking information about the test set's suburb
distribution into the training process.

## Exploratory Data Analysis (EDA)  

The analysis showed that house prices have a right-skewed distribution,
with most properties sold at lower prices and fewer very expensive homes.

The strongest relationships with price were found for the number of rooms,
bathrooms, and bedrooms. A high correlation was also found between `rooms`
and `bedrooms`, which suggests that these features contain similar
information — `bedrooms` was therefore dropped before modeling to avoid
redundancy.

## Model Performance

Three regression models were compared:

| Model              | MAE      | RMSE     | R²    |
|---------------------|---------:|---------:|------:|
| Dummy Regressor      | 461,258  | 630,259  | 0.000 |
| Linear Regression     | 257,177  | 374,051  | 0.648 |
| **Random Forest**     | **162,870** | **270,754** | **0.815** |

The Random Forest model produced the best results. It achieved the highest
R² score and the lowest prediction errors, making it the most accurate
model for this project.

*Results are based on a single train/test split (`random_state=42`),
without cross-validation.*

## Most Important Features

According to the Random Forest model, the most important factors affecting
house prices are:

- Region
- Number of rooms
- Distance from the city center
- Property type
- Land size

These results show that both the property's location and its main
characteristics have a strong impact on its price.

## Possible Improvements

The project could be improved in several ways:

- Try more advanced models such as XGBoost or LightGBM
- Create additional features that may better describe each property, such
  as the ratio of building area to land size or other useful combinations
  of existing features
- Log-transform the target variable (`price`), since its distribution is
  right-skewed — this could improve Linear Regression performance in
  particular
- Validate model stability using cross-validation instead of a single
  train/test split
