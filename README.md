# Hotel Booking Demand Analysis

This project analyzes hotel-booking cancellations and compares logistic regression with a random-forest classifier.

## Context

- **Course:** Programming for Business Analytics, University of Vienna
- **Project format:** [TODO: group or individual project]

## Goal and research questions

The project examines:

1. How do booking characteristics differ between canceled and non-canceled bookings?
2. How do cancellation rates vary by deposit type and arrival month?
3. How do hotel type and month relate to average daily rate (ADR)?
4. How well can booking characteristics predict whether a booking is canceled?

## Data

- **Input:** `data/hotel_bookings.csv`
- **Source:** [TODO: document the original dataset source]
- **Raw size:** 119,390 rows and 32 columns.
- **Cleaned size:** 119,210 rows and 37 columns.
- **Target:** `is_canceled`.
- **Main variables:** hotel type, lead time, arrival year/month/week/day, weekend and weekday nights, guests, ADR, deposit type, previous cancellations, booking changes, market segment, distribution channel, customer type, and special requests.
- **License:** [TODO: document the dataset license or usage terms]

The repository ignores CSV files in `data/`, so the input data must be provided locally before running the notebooks.

## Methods

### Cleaning and feature engineering

The preprocessing notebook:

- fills missing `children`, `agent`, and `company` values with `0`, and missing `country` values with `"Unknown"`;
- removes 180 bookings with zero adults, children, and babies;
- creates `total_guests`, `total_nights`, `arrival_date`, `arrival_month`, and `arrival_weekday`;
- validates the cancellation target, guest counts, night counts, and arrival-date type; and
- saves `data/hotel_bookings_clean.csv`.

The raw data contains 129,425 missing values before these replacements and zero afterward.

### Exploratory analysis

The EDA includes descriptive statistics, cancellation comparisons, deposit-type and monthly cancellation rates, numeric and categorical distributions, a correlation matrix, and monthly ADR comparisons for City and Resort Hotels.

### Models and evaluation

The modeling workflow uses eight features: `lead_time`, `adr`, `stays_in_week_nights`, `stays_in_weekend_nights`, `total_guests`, hotel type, `previous_cancellations`, and `booking_changes`. Hotel type is encoded as `hotel_city`; the numeric features are standardized using training-set statistics.

The data is randomly split 80/20 with NumPy seed 42: 95,368 training rows and 23,842 test rows. The two models are:

- **Logistic regression**, used as an interpretable binary-classification baseline.
- **Random forest**, configured with 300 trees, `max_depth=15`, `min_samples_leaf=20`, and `random_state=42`.

The notebooks report test accuracy, AUC, recall for canceled bookings, a classification report for the random forest, ROC curves, and a random-forest confusion matrix. They also compare random-forest depths 8, 10, 12, and 15 using AUC.

## Key findings

- After cleaning, the dataset contains **119,210 bookings**. Mean ADR is **101.9691**, mean lead time is **104.1092 days**, mean total guests is **1.9712**, and mean total nights is **3.4262**.
- **62.9234%** of bookings were not canceled and **37.0766%** were canceled.
- Mean lead time was **80.0820 days** for non-canceled bookings and **144.8863 days** for canceled bookings.
- Monthly cancellation rates ranged from **30.5016%** in January to **41.4860%** in June; June was the highest month shown.
- The correlation between `lead_time` and `is_canceled` was **0.292876**. The correlations of ADR, total guests, and total nights with cancellation were **0.046492**, **0.044826**, and **0.018554**, respectively.
- Resort Hotel ADR was **186.7906** in August, compared with **114.8573** for City Hotel in August. City Hotel ADR varied less across the displayed months.
- Logistic regression achieved test accuracy **0.6966**, AUC **0.6277**, and cancellation recall **0.3572**.
- The random forest achieved test accuracy **0.7691**, AUC **0.7108**, and cancellation recall **0.4818**.
- For the random forest, class-1 precision, recall, and F1-score were **0.8259**, **0.4818**, and **0.6086**, respectively.
- The reported random-forest feature importances were: `lead_time` **0.3885**, `adr` **0.1867**, `previous_cancellations` **0.1849**, `booking_changes` **0.0975**, `stays_in_week_nights` **0.0486**, `hotel_city` **0.0335**, `total_guests` **0.0331**, and `stays_in_weekend_nights` **0.0273**.
- In the tested depth search, AUC values were **0.7833** at depth 8, **0.7979** at depth 10, **0.8114** at depth 12, and **0.8281** at depth 15.

## Limitations

- The dataset source and license are not documented in the notebooks or repository.
- The models use a single random 80/20 split rather than cross-validation.
- The cancellation classes are uneven, and the random forest cancellation recall is **0.4818**, so many cancellations remain undetected.
- The feature set and random-forest depth search are limited; the notebooks do not report threshold tuning, PR-AUC, log loss, or cost-sensitive evaluation.
- The descriptive statistics include an ADR minimum of **-6.38** and a maximum of **5,400**, which are not investigated further in the notebooks.

## How to run

### Requirements

Use Python with the packages listed in [`requirements.txt`](requirements.txt). The notebooks directly use:

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn`
- Jupyter Notebook

Place the raw file at `data/hotel_bookings.csv`. The preprocessing notebook writes `data/hotel_bookings_clean.csv`, which is required by the EDA and modeling notebooks.

### Notebook order

Run the final notebooks in this order:

1. [`01_preprocessing.ipynb`](notebooks/final/01_preprocessing.ipynb)
2. [`02_eda.ipynb`](notebooks/final/02_eda.ipynb)
3. [`03_modeling.ipynb`](notebooks/final/03_modeling.ipynb)

The combined notebook [`Hotel_Booking_Demand_Analysis.ipynb`](notebooks/final/Hotel_Booking_Demand_Analysis.ipynb) contains the preprocessing, EDA, and modeling workflow in one file. The notebooks in [`notebooks/original/`](notebooks/original/) are earlier versions.
