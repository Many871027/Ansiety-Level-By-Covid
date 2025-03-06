## Predicting Nurse Anxiety Levels During COVID-19: A Data-Driven Approach

This project investigates the factors associated with anxiety levels among nurses during the COVID-19 pandemic in 2020. Using a dataset of 140 nurses, where anxiety was measured subjectively, we employ a combination of data preprocessing, exploratory data analysis (EDA), statistical hypothesis testing, and machine learning to build a predictive model for anxiety levels. This README outlines the key steps and findings of the project.

**Project Goals:**

*   Understand the distribution of anxiety levels among the nurses in the sample.
*   Identify demographic and work-related factors significantly associated with anxiety.
*   Develop a predictive model to classify nurses into different anxiety levels (Minimal, Mild, Moderate, Severe).
*   Gain insights that could potentially inform interventions to support nurse well-being.

**Dataset:**

The dataset comprises subjective anxiety level measurements from a sample of 140 nurses during the COVID-19 pandemic in 2020.  The key variable, `anxiety_level`, is an ordinal categorical variable with four levels:

*   0: Minimal
*   1: Mild
*   2: Moderate
*   3: Severe

In addition to the anxiety level, the dataset includes the following demographic and work-related variables:

*   `sex`:  Binary (0: Female, 1: Male)
*   `education_level`: Ordinal (0: Technical Level, 1: Bachelor, 2: Graduate)
*   `shift`: Categorical (Morning, Afternoon, Night A, Night B) - One-hot encoded.
*   `marital_status`: Categorical (Married, Single, Domestic Partnership, Widowed) - One-hot encoded, with 'Widowed' grouped with 'Single' due to low frequency.
*   `category`: Categorical (Gen Nurse, Nurse Aux, Spec Nurse, Head Nurse) - One-hot encoded, with 'Spec Nurse' and 'Head Nurse' grouped into 'Other' due to low frequency.
*   `age_range`: Ordinal (0: 20-29, 1: 30-39, 2: 40-49, 3: 50 and over)
*   `seniority_range`: Ordinal (0: 1-5 years, 1: 6-10 years, 2: 11-15 years, 3: 16-20 years, 4: 21+ years)

**Tools and Technologies:**

*   **Programming Language:** Python
*   **Data Manipulation:** pandas
*   **Data Visualization:** matplotlib, seaborn, plotly
*   **Statistical Analysis:** scipy.stats, scikit-posthocs
*   **Machine Learning:** scikit-learn, xgboost
*   **Data Augmentation:** imbalanced-learn (SMOTE)

**Project Structure and Methodology:**

1.  **Data Loading and Preprocessing:**
    *   The dataset is loaded using pandas.
    *   Variables are encoded appropriately: ordinal encoding for ordinal variables (`anxiety_level`, `education_level`, `age_range`, `seniority_range`), binary encoding for `sex`, and one-hot encoding for nominal categorical variables (`shift`, `marital_status`, `category`).
    *   Low-frequency categories are grouped to avoid issues with statistical tests and model training.

2.  **Feature Engineering:**
    *   Interaction terms are created between key variables to capture potential non-additive effects.  This includes interactions between:
        *   Numerical/ordinal variables (e.g., `age_x_seniority`).
        *   `sex` and numerical/ordinal variables.
        *   One-hot encoded variables and numerical/ordinal variables.
        *  One-hot encoded variables between them.

3.  **Data Augmentation (SMOTE):**
    *   The Synthetic Minority Oversampling Technique (SMOTE) is applied to address class imbalance in the `anxiety_level` variable. This is *crucial* for building robust classification models, as it prevents the model from being biased towards the majority classes. SMOTE creates synthetic samples of the minority classes, resulting in a balanced dataset.

4.  **Exploratory Data Analysis (EDA) and Correlation Analysis:**
    *   Descriptive statistics and visualizations (histograms, bar plots) are used to understand the distribution of individual variables.
    *   Spearman rank correlation is used to assess the *monotonic* relationships between the ordinal `anxiety_level` variable and other numerical/ordinal predictors. This provides an initial indication of potential associations.  *However*, it's important to note that all observed Spearman correlations were weak, suggesting that no single variable strongly predicts anxiety.

5.  **Statistical Hypothesis Testing:**
    *   **Kruskal-Wallis Test:**  A non-parametric test used to compare the distributions of numerical/ordinal variables across the different levels of `anxiety_level`.  Dunn's test is used for post-hoc pairwise comparisons when Kruskal-Wallis is significant.
    *   **Chi-square Test:** Used to assess the association between `anxiety_level` and categorical variables (including one-hot encoded variables). Cramer's V is calculated to measure the strength of association.
    * These tests confirm the statistical significance of several associations, even with weak correlations, highlighting the importance of inferential testing.

6.  **Model Training and Evaluation:**
    *    The dataset is splitted in Train and Test sets, using stratification.
    *   Several classification models are trained and evaluated, Gradient Boosting perform best.

**Key Findings:**

*   **Weak Individual Predictors:**  No single variable strongly predicts anxiety levels, as evidenced by the weak Spearman correlations.
*   **Significant Associations:** Despite the weak individual correlations, statistical tests (Kruskal-Wallis and Chi-square) revealed significant associations between `anxiety_level` and several factors:
    *   `sex`
    *   `age_range`
    *   `seniority_range`
    *   `shift` (all categories)
    *   `marital_status` (all categories)
    *   `category` (all categories)
    *   Several *interaction* terms (e.g., `age_x_seniority`, `sex_x_seniority`, interactions with `shift` and `category`)
*   **Non-Significant Association:**  `education_level`, when considered alone, did *not* show a significant association with anxiety. However, interactions *involving* education were significant.
* **Importance of Interactions:** The significance of interaction terms indicates that the relationships are complex and non-additive.

**Model Performance:**

*   After data augmentation, feature engineering, and hyperparameter tuning, Gradient Boosting achieved the best Test Accuracy of 0.725.
