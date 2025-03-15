![Logo](./Images/Readme%20Header.png)

# DoorDash
Case study for Door Dash Senior Analyst

## Executive Summary
### TLDR:
- We expect overall employee churn to be XX with XX level of accuracy in FY 2025
  - Churn rates vary across job functions, highest rates for Ops & lowest for XX
  - Largest drivers for churn include X, Y, Z
- Recommendation
  - To lower churn & increase employee retention we recommend to do X,Y,Z
 
### Hypothesis
*We anticipate pay rate, job function, location, tenure & total promotions are correlated with employee turnover*
Using these key indicators we will construct an quarterly churn rate for 2025 to ensure consistent staff

### Data
*Assumptions*
- Total promotions only include what can be observed in the data (associate may have received a promotion prior to 2022)
- We assume any job changes in data imply a promotion

*Description*
Given Values
- Quarterly associate level data from Jan 2022 to Oct 2024
  - Gegraphical (location)
  - Job Type Characteristics (function, level, pay type)

Calculated Values
- Total tenure & role tenure for associates

### Methods
We use a dual approach to estimate churn in 2025 - analytical & ML. 

First is an analytical approach where (only includes historical churn rates for a given job type). The second approach uses an ML model that utlizes all of our employee data (not only historical churn rates)

*Analytical*
-  We visually inspect the data & apply a standard time series model. Put simply, this is an elevated moving average
  -  Pros: simple to implement & easy to explain
  -  Cons: unlikely to catch new trends & difficult to improve since it is only looking at past churn rates

*ML*
- We utilize all of our employee data & apply an ML model to predict if our active associates are likely to leave the company
  - Pros: utilizes all of the information at our disposal, more likely to catch new patterns in data, typically more accurate
  - Cons: somewhat blackbox, difficult derive insights from
 
### Recommendation


## Appendix
### Supplemental Notes
### Code

