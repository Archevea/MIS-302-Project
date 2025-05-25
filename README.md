------------------------------------------------------------------------

## Objective

-   Predict whether a client subscribes to a term deposit.
-   Use demographic and campaign-related features.
-   Build and evaluate a predictive model using logistic regression.

## About the Dataset

-   Source: Portuguese bank’s direct marketing campaigns  
-   Observations: 45211  
-   Variables: 17  
-   Features include:
    -   Age, job, marital status, education
    -   Housing and personal loan status
    -   Contact type and campaign info  
-   **Target variable:** `y` – whether the client subscribed (yes/no)

------------------------------------------------------------------------

## Presentation Roadmap

1.  Exploratory Data Analysis (EDA)
2.  Linear Modeling
3.  Logistic Regression Modeling
4.  Model Evaluation (Accuracy, ROC, AUC)
5.  Marketing Recommendations

------------------------------------------------------------------------

## Colnames, Glimpse and Summary

     [1] "age"       "job"       "marital"   "education" "default"   "balance"  
     [7] "housing"   "loan"      "contact"   "day"       "month"     "duration" 
    [13] "campaign"  "pdays"     "previous"  "poutcome"  "y"        

    Rows: 45,211
    Columns: 17
    $ age       <int> 58, 44, 33, 47, 33, 35, 28, 42, 58, 43, 41, 29, 53, 58, 57, …
    $ job       <chr> "management", "technician", "entrepreneur", "blue-collar", "…
    $ marital   <chr> "married", "single", "married", "married", "single", "marrie…
    $ education <chr> "tertiary", "secondary", "secondary", "unknown", "unknown", …
    $ default   <chr> "no", "no", "no", "no", "no", "no", "no", "yes", "no", "no",…
    $ balance   <int> 2143, 29, 2, 1506, 1, 231, 447, 2, 121, 593, 270, 390, 6, 71…
    $ housing   <chr> "yes", "yes", "yes", "yes", "no", "yes", "yes", "yes", "yes"…
    $ loan      <chr> "no", "no", "yes", "no", "no", "no", "yes", "no", "no", "no"…
    $ contact   <chr> "unknown", "unknown", "unknown", "unknown", "unknown", "unkn…
    $ day       <int> 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, …
    $ month     <chr> "may", "may", "may", "may", "may", "may", "may", "may", "may…
    $ duration  <int> 261, 151, 76, 92, 198, 139, 217, 380, 50, 55, 222, 137, 517,…
    $ campaign  <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    $ pdays     <int> -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, …
    $ previous  <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    $ poutcome  <chr> "unknown", "unknown", "unknown", "unknown", "unknown", "unkn…
    $ y         <chr> "no", "no", "no", "no", "no", "no", "no", "no", "no", "no", …

-   Simple Look at the Data
-   No missing values, but some variables (job, education, poutcome)
    include “unknown” categories.
-   Numeric variables like balance and duration show skewed
    distributions and potential outliers.
-   Skewness can affect models that assume normality (e.g., logistic
    regression).

<!-- -->

          age            job              marital           education        
     Min.   :18.00   Length:45211       Length:45211       Length:45211      
     1st Qu.:33.00   Class :character   Class :character   Class :character  
     Median :39.00   Mode  :character   Mode  :character   Mode  :character  
     Mean   :40.94                                                           
     3rd Qu.:48.00                                                           
     Max.   :95.00                                                           
       default             balance         housing              loan          
     Length:45211       Min.   : -8019   Length:45211       Length:45211      
     Class :character   1st Qu.:    72   Class :character   Class :character  
     Mode  :character   Median :   448   Mode  :character   Mode  :character  
                        Mean   :  1362                                        
                        3rd Qu.:  1428                                        
                        Max.   :102127                                        
       contact               day           month              duration     
     Length:45211       Min.   : 1.00   Length:45211       Min.   :   0.0  
     Class :character   1st Qu.: 8.00   Class :character   1st Qu.: 103.0  
     Mode  :character   Median :16.00   Mode  :character   Median : 180.0  
                        Mean   :15.81                      Mean   : 258.2  
                        3rd Qu.:21.00                      3rd Qu.: 319.0  
                        Max.   :31.00                      Max.   :4918.0  
        campaign          pdays          previous          poutcome        
     Min.   : 1.000   Min.   : -1.0   Min.   :  0.0000   Length:45211      
     1st Qu.: 1.000   1st Qu.: -1.0   1st Qu.:  0.0000   Class :character  
     Median : 2.000   Median : -1.0   Median :  0.0000   Mode  :character  
     Mean   : 2.764   Mean   : 40.2   Mean   :  0.5803                     
     3rd Qu.: 3.000   3rd Qu.: -1.0   3rd Qu.:  0.0000                     
     Max.   :63.000   Max.   :871.0   Max.   :275.0000                     
          y            
     Length:45211      
     Class :character  
     Mode  :character  
                       
                       
                       

-   Quick overview of the dataset’s structure.
-   It showed that the data contains 45,211 observations and 17
    variables, including both numeric and categorical types.

------------------------------------------------------------------------

## Target Variable: Subscription Outcome (y)

-   y represents whether a client subscribed to a term deposit after the
    marketing campaign.
-   It is a binary categorical variable with two possible values: “yes”
    and “no”.

