
<img src="./Images/ReadmeHeader.svg" style="width:100%; height:auto;">

# Executive Summary
### TLDR:
**Summary**
- We expect overall employee churn (net loss) & turnover (total loss) to be 15% & 26% respectively for FY 2025
  - Churn rates vary across job functions, highest rates for Ops & lowest for Engineering/Finance
  - Largest drivers for turnover is employee tenure & working from either the Seattle or SanFran branches
    - On average, 1 additional year of tenure is associated with a 5% decrease in turnover
      - Highest rates of turnover for associates < 2 years tenure
    - Seattle & SanFran branches are associated with 5% less turnover
      
**Recommendation**
- Investigating the differences between Seattle/SanFran & the NYC location, as both have similar work forces but different churn rates
- Paying extra attention to associates with < 2 years of tenure & identify what is driving turnover
 
### Hypothesis
&nbsp;&nbsp;&nbsp;&nbsp;*We anticipate **pay rate, job function, location, tenure & total promotions** are correlated with employee turnover*
  
&nbsp;&nbsp;&nbsp;&nbsp;*Using these key indicators we will construct an quarterly churn rate for 2025 to ensure consistent staffing*

### Data

*Assumptions*
- Total promotions only include what can be observed in the data (associate may have received a promotion prior to 2022)
- We assume any job changes in data imply a promotion
- We assume an employees hire date is the earliest date between recorded hire date & effective date
  - 26% of the data contained a hire date after a recorded effective date   

*Description*

Given Values
- Quarterly associate level data from Jan 2022 to Oct 2024
  - Gegraphical (location)
  - Job Type Characteristics (function, level, pay type)

Calculated Values
- Total tenure role tenure for associates
- Total promotions (unique job function/level combinations)

### Methods
We use a dual approach to estimate churn in 2025 - analytical & ML. 

First is an analytical approach where (only includes historical churn rates for a given job type). The second approach uses an ML model that utlizes all of our employee data (not only historical churn rates)

*Analytical*
-  We visually inspect the data & apply a standard time series model. Put simply, this is an elevated moving average
  -  Pros: simple to implement & easy to explain, predicts churn directly
  -  Cons: unlikely to catch new trends & has 'self-fulfilling prophecy' as we can directly impact churn by our hiring practices

*ML*
- We utilize all of our employee data & apply an ML model to predict if our active associates are likely to leave the company
  - Pros: utilizes all of the information at our disposal, more likely to catch new patterns in data, typically more accurate
  - Cons: somewhat blackbox, difficult derive insights from, calculates current staff turnover not churn

### Results
- For FY 2025 our time series model predicts 15% churn & our ML model predicts 26% turnover
  - since our ML model predicts turnover, not churn, we can back out desired churn by setting a hiring rate & subtracting that from our anticipated turnover rate
- Churn/Attrition rates show similar results with both models (within 5%) except for  Sales & StrategyOps
  - Largest variation is in Sales & StrategyOps which is to be expected with the spikey data shape (Sales) & inconsistent trend (Strategy) making it difficult to fit a time series model (see **appendix**)

**Model Predictions**
<p float="left">
  <img src="./Output_Files/ts_class_pred.svg" width="45%" />
  <img src="./Output_Files/xgboost_class_pred.svg" width="45%" />
</p>

**Turnover Drivers**
- The ML model indicates the highest driver of turnover is employee Tenure & working in the SanFran/Seattle offices
  - each additional year of Tenure is associated with a 5% decrease in turnover, with a steep dropoffs at year 3 & 4
  - Working from SanFran/Seattel is also associated with a 5% decrease in turnover

<p float="left">
  <img src="./Output_Files/xgboost_importance.svg" width="50%" />
  <img src="./Output_Files/tenure_term_rate.svg" width="40%" />
</p>

### Recommendation
- It is important to note that churn only captures net staff levels, not how many associates have left the company
- Instead of targeting a churn rate, which is directly influenced by companies past hiring rates, we would be better to estimate turnover & set a hiring rate to achieve the desired churn rate

## Appendix

### Supplemental Notes

*Trends*
- Hourly Staff have highest rates of churn
  - Hourly staff is also highly collinear with OpsStrategy & the MoonMar1 location
  - OpsStrategy has the highest rate of churn & is a near constant pattern since 2023 arounnd 27% per quarter
- Sales has the next highest rate of church and it is highly seasonal, averaging 22% per quarter
- HR had a constant churn rate in early 2022, high variability in 2023, then stabalizing in 2024
  - was churn due to voluntary resignations, layoffs, or a combination? This will help determine the best forecast for 2025
- Both engineering & finance have the lowest churn rates and follow a nearly identical seasonality
  -  Engineering is highest job type correlated with positive tenure (21%) followed by finance (7%), likely driving lower turnover rates
 
*Models - Exponential Smoothing*
- We use an exponential smoothing model with additive seasonality & trend, this model fits well for Engineering & Sales, but has large misses in the other job types
  - If we proceed with this method, it is recommended to:
    - Switch OpsStrategy to a moving average since it is fairly stable quarter over quarter since 2023
    - Investigate why the model overshot for Finance but not Engineering. As both jobs have fairly similar patterns it may be worth combining them
- To measure accuracy, we use first 8 periods in the data to build the model & test on the remaining 4 periods
- We calculate a weighted MAPE of 12.71%, weighting by a job function (to account for differences in workforce sizes)

 <p float="left">
  <img src="./Output_Files/forecast_Strategy.svg" width="45%" />
  <img src="./Output_Files/forecast_Sales.svg" width="45%" />
</p>

 <p float="left">
  <img src="./Output_Files/forecast_HR.svg" width="45%" />
  <img src="./Output_Files/forecast_Engineering.svg" width="45%" />
</p>


*Models - ML*
- We use XGBOOST Classifier (logistic random forest) to predict if a given associate is likely to leave the company
  - To estimate churn, we must back it out from predicted employee turnover
  - As we are predicting at an associate level how likely the are to leave, we do not have a quarterly forecast, but an entire workforce prediction
    - We can apply our turnover rate and scale historical quarters should we need a quarter by quarter estimation
- Model Specifics
  - We apply a proportional sampling of terminated/active associates as well as job type
    - This prevents the model overfitting to a specific job type
  - 70% of data is used for training & the remaining 30% split equally between validation & testing sets
  - The model accurate predicts employee turnover with 84% accuracy
- Enhancements
  - To improve the model having additional factors such as associate age, marital status, & workforce modality could benefit 

### Code
[Data Prep Code](https://github.com/couch2coders/DoorDash/blob/main/Code%20Files/Data%20Prep.ipynb)
[Data Modeling Code](https://github.com/couch2coders/DoorDash/blob/main/Code%20Files/Modeling.ipynb)
