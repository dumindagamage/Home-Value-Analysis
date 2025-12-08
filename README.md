# ![CI logo](https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png)

---

# King County Housing Analytics 🏠

![](https://github.com/dumindagamage/Home-Value-Analysis/blob/wip/resources/images/home_analysis_banner.png?raw=true)

---
## Project Overview

This Data Analytics project analyzes housing sales data in King County, USA (May 2014 - May 2015). The goal is to provide actionable insights for two distinct user groups: **Buyers** looking for value and affordability, and **Sellers** aiming to maximize their profit. 

The project culminates in an interactive Streamlit dashboard that allows users to explore market trends, assess property value based on condition and location, and predict house prices using a Machine Learning model.

## Executive Summary
**Client:** King County Real Estate Agency.  
**Goal:** The client wants to launch a digital dashboard to assist two user groups:
1. **Buyers:** Finding undervalued properties and understanding affordability.
2. **Sellers:** Determining the optimal listing time and renovation strategy.

## Dataset Content
The project uses the **King County House Sales dataset** (`kc_house_data.csv`).
* **Source:** [Kaggle - House Sales in King County, USA](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction)
* **Volume:** ~21,613 records (from May 2014 - May 2015)
* **Features:** 21 variables including Price (Target), Bedrooms, Bathrooms, Sqft Living, Floors, Waterfront, View, Condition, Grade, Zipcode, and Year Built.

## Business Requirements
### 🏷️ User Group 1: Buyers
* **Affordability:** Identify the top 10 most affordable zip codes.
* **Value Assessment:** Quantify the premium for "Waterfront" properties.
* **Feature Importance:** Determine house grade vs condition impact on price.
* **Prediction:** Estimate fair market value to make competitive offers.

### 💰 User Group 2: Sellers
* **Feature Value:** Determine which specific home attributes contribute most significantly to property valuation
* **ROI Analysis:** Determine if renovations yield a statistically significant return.
* **Timing:** Identify the best month to sell for maximum profit.
* **Listing Strategy:** Set optimal prices based on neighborhood trends.

## Project Hypothesis and Validation

The following hypotheses were posed at the project's inception and validated through the data analysis process.

### Hypothesis 1: There are distinct geographic clusters (zip codes) within King County that are significantly more affordable than the county median.
* **Validation:** Analysis of median price per zip code visualized via bar charts.
* **Outcome:** **Confirmed**. The analysis identified specific zip codes (e.g., 98002, 98168) where the median price is less than half of the county average, validating the "Affordability" user story.

### Hypothesis 2: Waterfront properties are significantly more expensive than non-waterfront properties.
* **Validation:** Since price is non-normal (NB1 outcome), we use the Mann-Whitney U Test.
* **Outcome:** **Confirmed**. Waterfront homes command a considerable premium. This confirms that the view is a statistically significant value driver.

### Hypothesis 3: Construction Grade has a stronger causal impact on price than Condition, implying buyers pay for "bones" over "paint."
* **Validation:** Heatmap Interaction Analysis & Correlation Comparison.Use Spearman Correlation because Grade/Condition are ordinal categories
* **Outcome:** **Confirmed**.Grade (~0.65) is a far stronger correlation than Condition (~0.04).

### Hypothesis 4: Sales prices follow a seasonal trend, peaking in Spring/Summer, suggesting an optimal window for sellers.
* **Validation:** Time-series analysis of `Median Price` by `Month Sold`.
* **Outcome:** **Confirmed**. The data shows a visible trend where median prices and sales volume tick upward starting in April/May, supporting the advice for sellers to list during these months for maximum potential profit.

### Hypothesis 5: The value of renovation is not uniform; it provides a significantly higher ROI for Mid-Century homes (1950-1990) than for pre-war or modern homes.
* **Validation:**  Interaction Plot (Age x Renovation).
* **Outcome:** **Confirmed**. It confimrmed that the ROI varies significantly by era as Mid-Century (1950-1990) shows ~60% ROI
* **TODO:** We need to perform a statistical test confirms that these observed differences (60% vs 30%) are not just due to random chance.


## Project Management
This project was managed using Agile methodologies with a **GitHub Project Board**.
* **Kanban Board:** [Link to your GitHub Project Board](https://github.com/users/dumindagamage/projects/5/views/1)


## Project Plan
### 1. High-Level Steps
The analysis was executed in four distinct phases:

* **Phase 1: Business Understanding & Data Collection**
    * Defined the scope: Help Buyers find value and Sellers maximize profit.
    * Acquired the King County Housing dataset (21k+ records) from Kaggle.
    * Established core hypotheses regarding location, condition, and renovation value.
* **Phase 2: Data Cleaning & Transformation (ETL)**
    * **Cleaning:** Handled missing values, removed duplicates, and corrected data types (e.g., converting `date` strings to datetime objects).
    * **Feature Engineering:** Created new variables to drive insight, including `price_per_sqft`, `house_age`, and categorization of `age_group` (Pre-War, Mid-Century, Modern).
* **Phase 3: Exploratory Data Analysis (EDA) & Hypothesis Testing**
    * Conducted Univariate analysis to understand distributions (Normality checks).
    * Performed Bivariate/Multivariate analysis to test relationships (e.g., *Does renovation impact price differently by era?*).
    * Validated hypotheses using rigorous statistical tests (Mann-Whitney U, ANOVA).
* **Phase 4: Predictive Modeling & Dashboarding**
    * Trained Machine Learning models (Random Forest) for price estimation.
    * Developed an interactive Streamlit dashboard to operationalize findings for end-users.

### 2. Data Management Strategy
Data integrity was maintained through a strict separation of concerns:

* **Storage Architecture:** The project utilizes a "Raw vs. Processed" directory structure.
    * `data/raw/`: Contains the immutable original CSV file (`kc_house_data.csv`).
    * `data/processed/`: Stores the cleaned and transformed dataset (`final_house_data.csv`) used for modeling and the dashboard. This ensures the original data is never overwritten.
* **Privacy & Ethics:** The dataset contains no Personally Identifiable Information (PII). All analysis is performed at the property level, aggregated by Zip Code or structural features, ensuring ethical compliance.
* **Version Control:** All code, notebooks, and dataset metadata are managed via Git and GitHub to ensure reproducibility.

### 3. Rationale for Methodologies
The research methodologies were chosen based on the specific distribution of the data and the nature of the business questions.

**Statistical Analysis Choices:**
* **Median vs. Mean:** House prices are right-skewed with significant outliers (luxury properties). We utilized the **Median** as the primary central tendency metric to provide a realistic representation of the "typical" buyer experience.
* **Mann-Whitney U Test:** Used to validate the "Waterfront Premium." Since the Shapiro-Wilk test confirmed price data was **non-normal**, this non-parametric test was chosen over the T-Test to compare independent groups.
* **Spearman Correlation:** Used for the "Feature Importance" analysis. Features like `Grade` (1-13) and `Condition` (1-5) are **ordinal** (ranked categories). Spearman correlation is more appropriate than Pearson for detecting monotonic relationships in ranked data.
* TODO **Two-Way ANOVA:** Used to validate the "Renovation ROI" hypothesis. This test was necessary to detect the **interaction effect** between `Age Group` and `Renovation Status`, proving that renovation value depends on the era of the home.

**Machine Learning Choices:**
* **Random Forest Regressor:** Selected for the Price Prediction tool. Real estate data contains non-linear relationships (e.g., the complex interaction between latitude/longitude and price). Random Forest handles these non-linearities and feature interactions better than Linear Regression, resulting in a higher R² score (0.865) and a more accurate "Fair Value" estimate.

## The rationale to map the business requirements to the Data Visualisations
| Business Requirement | Data Visualisation(s) | Rationale |
| :--- | :--- | :--- |
| **🏷️ User Group 1: Buyers** | | |
| **1. Affordability:**<br>Identify the top 10 most affordable zip codes. | **Bar Chart:**<br>Top 10 Zip Codes by Median Price. | **Validates Hypothesis 1.**<br>A bar chart allows for a clear ranking of categorical data (zip codes). We use the **Median** rather than the Mean to prevent high-priced outliers (luxury estates) from skewing the perception of affordability, ensuring buyers see a realistic entry point. |
| **2. Value Assessment:**<br>Quantify the premium for "Waterfront" properties. | **Box Plot & Mann-Whitney U Test:**<br>Price distribution by Waterfront status. | **Validates Hypothesis 2.**<br>As established in Notebook 1, price data is **non-normal**. A Box Plot visually contrasts the spread and outliers of the two groups, while the Mann-Whitney U test provides the necessary non-parametric statistical confirmation that the premium is significant and not due to chance. |
| **3. Feature Importance:**<br>Determine if House Grade or Condition matters more. | **Heatmap & Spearman Correlation:**<br>Interaction between Grade, Condition, and Price. | **Validates Hypothesis 3.**<br>Since Grade and Condition are **ordinal** variables (ranked categories), Spearman Correlation is the appropriate statistical measure. The heatmap visualizes the interaction, confirming that higher construction grades correlate more strongly with price than cosmetic condition. |
| **4. Prediction:**<br>Estimate fair market value to make competitive offers. | **Predictive Model:**<br>Random Forest Regressor. | **Operationalizes Findings.**<br>House prices are influenced by **non-linear relationships** (e.g., the interaction between latitude/longitude and property size). A Random Forest model captures these complexities better than simple linear formulas, providing a "Fair Value" estimate to the buyers |
| **💰 User Group 2: Sellers** | | |
| **5. Feature Value:**<br>Identify specific home features that add the most financial value. | **Bar Chart:**<br>Feature Correlation with Price. | **General Feature Analysis.**<br>Sellers need a quick, digestible hierarchy of value. A correlation bar chart visually ranks features (e.g., SqFt, Bathrooms) by their impact on price, allowing sellers to prioritize which attributes to highlight in their marketing materials. |
| **6. Timing:**<br>Identify the best month to sell for maximum profit. | **Line Chart (Time-Series):**<br>Median Price vs. Month Sold. | **Validates Hypothesis 4.**<br>To identify seasonal trends, a time-series view is essential. This visualization exposes the cyclical nature of the market, highlighting the Spring/Summer peak (April/May) to advise sellers on the optimal window for listing. |
| **7. ROI Analysis:**<br>Determine if renovations yield a statistically significant return. | **Interaction Plot (Bar Chart):**<br>Price by Age Group grouped by Renovation Status. | **Validates Hypothesis 5.**<br>The value of renovation is **not uniform**. A simple average would hide the truth. This plot separates the data by Era (Pre-war, Mid-Century, Modern), revealing that Mid-Century homes yield a significantly higher ROI (~60%) than other eras. |
| **8. Listing Strategy:**<br>Set optimal prices based on neighborhood trends. | **Predictive Model:**<br>Random Forest. | **Operationalizes Findings.**<br>While buyers use the model to find deals, sellers use it to establish a **baseline market value**. By inputting their specific home features, they get a data-driven starting price that removes emotional bias from the listing strategy. |

## Analysis techniques used - TODO
* List the data analysis methods used and explain limitations or alternative approaches.
* How did you structure the data analysis techniques. Justify your response.
* Did the data limit you, and did you use an alternative approach to meet these challenges?
* How did you use generative AI tools to help with ideation, design thinking and code optimisation?

## Ethical Considerations & Data Privacy
Although this is a public dataset, ethical usage of data is paramount:
* **Privacy:** The dataset contains property features and locations (Latitude/Longitude) but **no Personally Identifiable Information (PII)** regarding previous owners.
* **Fairness:** We excluded variables that could introduce bias (e.g., zip codes were used for location value, not demographic profiling).
* **Usage:** Data is used strictly for educational and market analysis purposes, complying with Kaggle's open license terms.

## Dashboard Design - TODO
* List all dashboard pages and their content, either blocks of information or widgets, like buttons, checkboxes, images, or any other item that your dashboard library supports.
* Later, during the project development, you may revisit your dashboard plan to update a given feature (for example, at the beginning of the project you were confident you would use a given plot to display an insight but subsequently you used another plot type).
* How were data insights communicated to technical and non-technical audiences?
* Explain how the dashboard was designed to communicate complex data insights to different audiences. 

## Unfixed Bugs - TODO
* Please mention unfixed bugs and why they were not fixed. This section should include shortcomings of the frameworks or technologies used. Although time can be a significant variable to consider, paucity of time and difficulty understanding implementation are not valid reasons to leave bugs unfixed.
* Did you recognise gaps in your knowledge, and how did you address them?
* If applicable, include evidence of feedback received (from peers or instructors) and how it improved your approach or understanding.

## Development Roadmap
The project followed a 5-phase development lifecycle:

1.  **Phase 1: ETL & Cleaning (Notebook 01)**
    * Handling outliers (e.g., the 33-bedroom error).
    * Imputing missing values and fixing data types.
2.  **Phase 2: Feature Engineering (Notebook 02)**
    * Creating `log_price` for normalization.
    * Extracting `sale_month` and `year_renovated`.
3.  **Phase 3: Statistical Analysis (Notebooks 03 & 04)**
    * Conducting Hypothesis Testing (Hypothesis H1-H5).
    * Visualizing correlations and distributions.
4.  **Phase 4: Machine Learning (Notebook 05)**
    * Building and tuning the Random Forest Regressor.
5.  **Phase 5: Dashboard & Documentation**
    * Streamlit app development and README creation.

### Challenges & Strategies
    * Data Skewness & Quality: Addressed significant right-skewness in the target variable (price) by applying Log Transformations to improve model performance. Mitigated data quality issues, such as the erroneous "33-bedroom" outlier, through rigorous cleaning and domain-aware filtering.
    * Computational Constraints: Overcame performance bottlenecks during Random Forest Hyperparameter Tuning on local hardware (laptop) by optimizing the feature set and prioritizing RandomizedSearchCV over exhaustive grid searches to balance accuracy with training time.

### Future Learning
    * Advanced Modeling: I plan to explore Gradient Boosting algorithms (XGBoost, LightGBM) to potentially surpass the performance of the Random Forest model.
    * Advanced Statistical Analysis: Aim to deepen expertise in Statistical Inference, specifically mastering the application of complex Parametric vs. Non-Parametric tests


## Deployment
* The App live link is: https://YOUR_APP_NAME.streamlit.com/ 

To run this project locally, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/](https://github.com/dumindagamage/House-Price-Analysis.git)
    ```
2.  **Navigate to the project directory:**
    ```bash
    cd [REPO-NAME]
    ```
3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
4.  **Run the Streamlit Dashboard:**
    ```bash
    streamlit run dashboard/app.py
    ```


## Main Data Analysis Libraries
* **Language:** Python 3.x
* **Data Manipulation:** Pandas, NumPy
* **Visualization:** Plotly, Seaborn, Matplotlib
* **Machine Learning:** Scikit-Learn (Pipeline, RandomForestRegressor)
* **Dashboarding:** Streamlit


## Credits 
* **Dataset:** [Kaggle House Sales in King County](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction).
* **Template:** Code Institute README Template.
* **CI LMS:** Code Institute LMS.

### Content  - TODO

- The text for the Home page was taken from Wikipedia Article A
- Instructions on how to implement form validation on the Sign-Up page was taken from [Specific YouTube Tutorial](https://www.youtube.com/)
- The icons in the footer were taken from [Font Awesome](https://fontawesome.com/)

### Media

- The photos used on the home and sign-up page are from This Open-Source site
- The images used for the gallery page were taken from this other open-source site



## Acknowledgements (optional)
* Thank the people who provided support through this project.