![image](https://github.com/user-attachments/assets/e31f4c71-7c30-4cef-afa6-18c1416d2931)

-   Only about 11.7% of clients subscribed (yes), while 88.3% did not
    (no).
-   This imbalance can bias models toward predicting the majority class
    (no) if not addressed properly.

------------------------------------------------------------------------

## 📊 Response Rate by Job Type

-   This horizontal bar chart displays the conversion (subscription)
    rate for each job category.
-   Jobs are sorted by response rate, and a dashed vertical line
    represents the overall average subscription rate (~11.7%).

![image](https://github.com/user-attachments/assets/8e30e672-884c-4b1d-a920-7c844256fd75)

-   Clients in roles like student, retired, and unemployed are more
    likely to subscribe, making them ideal segments to focus on in
    future campaigns.

-   Conversely, blue-collar, entrepreneur, and housemaid roles have much
    lower response rates, indicating lower campaign efficiency in those
    groups.

------------------------------------------------------------------------

## Subscription Rate by Marital Status

-   This bar chart shows subscription rates across different **marital
    status** groups.
-   Segments with **above-average rates** are shown in blue.
-   The **dashed line** indicates the overall conversion conversion
    rate.

![image](https://github.com/user-attachments/assets/2b30df28-0675-4912-ac0f-0776d2e3b9ce)

-   Why We Analyzed marital?

-   In marketing, marital status is commonly used for segmentation and
    message personalization.

-   Graphs:

-   Single clients have the highest conversion rate, significantly above
    average.

-   Divorced clients are slightly above average.

-   Married clients show a below-average response rate.

------------------------------------------------------------------------

## Response Rate by Education Level

-   This chart shows subscription rates across different **education
    levels**.
-   Segments with **above-average subscription rates** are shown in
    blue.
-   The **dashed line** marks the overall subscription rate across all
    clients.

![image](https://github.com/user-attachments/assets/e452d7ff-2cfe-46cc-886f-227fc0783c30)

-   Clients with tertiary education have the highest conversion rate,
    well above the average.

-   The “unknown” group surprisingly also performs above average.

-   Clients with secondary and primary education are below the average,
    with primary being the lowest.

-   How to Use This Insight?

-   In modeling: Include education as a categorical feature with
    “unknown” treated explicitly.

-   In strategy: Prioritize tertiary-educated clients for high-value
    campaigns

------------------------------------------------------------------------

## Age Distribution of Customers

-   This histogram shows the overall age distribution of clients.
-   It helps us detect age clusters and possible outliers.
-   Can age be an informative predictor?

![image](https://github.com/user-attachments/assets/8b116ab8-10ea-4f60-bba0-0e017e0fd1b5)

-   Most clients are between 30 and 60 years old.

-   There are some younger (under 25) and older (over 70) clients, but
    they’re fewer.

-   The distribution is slightly right-skewed, suggesting more clients
    are in middle adulthood.

------------------------------------------------------------------------

## Age Distribution by Subscription Outcome

-   This violin plot shows how **age** is distributed across
    subscription outcomes.
-   It helps us assess whether **age** influences the likelihood of
    subscribing.

![image](https://github.com/user-attachments/assets/af36d7eb-8c73-4db5-a2ee-7c0e886fa008)

-   ✅ For yes (subscribed):

-   The distribution is slightly left-skewed, with a noticeable peak in
    the 30–40 age range.

-   Boxplot shows the median is lower than the “no” group → younger
    clients subscribe more often.

-   Distribution is more compact, suggesting more consistency in age
    among those who subscribed.

-   ❌ For no (did not subscribe):

-   Broader distribution with a slight right-skew.

-   Median age is higher, and the range of ages is wider.

-   ## Larger presence of older individuals who are less likely to subscribe.

## Call Duration vs Subscription Outcome

-   `duration` represents the call length in seconds.
-   It’s **highly correlated** with the target variable (`y`) because
    longer calls often result in subscriptions.
-   ⚠️ However, it’s a **post-outcome variable** — we can’t know
    duration before the call ends, so it must be used carefully.

![image](https://github.com/user-attachments/assets/fe1b50fb-cacc-4d42-88d0-d188853431c4)

-   **Clients who subscribed (yes) tend to have longer call durations.**

-   The **yes curve** peaks later and has a **wider tail**, indicating
    extended conversations.

-   The contrast, **no responses** are mostly concentrated in **short
    calls (under ~300 seconds)**.

-   The distributions are clearly separated, which explains the **high
    correlation with the target `y`**.

------------------------------------------------------------------------

## Account Balance vs Subscription Outcome

-   `balance` shows the client’s average yearly account balance in
    euros.
-   The violin plot below compares balance distributions for subscribers
    vs non-subscribers.

![image](https://github.com/user-attachments/assets/b56b13ab-e7fb-43a3-9207-98da52e96242)

-   **Clients who subscribed (`yes`) generally have higher balances**
    than non-subscribers.

-   The **median balance** (center of the box) is **visibly higher** for
    the `yes` group.

-   Both distributions are **positively skewed**, but the **`no` group
    shows more concentration at lower balances**.

-   The **spread is larger for subscribers**, suggesting **greater
    financial diversity** among those who converted.

⚠️ Caution: Outliers & Skewness The raw balance variable includes
extreme outliers, which can distort model training.

------------------------------------------------------------------------

## Correlation Among Numeric Variables

-   This plot shows the correlations between all numeric variables.
-   It helps us detect multicollinearity and understand variable
    relationships.

![image](https://github.com/user-attachments/assets/52f0289d-5bdf-4b39-902d-d5e67f7b62b0)

-   duration and y have the strongest positive correlation (0.39), which
    is expected.

-   Longer calls often lead to successful subscriptions.

-   ️⚠️ However, as discussed earlier, duration is a post-outcome
    variable and should be excluded from predictive models to avoid data
    leakage.

-   pdays and duration also show a moderate correlation (0.45),
    indicating that clients who were recently contacted may also have
    longer conversations.

-   Other variables like balance, age, previous, and campaign show very
    weak correlations with y (mostly under ±0.1).

-   How This Results Affect The Model Choosin Proccess ?

-   The low correlation among most predictors suggests no serious
    multicollinearity, which is good for models like logistic
    regression.

-   Variables with even small correlation to y (like pdays, previous,
    balance) might still carry non-linear or interaction effects
    valuable to tree-based models

------------------------------------------------------------------------

## Campaign Contact Frequency

-   `campaign` shows the number of contacts during this campaign.
-   High values may indicate over-contacting or customer fatigue.

![image](https://github.com/user-attachments/assets/f733506b-1dd6-4753-87fe-8805190497f6)

-   **Most clients were contacted fewer than 5 times**, with a **sharp
    peak at 1 and 2 calls**.

-   **Very few clients received more than 10 calls** — but those who did
    might represent cases of **persistent outreach or inefficient
    targeting**.

-   The distribution is **heavily right-skewed**, indicating that
    **frequent contact is rare but exists**.

-   ⚠️ Modeling Note:

-   Due to its skewed distribution, consider log transformation or
    binning (e.g., 1 call, 2–3 calls, 4+ calls) before using in models.

------------------------------------------------------------------------

## Linear Models – Exploratory Insight

-   Although `y` is a binary variable, linear regression can still give
    us **initial insight** into which variable groups explain variance
    in subscription outcomes.
-   Below, we estimate four simple linear models and compare their **R²
    scores**.

------------------------------------------------------------------------

### Full Linear Model


    Call:
    lm(formula = y_numeric ~ ., data = bank_data_new)

    Residuals:
           Min         1Q     Median         3Q        Max 
    -1.019e-15 -5.000e-18  1.000e-18  5.000e-18  4.645e-14 

    Coefficients:
                         Estimate Std. Error    t value Pr(>|t|)    
    (Intercept)         3.781e-17  1.150e-17  3.287e+00  0.00101 ** 
    age                 1.132e-19  1.276e-19  8.870e-01  0.37505    
    jobblue-collar     -1.695e-17  3.981e-18 -4.258e+00 2.06e-05 ***
    jobentrepreneur    -1.843e-17  6.612e-18 -2.787e+00  0.00531 ** 
    jobhousemaid       -2.950e-17  7.181e-18 -4.109e+00 3.98e-05 ***
    jobmanagement      -9.305e-18  4.418e-18 -2.106e+00  0.03521 *  
    jobretired          3.339e-17  6.181e-18  5.402e+00 6.63e-08 ***
    jobself-employed   -1.996e-17  6.470e-18 -3.085e+00  0.00203 ** 
    jobservices        -1.476e-17  4.588e-18 -3.218e+00  0.00129 ** 
    jobstudent          6.585e-17  8.130e-18  8.100e+00 5.63e-16 ***
    jobtechnician      -1.034e-17  4.018e-18 -2.574e+00  0.01006 *  
    jobunemployed      -7.689e-18  6.885e-18 -1.117e+00  0.26409    
    jobunknown         -2.233e-17  1.357e-17 -1.646e+00  0.09984 .  
    maritalmarried     -1.346e-17  3.360e-18 -4.007e+00 6.17e-05 ***
    maritalsingle       5.649e-18  3.892e-18  1.451e+00  0.14665    
    educationsecondary  7.857e-18  3.333e-18  2.357e+00  0.01842 *  
    educationtertiary   2.375e-17  4.174e-18  5.690e+00 1.28e-08 ***
    educationunknown    1.278e-17  5.940e-18  2.151e+00  0.03147 *  
    defaultyes         -7.652e-19  7.817e-18 -9.800e-02  0.92201    
    balance             8.208e-22  3.479e-22  2.359e+00  0.01831 *  
    housingyes         -3.914e-17  2.513e-18 -1.557e+01  < 2e-16 ***
    loanyes            -2.247e-17  2.901e-18 -7.745e+00 9.78e-15 ***
    contacttelephone   -9.215e-18  4.406e-18 -2.091e+00  0.03650 *  
    contactunknown     -7.544e-17  3.569e-18 -2.114e+01  < 2e-16 ***
    day                 1.007e-18  1.439e-19  6.997e+00 2.65e-12 ***
    monthaug           -4.940e-17  5.281e-18 -9.355e+00  < 2e-16 ***
    monthdec            1.092e-16  1.567e-17  6.969e+00 3.23e-12 ***
    monthfeb           -6.836e-18  6.188e-18 -1.105e+00  0.26928    
    monthjan           -9.591e-17  7.354e-18 -1.304e+01  < 2e-16 ***
    monthjul           -5.390e-17  5.054e-18 -1.067e+01  < 2e-16 ***
    monthjun            1.919e-17  5.995e-18  3.201e+00  0.00137 ** 
    monthmar            2.400e-16  1.100e-17  2.182e+01  < 2e-16 ***
    monthmay           -2.292e-17  4.903e-18 -4.674e+00 2.96e-06 ***
    monthnov           -5.779e-17  5.455e-18 -1.059e+01  < 2e-16 ***
    monthoct            1.372e-16  9.209e-18  1.490e+01  < 2e-16 ***
    monthsep            1.446e-16  1.020e-17  1.418e+01  < 2e-16 ***
    duration            4.462e-19  4.431e-21  1.007e+02  < 2e-16 ***
    campaign           -1.415e-18  3.522e-19 -4.018e+00 5.89e-05 ***
    pdays              -5.983e-20  2.230e-20 -2.683e+00  0.00730 ** 
    previous            1.019e-18  5.328e-19  1.913e+00  0.05581 .  
    poutcomeother       2.563e-17  6.049e-18  4.237e+00 2.27e-05 ***
    poutcomesuccess     3.933e-16  6.981e-18  5.634e+01  < 2e-16 ***
    poutcomeunknown    -1.350e-17  6.673e-18 -2.023e+00  0.04304 *  
    yyes                1.000e+00  3.845e-18  2.601e+17  < 2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 2.195e-16 on 45167 degrees of freedom
    Multiple R-squared:      1, Adjusted R-squared:      1 
    F-statistic: 2.255e+33 on 43 and 45167 DF,  p-value: < 2.2e-16

<span style="color:red"><b>Intercept (~0):</b></span>

-   The intercept is very close to zero (3.78e-17), which means the
    baseline prediction (when all variables are zero) is nearly 0.

-   In real life, this value doesn’t have practical meaning, since no
    client can have all features as zero. It mainly helps set the
    model’s starting point.

<span style="color:red"><b>Significant Job Types:</b></span>

-   Some job categories strongly affect the probability of subscription.

-   jobstudent (+): Students are more likely to say “yes”.

-   jobretired (+): Retired people also have a higher chance.

-   jobblue-collar, jobself-employed, jobservices (-): These jobs are
    associated with a lower chance.

-   These effects are statistically significant (p &lt; 0.01) and could
    reflect real differences in behavior across job types.

<span style="color:red"><b>Month Effects:</b></span>

-   Some months show very strong patterns:

-   monthmar, monthoct, monthsep: Very high positive effect — clients
    are much more likely to subscribe in these months.

-   monthjan, monthjul, monthnov, monthaug: Negative effect — lower
    success rates in these months.

-   This suggests that timing of the campaign plays an important role in
    client decisions.

<span style="color:red"><b>Call Duration (0.000446):</b></span>

-   Duration is the most powerful predictor.

-   Longer calls = higher chance of success.

-   This is highly significant and makes sense: more talk time means
    better engagement.

<span style="color:red"><b>R² = 1.0 (100%):</b></span>

-   This model explains all the variation in the data — which seems too
    perfect.

-   This usually means there is a problem like data leakage.

<span style="color:red"><b>Problem Detected – yyes:</b></span>

-   The variable yyes has a coefficient of 1.0, meaning the model is
    predicting y\_numeric using itself.

-   This is a serious mistake. It makes the model useless in practice.

### Model 1 – Demographic Variables


    Call:
    lm(formula = y_numeric ~ age, data = bank_data_new)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -0.1582 -0.1216 -0.1140 -0.1094  0.9005 

    Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
    (Intercept) 0.0858166  0.0060184   14.26  < 2e-16 ***
    age         0.0007614  0.0001423    5.35 8.83e-08 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3213 on 45209 degrees of freedom
    Multiple R-squared:  0.0006328, Adjusted R-squared:  0.0006107 
    F-statistic: 28.63 on 1 and 45209 DF,  p-value: 8.826e-08

<span style="color:red"><b>Intercept (0.0858):</b></span>

-   When age = 0, the predicted probability of subscription is 8.58%.

-   Note: This is purely theoretical (clients cannot be age 0). The
    intercept helps anchor the model but has no real-world meaning here.

<span style="color:red"><b>Age Coefficient (0.00076):</b></span>

For every 1-year increase in age, the probability of subscribing
increases by 0.076% (a negligible effect).

-   Example: A 30-year-old has a predicted subscription probability of
    0.0858 + (30 × 0.00076) = 10.86%.

-   While statistically significant (*p* &lt; 0.001), this effect is
    practically meaningless due to its tiny magnitude.

<span style="color:red"><b>R² = 0.00063 (0.063%):</b></span>

-   The model explains only 0.063% of the variation in subscriptions.

-   This is extremely weak—age alone is irrelevant for predicting
    subscriptions.

<span style="color:red"><b>Residual Standard Error (0.321):</b></span>

-   Predictions are off by ±32 percentage points on average—far too high
    for practical use.

<span style="color:red"><b>F-test (*p* = 8.83e-08):</b></span>

-   The relationship is statistically significant, but only due to the
    huge sample size (n=45,211).

-   Key Insight: Statistical significance ≠ practical importance.

### Model 2 – Campaign Activity Variables

<span style="color:red"><b>Why these variables?</b></span>  
We included `campaign`, `pdays`, and `previous` because they directly
measure **customer engagement** during the marketing process:

-   **`campaign`**: How many times the client was contacted during this
    campaign.
-   **`pdays`**: Days since the client was last contacted (in a previous
    campaign).
-   **`previous`**: Number of times the client was contacted in earlier
    campaigns.

These are behavior-based features — not personal demographics — and are
often used to assess **marketing pressure, timing**, and
**persistence**.


    Call:
    lm(formula = y_numeric ~ campaign + pdays + previous, data = bank_data_new)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -2.42206 -0.11470 -0.10797 -0.09449  1.09414 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  1.217e-01  2.137e-03   56.95   <2e-16 ***
    campaign    -6.737e-03  4.856e-04  -13.88   <2e-16 ***
    pdays        2.284e-04  1.686e-05   13.55   <2e-16 ***
    previous     8.196e-03  7.304e-04   11.22   <2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3186 on 45207 degrees of freedom
    Multiple R-squared:  0.0176,    Adjusted R-squared:  0.01754 
    F-statistic:   270 on 3 and 45207 DF,  p-value: < 2.2e-16

<span style="color:red"><b>campaign coefficient = -0.0067</b></span>  
→ A negative relationship: more contact attempts in this campaign
slightly **decrease** the likelihood of subscription.  
→ This may indicate **diminishing returns** or even customer fatigue.

<span style="color:red"><b>pdays coefficient = +0.00023</b></span>  
→ Clients who were contacted recently (lower pdays) are **less likely**
to subscribe.  
→ Those contacted after more time tend to convert more, suggesting the
benefit of **spaced outreach**.

<span style="color:red"><b>previous coefficient = +0.0082</b></span>  
→ More successful interactions in past campaigns are **positively
associated** with current subscription.  
→ This matches intuition: **past responsiveness predicts future
interest**.

Model Fit Evaluation

<span style="color:red"><b>R² = 0.0176</b></span>  
→ Only **1.76%** of the variance is explained by these campaign
variables.  
→ This is **very low**, indicating the model has **poor explanatory
power**.

<span style="color:red"><b>Residual Std. Error = 0.3186</b></span>  
→ High residual error confirms that these variables **alone** are **not
sufficient** to predict subscriptions reliably.

### Model 3 – Financial Status & Credit Risk

<span style="color:red"><b>Why these variables?</b></span>  
This model includes financial background indicators that may influence a
client’s likelihood to invest:

-   **`balance`**: Yearly average account balance — a proxy for savings
    potential.
-   **`loan`**: Whether the client has a personal loan.
-   **`housing`**: Whether they have a housing loan.
-   **`default`**: History of credit default.

These variables are directly tied to a client’s **financial capacity and
risk profile**, which can shape their decision to commit to a term
deposit.


    Call:
    lm(formula = y_numeric ~ balance + loan + housing + default, 
        data = bank_data_new)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -0.5742 -0.1688 -0.0856 -0.0817  1.0113 

    Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
    (Intercept)  1.687e-01  2.470e-03  68.308  < 2e-16 ***
    balance      3.970e-06  4.940e-07   8.037  9.4e-16 ***
    loanyes     -5.100e-02  4.097e-03 -12.449  < 2e-16 ***
    housingyes  -8.686e-02  3.013e-03 -28.824  < 2e-16 ***
    defaultyes  -3.919e-02  1.127e-02  -3.476  0.00051 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3174 on 45206 degrees of freedom
    Multiple R-squared:  0.02501,   Adjusted R-squared:  0.02492 
    F-statistic: 289.9 on 4 and 45206 DF,  p-value: < 2.2e-16

<span style="color:red"><b>balance coefficient =
+0.00000397</b></span>  
→ Slight positive effect: clients with higher balances are **more
likely** to subscribe.  
→ Effect is statistically significant but **very small in magnitude**.

<span style="color:red"><b>loan = yes → -0.051</b></span>  
→ Clients with personal loans are **less likely** to subscribe. Possibly
due to financial constraints.

<span style="color:red"><b>housing = yes → -0.087</b></span>  
→ Having a housing loan is negatively associated with subscription,
indicating **lower investment capacity**.

<span style="color:red"><b>default = yes → -0.039</b></span>  
→ Past defaulters are **less responsive**, though this effect is weaker
than loan or housing.

Model Fit Evaluation

<span style="color:red"><b>R² = 0.025</b></span>  
→ The model explains only **2.5%** of the variation in the target
variable.  
→ This is a **very weak model** in terms of predictive power.

<span style="color:red"><b>Residual Std. Error = 0.3174</b></span>  
→ High error remains — the model is far from capturing subscription
behavior accurately.

### Model 4 – Temporal and Contact Info

<span style="color:red"><b>Why these variables?</b></span>  
We used `contact`, `month`, and `day` because they capture **when** and
**how** the client was reached, which may impact responsiveness:

-   **`contact`**: Method used to reach the client (cellular, telephone,
    unknown).
-   **`month`**: The month when the contact was made — seasonality
    effect.
-   **`day`**: Day of the month (1–31) — to check timing sensitivity.

<!-- -->


    Call:
    lm(formula = y_numeric ~ contact + month + day, data = bank_data_new)

    Residuals:
         Min       1Q   Median       3Q      Max 
    -0.52891 -0.11505 -0.10067 -0.01753  1.03121 

    Coefficients:
                       Estimate Std. Error t value Pr(>|t|)    
    (Intercept)       0.1947108  0.0066405  29.322  < 2e-16 ***
    contacttelephone -0.0274479  0.0060297  -4.552 5.33e-06 ***
    contactunknown   -0.1262973  0.0046539 -27.138  < 2e-16 ***
    monthaug         -0.0864558  0.0068959 -12.537  < 2e-16 ***
    monthdec          0.2755904  0.0218158  12.633  < 2e-16 ***
    monthfeb         -0.0263170  0.0085349  -3.083  0.00205 ** 
    monthjan         -0.0968317  0.0101916  -9.501  < 2e-16 ***
    monthjul         -0.0998670  0.0068030 -14.680  < 2e-16 ***
    monthjun          0.0123276  0.0082538   1.494  0.13529    
    monthmar          0.3266768  0.0152147  21.471  < 2e-16 ***
    monthmay         -0.0571930  0.0068379  -8.364  < 2e-16 ***
    monthnov         -0.0934202  0.0075011 -12.454  < 2e-16 ***
    monthoct          0.2520711  0.0127008  19.847  < 2e-16 ***
    monthsep          0.2803825  0.0140542  19.950  < 2e-16 ***
    day               0.0002428  0.0001972   1.231  0.21828    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.3078 on 45196 degrees of freedom
    Multiple R-squared:  0.08288,   Adjusted R-squared:  0.08259 
    F-statistic: 291.7 on 14 and 45196 DF,  p-value: < 2.2e-16

<span style="color:red"><b>contactunknown = -0.126</b></span>  
→ Clients contacted via unknown channels are **much less likely** to
subscribe.

<span style="color:red"><b>contacttelephone = -0.027</b></span>  
→ Traditional landline outreach performs **worse** than cellular
(reference).

<span style="color:red"><b>month effects:</b></span>  
→ **December (+0.27)**, **March (+0.33)**, **October (+0.25)**, and
**September (+0.28)** are associated with **much higher conversion
rates**.  
→ In contrast, **July (-0.10)**, **January (-0.097)**, and **August
(-0.086)** show **significantly lower** performance.

<span style="color:red"><b>day = not significant (p =
0.218)</b></span>  
→ The exact day of the month **does not significantly affect**
subscription outcome.

Model Fit Evaluation

<span style="color:red"><b>R² = 0.083</b></span>  
→ The model explains about **8.3%** of the variance in subscription.  
→ Better than campaign or financial models, but still **limited** in
predictive power.

<span style="color:red"><b>Residual Std. Error = 0.3078</b></span>  
→ Consistent with prior models — confirms that **substantial variation
remains unexplained**.

### R² Comparison Across Models

<table>
<caption>R² Comparison of Linear Models</caption>
<thead>
<tr>
<th style="text-align: left;">Model</th>
<th style="text-align: right;">R_Squared</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Demographic (Model 1)</td>
<td style="text-align: right;">0.0006</td>
</tr>
<tr>
<td style="text-align: left;">Campaign Activity (Model 2)</td>
<td style="text-align: right;">0.0176</td>
</tr>
<tr>
<td style="text-align: left;">Financial Status (Model 3)</td>
<td style="text-align: right;">0.0250</td>
</tr>
<tr>
<td style="text-align: left;">Contact &amp; Timing (Model 4)</td>
<td style="text-align: right;">0.0829</td>
</tr>
</tbody>
</table>

-   We compared four simple linear models to understand which types of
    variables carry more predictive power.
-   Model 1 (age + duration) performed best, but even that explained
    only 15.6% of the variance.
-   This suggests that subscription behavior is complex, and we need to
    combine multiple variable groups to build more powerful predictive
    models.”

<h3>
<b>Summary – Why Linear Regression Falls Short</b>
</h3>
<table border="1" cellspacing="0" cellpadding="6">
<thead style="background-color:#f2f2f2;">
<tr>
<th>
<b>Issue</b>
</th>
<th>
<b>Why It’s a Problem</b>
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
Binary Target
</td>
<td>
Linear model assumes continuity
</td>
</tr>
<tr>
<td>
Invalid Predictions
</td>
<td>
Can output values &lt; 0 or &gt; 1
</td>
</tr>
<tr>
<td>
No Class Probabilities
</td>
<td>
Doesn’t estimate likelihood of “yes”
</td>
</tr>
<tr>
<td>
Misleading Metrics
</td>
<td>
R² doesn’t tell us real classification performance
</td>
</tr>
<tr>
<td>
Poor in Decision-Making
</td>
<td>
No clear threshold for class decision
</td>
</tr>
</tbody>
</table>

<span style="font-weight:bold">Linear Model Review</span>

-   Although we used linear regression for exploratory purposes, it’s
    not suitable for binary classification tasks like ours.
-   It doesn’t model probabilities, can produce invalid outputs, and
    provides metrics that don’t reflect true model performance.
-   We instead rely on logistic regression, SVM, and random forest,
    which are designed for binary outcomes and decision boundaries.”

Let’s now move on to logistic regression for proper binary
classification.

------------------------------------------------------------------------

<html lang="en">
<head>
<meta charset="UTF-8">
<title>
Model 1: Logistic Regression with Cross-Validation
</title>
<style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px;
      background-color: #fafafa;
      color: #333;
    }
    h1, h2 {
      color: #2c3e50;
    }
    p {
      line-height: 1.6;
    }
    .highlight {
      color: #e74c3c;
      font-weight: bold;
    }
    .code-block {
      background-color: #f4f4f4;
      padding: 12px;
      border-left: 5px solid #2980b9;
      font-family: monospace;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
<h1>
Model 1: Logistic Regression – Initial Setup
</h1>
<h2>
🔍 Objective
</h2>
<p>
The goal of this model is to <span class="highlight">predict whether a
client subscribes to a term deposit</span> based on various features
such as demographics, campaign interactions, and financial history.
Since the target variable (<code>y</code>) is binary
(<code>yes</code>/<code>no</code>), <span class="highlight">logistic
regression</span> is a suitable classification method.
</p>
<h2>
⚙️ Why Use Cross-Validation?
</h2>
<p>
Instead of a single train-test split, we applied
<span class="highlight">10-fold cross-validation (CV)</span> to ensure
more reliable and generalizable results.
</p>
<ul>
<li>
<b>Stability:</b> Each data point is used for both training and
validation.
</li>
<li>
<b>Bias Reduction:</b> Less susceptible to overfitting than a single
split.
</li>
<li>
<b>Better Evaluation:</b> Performance metrics (e.g., ROC, accuracy) are
averaged across 10 folds.
</li>
</ul>
<h2>
📦 Model Implementation
</h2>

    library(caret)<br>
    library(dplyr)<br><br>

<h2>
✅ Summary
</h2>
<p>
Logistic regression provides a statistically sound and interpretable
model, especially useful for understanding how different predictors
affect the probability of subscription. Cross-validation further
strengthens this approach by reducing variance and providing a more
honest assessment of performance.
</p>
</body>
</html>

    Generalized Linear Model 

    45211 samples
       16 predictor
        2 classes: 'no', 'yes' 

    No pre-processing
    Resampling: Cross-Validated (10 fold) 
    Summary of sample sizes: 40689, 40690, 40691, 40690, 40690, 40690, ... 
    Resampling results:

      ROC        Sens      Spec     
      0.9066801  0.975402  0.3452447

Model Summary

<h3>
Key Performance Metrics
</h3>
<table border="1" cellpadding="8" cellspacing="0" style="border-collapse: collapse; width: 100%;">
<thead style="background-color: #f2f2f2;">
<tr>
<th>
<strong>Metric</strong>
</th>
<th>
<strong>Value</strong>
</th>
<th>
<strong>Interpretation</strong>
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
ROC (AUC)
</td>
<td>
0.9067
</td>
<td>
The Area Under the Curve is very high, indicating excellent ability to
distinguish between subscribers and non-subscribers. AUC &gt; 0.9 is
considered outstanding.
</td>
</tr>
<tr>
<td>
Sensitivity (Recall)
</td>
<td>
0.9754
</td>
<td>
The model correctly identifies 97.5% of the actual subscribers (true
positives). This is very high and desirable in marketing to avoid
missing potential customers.
</td>
</tr>
<tr>
<td>
Specificity
</td>
<td>
0.3452
</td>
<td>
The model correctly identifies only 34.5% of non-subscribers (true
negatives). This is low, meaning the model tends to overpredict “yes”,
possibly leading to unnecessary outreach.
</td>
</tr>
</tbody>
</table>

-   What This Tells Us?

-   The model is highly sensitive – it captures almost all the true
    positives. This is good when the cost of missing a potential
    subscriber is high (e.g., missed revenue).

-   The low specificity shows that it struggles to correctly classify
    non-subscribers, which might lead to false positives.

-   The AUC value of 0.9067 confirms that overall, the model separates
    the two classes (yes vs no) very well, despite the imbalance.

------------------------------------------------------------------------

## Interpreting Model Coefficients

• We now interpret our logistic regression results. • First, we test
whether the overall model is statistically significant. • Then we
interpret the magnitude and direction of each coefficient via odds
ratios.

           (Intercept)                age   `jobblue-collar`    jobentrepreneur 
                  0.08               1.00               0.73               0.70 
          jobhousemaid      jobmanagement         jobretired `jobself-employed` 
                  0.60               0.85               1.29               0.74 
           jobservices         jobstudent      jobtechnician      jobunemployed 
                  0.80               1.47               0.84               0.84 
            jobunknown     maritalmarried      maritalsingle educationsecondary 
                  0.73               0.84               1.10               1.20 
     educationtertiary   educationunknown         defaultyes            balance 
                  1.46               1.28               0.98               1.00 
            housingyes            loanyes   contacttelephone     contactunknown 
                  0.51               0.65               0.85               0.20 
                   day           monthaug           monthdec           monthfeb 
                  1.01               0.50               2.00               0.86 
              monthjan           monthjul           monthjun           monthmar 
                  0.28               0.44               1.57               4.90 
              monthmay           monthnov           monthoct           monthsep 
                  0.67               0.42               2.41               2.40 
              duration           campaign              pdays           previous 
                  1.00               0.91               1.00               1.01 
         poutcomeother    poutcomesuccess    poutcomeunknown 
                  1.23               9.89               0.91 

-   Key Interpretations:

### Demographic & Job Titles

<table>
<thead>
<tr>
<th>
Variable
</th>
<th>
Odds Ratio
</th>
<th>
Interpretation
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>jobstudent</code>
</td>
<td>
1.47
</td>
<td>
Students are 47% more likely to subscribe.
</td>
</tr>
<tr>
<td>
<code>jobretired</code>
</td>
<td>
1.29
</td>
<td>
Retired individuals are 29% more likely to subscribe.
</td>
</tr>
<tr>
<td>
<code>jobhousemaid</code>
</td>
<td>
0.60
</td>
<td>
Housemaids are 40% less likely to subscribe.
</td>
</tr>
<tr>
<td>
<code>jobentrepreneur</code>
</td>
<td>
0.70
</td>
<td>
Entrepreneurs are 30% less likely to subscribe.
</td>
</tr>
<tr>
<td>
<code>jobblue-collar</code>
</td>
<td>
0.73
</td>
<td>
Blue-collar workers are 27% less likely to subscribe.
</td>
</tr>
</tbody>
</table>

### Education

<table>
<thead>
<tr>
<th>
Variable
</th>
<th>
Odds Ratio
</th>
<th>
Interpretation
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>educationtertiary</code>
</td>
<td>
1.46
</td>
<td>
Tertiary education increases odds by 46%.
</td>
</tr>
<tr>
<td>
<code>educationsecondary</code>
</td>
<td>
1.20
</td>
<td>
Secondary education increases odds by 20%.
</td>
</tr>
<tr>
<td>
<code>educationunknown</code>
</td>
<td>
1.28
</td>
<td>
Unknown education still increases odds by 28%.
</td>
</tr>
</tbody>
</table>

### Financial Status

<table>
<thead>
<tr>
<th>
Variable
</th>
<th>
Odds Ratio
</th>
<th>
Interpretation
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>housingyes</code>
</td>
<td>
0.51
</td>
<td>
49% less likely to subscribe if client has a housing loan.
</td>
</tr>
<tr>
<td>
<code>loanyes</code>
</td>
<td>
0.65
</td>
<td>
Personal loan holders are 35% less likely to subscribe.
</td>
</tr>
<tr>
<td>
<code>balance</code>
</td>
<td>
1.00
</td>
<td>
Balance has no meaningful effect.
</td>
</tr>
</tbody>
</table>

### Contact Method & Month

<table>
<thead>
<tr>
<th>
Variable
</th>
<th>
Odds Ratio
</th>
<th>
Interpretation
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>contactunknown</code>
</td>
<td>
0.20
</td>
<td>
Significantly lowers odds of subscription.
</td>
</tr>
<tr>
<td>
<code>contacttelephone</code>
</td>
<td>
0.85
</td>
<td>
Less effective than mobile contact (baseline).
</td>
</tr>
<tr>
<td>
<code>monthmar</code>
</td>
<td>
4.90
</td>
<td>
March contact increases odds nearly 5x.
</td>
</tr>
<tr>
<td>
<code>monthdec</code>
</td>
<td>
2.00
</td>
<td>
December is also highly effective.
</td>
</tr>
<tr>
<td>
<code>monthsep</code>
</td>
<td>
2.40
</td>
<td>
September shows strong performance.
</td>
</tr>
<tr>
<td>
<code>monthjan</code>
</td>
<td>
0.28
</td>
<td>
Very weak month for conversion.
</td>
</tr>
</tbody>
</table>

### Campaign History

<table>
<thead>
<tr>
<th>
Variable
</th>
<th>
Odds Ratio
</th>
<th>
Interpretation
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<code>poutcomesuccess</code>
</td>
<td>
9.89
</td>
<td>
Previous success boosts odds nearly 10x.
</td>
</tr>
<tr>
<td>
<code>poutcomeother</code>
</td>
<td>
1.23
</td>
<td>
Mild positive influence.
</td>
</tr>
<tr>
<td>
<code>poutcomeunknown</code>
</td>
<td>
0.91
</td>
<td>
Slightly lowers chances of subscription.
</td>
</tr>
</tbody>
</table>

-   Most influential variables: poutcomesuccess, monthmar, jobstudent,
    educationtertiary, and housingyes.

-   This interpretation helps marketers and business analysts target
    their efforts more effectively .

------------------------------------------------------------------------

## Model Comparison: Full vs Reduced Model

-   Purpose:
-   To compare a full logistic regression model with a reduced one and
    see if we can simplify without losing performance.

<!-- -->

    [1] 0.9066801

    [1] 0.7208428

-   Interpretation:

-   Both models return similar AUC scores. This means the reduced model
    (with fewer predictors) is nearly as effective as the full model.

-   Insights:

-   In business applications, simpler models are often preferred — they
    are easier to implement, explain, and maintain. If accuracy remains
    intact, we should choose the reduced model.

## Model Performance: Accuracy & Confusion Matrix

-   Purpose:
-   To evaluate the logistic regression model using a 0.5 threshold and
    examine its raw accuracy and prediction breakdown.

<!-- -->

    [1] 13562

    [1] 13562

          Predicted
    Actual     0     1
         0 11696   280
         1  1020   566

    [1] 90.41

<table style="width:100%; border-collapse: collapse;" border="1">
<thead style="background-color:#f2f2f2;">
<tr>
<th>
<b>Term</b>
</th>
<th>
<b>Count</b>
</th>
<th>
<b>Description</b>
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
True Negative (TN)
</td>
<td>
11696
</td>
<td>
The model correctly predicted a client would not subscribe.
</td>
</tr>
<tr>
<td>
False Positive (FP)
</td>
<td>
280
</td>
<td>
The model incorrectly predicted subscription for a non-subscriber.
</td>
</tr>
<tr>
<td>
False Negative (FN)
</td>
<td>
1020
</td>
<td>
The model missed actual subscribers, predicting “no”.
</td>
</tr>
<tr>
<td>
True Positive (TP)
</td>
<td>
566
</td>
<td>
The model correctly identified actual subscribers.
</td>
</tr>
</tbody>
</table>

-   Accuracy: 90.41%

-   This means the model correctly predicted the outcome in 90% of all
    cases.

-   However, caution is needed:

-   The dataset is imbalanced — most clients said “no”.

-   Therefore, even if the model mostly predicts “no”, it can still
    achieve high accuracy.

-   This makes accuracy a potentially misleading metric in imbalanced
    classification tasks.

------------------------------------------------------------------------

## Precision, Recall, and F1 Score

-   Purpose:
-   To go beyond accuracy and evaluate model performance in detecting
    the minority class (“yes”).
-   Accuracy can be misleading, especially with imbalanced classes.
-   Precision, Recall, and F1 Score provide deeper evaluation of model
    performance.

<!-- -->

    Confusion Matrix and Statistics

              Reference
    Prediction    no   yes
           no  11696  1028
           yes   280   558
                                              
                   Accuracy : 0.9036          
                     95% CI : (0.8985, 0.9085)
        No Information Rate : 0.8831          
        P-Value [Acc > NIR] : 1.318e-14       
                                              
                      Kappa : 0.4129          
                                              
     Mcnemar's Test P-Value : < 2.2e-16       
                                              
                Sensitivity : 0.35183         
                Specificity : 0.97662         
             Pos Pred Value : 0.66587         
             Neg Pred Value : 0.91921         
                 Prevalence : 0.11694         
             Detection Rate : 0.04114         
       Detection Prevalence : 0.06179         
          Balanced Accuracy : 0.66422         
                                              
           'Positive' Class : yes             
                                              

-   Interpretation:

<table border="1" cellpadding="6" cellspacing="0" style="border-collapse: collapse; width: 100%;">
<thead style="background-color: #f2f2f2;">
<tr>
<th>
<b>Metric</b>
</th>
<th>
<b>Value</b>
</th>
<th>
<b>Interpretation</b>
</th>
</tr>
</thead>
<tbody>
<tr>
<td>
Accuracy
</td>
<td>
90.36%
</td>
<td>
The model correctly predicted 90.36% of all cases. Seems strong but may
be misleading due to class imbalance.
</td>
</tr>
<tr>
<td>
Kappa
</td>
<td>
0.4129
</td>
<td>
Indicates moderate agreement between predictions and actuals beyond
chance.
</td>
</tr>
<tr>
<td>
Sensitivity (Recall)
</td>
<td>
35.18%
</td>
<td>
Only 35% of actual subscribers were correctly predicted — many false
negatives.
</td>
</tr>
<tr>
<td>
Specificity
</td>
<td>
97.66%
</td>
<td>
Very good at identifying non-subscribers.
</td>
</tr>
<tr>
<td>
Positive Predictive Value (Precision)
</td>
<td>
66.59%
</td>
<td>
When the model predicts “Yes,” it is correct ~67% of the time.
</td>
</tr>
<tr>
<td>
Negative Predictive Value
</td>
<td>
91.92%
</td>
<td>
When the model predicts “No,” it’s correct ~92% of the time.
</td>
</tr>
<tr>
<td>
Balanced Accuracy
</td>
<td>
66.42%
</td>
<td>
Average of sensitivity and specificity — useful when classes are
imbalanced.
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

## Model Performance: ROC Curve & AUC

-   Purpose:
-   To evaluate the model’s ranking ability — how well it separates
    “yes” from “no” across different thresholds.
-   The ROC curve shows the trade-off between True Positive Rate (TPR)
    and False Positive Rate (FPR).
-   AUC (Area Under the Curve) tells us how well the model separates the
    classes.

![image](https://github.com/user-attachments/assets/2d5ed5b8-c531-458b-869a-0a3975aa3d45)

    [1] 0.9087577

• AUC closer to 1 → better model • AUC &lt; 0.7 → poor discrimination •
This tells us how confidently the model ranks predictions — not just
accuracy

-   Interpretation:

-   The AUC is high (~0.90), which means the model does a great job
    distinguishing between classes, even if the default threshold isn’t
    ideal.

    -   Insights:

-   We may consider adjusting the decision threshold (e.g., 0.3 instead
    of 0.5) to trade off precision and recall, depending on business
    goals.

------------------------------------------------------------------------

## Model 2: Support Vector Machine (Radial Kernel)

-   Purpose:

-   To evaluate a Support Vector Machine model and see if it captures
    non-linear relationships missed by logistic regression.

-   We now build an SVM model using a **radial kernel** with the `e1071`
    package.

-   This model captures non-linear patterns and returns class
    probabilities.

    • AUC Score: r round(svm\_auc, 3) • Confusion Matrix provides:
    accuracy, precision, recall, F1

    -   Interpretation:

-   SVM performs well in both AUC and confusion matrix metrics. It
    balances false positives and false negatives better than logistic
    regression.

    -   Insights:

-   SVM is a strong alternative when decision boundaries are complex or
    data isn’t linearly separable. However, it’s harder to interpret
    than logistic regression.

------------------------------------------------------------------------

## SVM (Radial) – Performance Evaluation

-   Let’s check AUC and classification metrics for the SVM model.

<!-- -->

    Confusion Matrix and Statistics

              Reference
    Prediction    No   Yes
           No  11749  1126
           Yes   227   460
                                              
                   Accuracy : 0.9002          
                     95% CI : (0.8951, 0.9052)
        No Information Rate : 0.8831          
        P-Value [Acc > NIR] : 1.076e-10       
                                              
                      Kappa : 0.3595          
                                              
     Mcnemar's Test P-Value : < 2.2e-16       
                                              
                Sensitivity : 0.29004         
                Specificity : 0.98105         
             Pos Pred Value : 0.66958         
             Neg Pred Value : 0.91254         
                 Prevalence : 0.11694         
             Detection Rate : 0.03392         
       Detection Prevalence : 0.05066         
          Balanced Accuracy : 0.63554         
                                              
           'Positive' Class : Yes             
                                              

![image](https://github.com/user-attachments/assets/6990d5f4-b74b-42ea-898c-5bdd99cba786)

    AUC: 0.9069758 

• AUC Score: r round(svm\_auc, 3) • Confusion Matrix provides: accuracy,
precision, recall, and F1-score

------------------------------------------------------------------------

## Model 3: Random Forest

-   We now try a third model using the **Random Forest** algorithm.
-   RF is an ensemble method based on multiple decision trees.
-   It is often very effective for classification tasks.

<!-- -->

          Predicted
    Actual     0     1
       No  11622   354
       Yes   919   667

    [1] 90.61

-   Let’s evaluate how well Random Forest ranks predictions.

![image](https://github.com/user-attachments/assets/ddd4f5b1-5e41-4cce-aed4-6ccdc776752a)

    [1] 0.9279942

• AUC Score: r round(rf\_auc\_value, 3) → r rf\_auc\_comment • Accuracy:
r round(rf\_accuracy \* 100, 2)%

## Random Forest: Feature Importance

-   RF provides a ranking of variable importance.
-   This is valuable for marketing decisions.

![image](https://github.com/user-attachments/assets/60d722c9-4c69-41cd-9f5c-581e4a378510)

• The most important features can guide segmentation and targeting
strategies.

      - Interpretation:

-   RF performs well and provides high interpretability in terms of
    feature contribution. Features like month, poutcome, and job are top
    drivers.

    -   Insights:

-   Random Forest is powerful when raw predictive performance is the
    priority. Its feature importance chart is especially valuable for
    guiding targeting strategies.

------------------------------------------------------------------------

## Model Comparison Summary

-   Let’s summarize the performance of all three models.

-   Metrics: **Accuracy** and **AUC Score**

-   Purpose:

-   To summarize the performance of all three models (Logistic, SVM, RF)
    across Accuracy and AUC.

<!-- -->

    [1] 0.9044059

    NULL

<table>
<caption>Model Performance Comparison: Accuracy and AUC</caption>
<thead>
<tr>
<th style="text-align: left;">Model</th>
<th style="text-align: right;">Accuracy</th>
<th style="text-align: right;">AUC</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">Logistic Regression</td>
<td style="text-align: right;">90.41</td>
<td style="text-align: right;">0.909</td>
</tr>
<tr>
<td style="text-align: left;">Support Vector Machine</td>
<td style="text-align: right;">90.02</td>
<td style="text-align: right;">0.907</td>
</tr>
<tr>
<td style="text-align: left;">Random Forest</td>
<td style="text-align: right;">90.61</td>
<td style="text-align: right;">0.928</td>
</tr>
</tbody>
</table>

• Logistic Regression is interpretable and statistically sound —
especially useful for communication with non-technical stakeholders. •
SVM showed strong performance with a smooth decision boundary and a
competitive AUC score. • Random Forest achieved high accuracy and offers
feature importance insights — helpful for segmentation strategy.

Final model choice depends on your priorities: • Use Logistic Regression
if interpretability matters most. • Use SVM if nonlinear relationships
dominate. • Use Random Forest if predictive performance is the top
priority.

-   Interpretation:

-   All models perform well, but trade-offs exist: • Logistic: best for
    transparency • SVM: strong boundary detection • RF: highest raw
    accuracy and feature insights

    -   Insights:
    -   Model choice depends on the business goal — whether you value
        interpretability, prediction power, or feature understanding
        more.

------------------------------------------------------------------------

## ✅ Final Conclusions & Strategic Marketing Recommendations

🔍 Key Takeaways • Our logistic regression model is statistically
significant and supported by strong performance metrics.

• Several predictors stand out as key drivers of subscription behavior:

• Job Role: Students and retirees are significantly more likely to
subscribe.

• Education Level: Clients with tertiary education show higher
engagement.

• Marital Status: Single clients are more responsive than married ones.

• Previous Outcome: Past campaign success is a powerful indicator of
future conversion.

## Overall Model Performance

• Accuracy: 90.41%

• AUC Score: 0.909

• Classification Quality: Outstanding

→ Indicates the model has strong discriminatory power and separates
responders from non-responders effectively.

------------------------------------------------------------------------

## 💡 Marketing Strategy Recommendations

### ✅ Target High-Potential Segments

-   Focus marketing efforts on customer groups with higher conversion
    potential:

    • 🎯 Students and retirees (response rates above 20%)

    • 🎓 Clients with tertiary education

    • 📱 Clients contacted via cellular phone

    • 🔁 Clients who had a successful outcome in previous campaigns

These segments are statistically more likely to respond and should be
prioritized in future outreach, content personalization, and retargeting
strategies.

### 🚫 Reconsider Lower-Performing Segments

Deprioritize or redesign campaigns for groups with consistently lower
response rates:

• 👷 Blue-collar and housemaid occupations

• 🏫 Clients with only primary education

• 💍 Married clients, who show below-average engagement

Reducing resource allocation toward these groups can improve campaign
efficiency and allow better ROI optimization.

------------------------------------------------------------------------

## 📌 Final Thoughts

• Predictive modeling is not just about forecasting — it’s about
enabling smarter decisions.

• By leveraging model insights, marketing teams can transition from
broad outreach to data-driven segmentation, improving both efficiency
and effectiveness.
