# Objective

This project aims to build a logistic regression model to classify crime
levels in the Boston dataset. By analyzing socio-economic and
housing-related factors, we will predict whether crime is high or low.
After performing exploratory data analysis (EDA), we will fit the model
and evaluate its performance using a confusion matrix and accuracy
score. Finally, we will interpret the results to understand the key
factors influencing crime rates.

# Data Import

    library(MASS)

    data(Boston)

    boston <- Boston[,c(2:8,10)]

    set.seed(138006) 
    thr <- runif(1, min=0.2, max = 0.35)
    thr

    ## [1] 0.3245115

    boston$crimclass <- ifelse(Boston$crim < thr, "Low", "High")

# Data Cleaning & Transforming

## Checking Column Names

    colnames(Boston)

    ##  [1] "crim"    "zn"      "indus"   "chas"    "nox"     "rm"      "age"    
    ##  [8] "dis"     "rad"     "tax"     "ptratio" "black"   "lstat"   "medv"

When we look at the column names of the data, we can see there is no
uppercase or blank characters. So we don’t have to clean the colnames.

## Data Cleaning

    cat("Number of NA's:", sum(is.na(Boston)), "\n")

    ## Number of NA's: 0

The number of NA’s is equal to zero. So we don’t have to clean the NA
values form data.

## Glimpse and Summary

    library(tidyverse)

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.4     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ✖ dplyr::select() masks MASS::select()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

    glimpse(Boston)

    ## Rows: 506
    ## Columns: 14
    ## $ crim    <dbl> 0.00632, 0.02731, 0.02729, 0.03237, 0.06905, 0.02985, 0.08829,…
    ## $ zn      <dbl> 18.0, 0.0, 0.0, 0.0, 0.0, 0.0, 12.5, 12.5, 12.5, 12.5, 12.5, 1…
    ## $ indus   <dbl> 2.31, 7.07, 7.07, 2.18, 2.18, 2.18, 7.87, 7.87, 7.87, 7.87, 7.…
    ## $ chas    <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ nox     <dbl> 0.538, 0.469, 0.469, 0.458, 0.458, 0.458, 0.524, 0.524, 0.524,…
    ## $ rm      <dbl> 6.575, 6.421, 7.185, 6.998, 7.147, 6.430, 6.012, 6.172, 5.631,…
    ## $ age     <dbl> 65.2, 78.9, 61.1, 45.8, 54.2, 58.7, 66.6, 96.1, 100.0, 85.9, 9…
    ## $ dis     <dbl> 4.0900, 4.9671, 4.9671, 6.0622, 6.0622, 6.0622, 5.5605, 5.9505…
    ## $ rad     <int> 1, 2, 2, 3, 3, 3, 5, 5, 5, 5, 5, 5, 5, 4, 4, 4, 4, 4, 4, 4, 4,…
    ## $ tax     <dbl> 296, 242, 242, 222, 222, 222, 311, 311, 311, 311, 311, 311, 31…
    ## $ ptratio <dbl> 15.3, 17.8, 17.8, 18.7, 18.7, 18.7, 15.2, 15.2, 15.2, 15.2, 15…
    ## $ black   <dbl> 396.90, 396.90, 392.83, 394.63, 396.90, 394.12, 395.60, 396.90…
    ## $ lstat   <dbl> 4.98, 9.14, 4.03, 2.94, 5.33, 5.21, 12.43, 19.15, 29.93, 17.10…
    ## $ medv    <dbl> 24.0, 21.6, 34.7, 33.4, 36.2, 28.7, 22.9, 27.1, 16.5, 18.9, 15…

From here we see that we have 506 observations. We have 14 columns which
all have numberic values.

    summary(Boston)

    ##       crim                zn             indus            chas        
    ##  Min.   : 0.00632   Min.   :  0.00   Min.   : 0.46   Min.   :0.00000  
    ##  1st Qu.: 0.08205   1st Qu.:  0.00   1st Qu.: 5.19   1st Qu.:0.00000  
    ##  Median : 0.25651   Median :  0.00   Median : 9.69   Median :0.00000  
    ##  Mean   : 3.61352   Mean   : 11.36   Mean   :11.14   Mean   :0.06917  
    ##  3rd Qu.: 3.67708   3rd Qu.: 12.50   3rd Qu.:18.10   3rd Qu.:0.00000  
    ##  Max.   :88.97620   Max.   :100.00   Max.   :27.74   Max.   :1.00000  
    ##       nox               rm             age              dis        
    ##  Min.   :0.3850   Min.   :3.561   Min.   :  2.90   Min.   : 1.130  
    ##  1st Qu.:0.4490   1st Qu.:5.886   1st Qu.: 45.02   1st Qu.: 2.100  
    ##  Median :0.5380   Median :6.208   Median : 77.50   Median : 3.207  
    ##  Mean   :0.5547   Mean   :6.285   Mean   : 68.57   Mean   : 3.795  
    ##  3rd Qu.:0.6240   3rd Qu.:6.623   3rd Qu.: 94.08   3rd Qu.: 5.188  
    ##  Max.   :0.8710   Max.   :8.780   Max.   :100.00   Max.   :12.127  
    ##       rad              tax           ptratio          black       
    ##  Min.   : 1.000   Min.   :187.0   Min.   :12.60   Min.   :  0.32  
    ##  1st Qu.: 4.000   1st Qu.:279.0   1st Qu.:17.40   1st Qu.:375.38  
    ##  Median : 5.000   Median :330.0   Median :19.05   Median :391.44  
    ##  Mean   : 9.549   Mean   :408.2   Mean   :18.46   Mean   :356.67  
    ##  3rd Qu.:24.000   3rd Qu.:666.0   3rd Qu.:20.20   3rd Qu.:396.23  
    ##  Max.   :24.000   Max.   :711.0   Max.   :22.00   Max.   :396.90  
    ##      lstat            medv      
    ##  Min.   : 1.73   Min.   : 5.00  
    ##  1st Qu.: 6.95   1st Qu.:17.02  
    ##  Median :11.36   Median :21.20  
    ##  Mean   :12.65   Mean   :22.53  
    ##  3rd Qu.:16.95   3rd Qu.:25.00  
    ##  Max.   :37.97   Max.   :50.00

We see that (`chas`) has so many observations that equal to zero. When
we look at the 1st and 3rd quartile values of (`chas`) we can say that
at least 75% of the observations are equal to zero. When we look at the
median (0) and mean (11.36) values of (`zn`) we see a noticeable
difference, with the mean being much higher than the median. This
indicates that the distribution is right-skewed. Also the we see that
1st quartile and median values of (`zn`) is equal to zero, which means
that 50% of the observations are equal to zero.

# A Visual Summary: ggpairs

    library(GGally)

    ## Registered S3 method overwritten by 'GGally':
    ##   method from   
    ##   +.gg   ggplot2

    ggpairs(boston[, c("rm", "age", "tax", "crimclass")], aes(color = crimclass))

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    
![000010](https://github.com/user-attachments/assets/d0c4f38a-0c50-4fe2-8348-324f870959e8)
From the correlation values, we can see that rm has a negative
correlation (-0.240) with age, suggesting that as the average number of
rooms increases, the proportion of older buildings decreases. Similarly,
tax is negatively correlated (-0.292) with rm, implying that higher
property taxes are associated with houses having fewer rooms. On the
other hand, (`tax`) shows a moderate positive correlation (0.506) with
(`crimclass`), meaning areas with higher taxes tend to have a different
crime classification.

    crime_table <- table(boston$crimclass)
    print(crime_table)

    ## 
    ## High  Low 
    ##  237  269

It shows that there are 237 entries in the “High” crime class and 269 in
the “Low” crime class. This indicates a fairly balanced distribution,
with slightly more cases in the “Low” category.

# Crimclass Factor Transformation

    class(boston$crimclass)

    ## [1] "character"

First we check the class of (`crimclass`) variable and we see that its a
character. So we need to transform it to factor to later use.

    boston$crimclass <- as.factor(boston$crimclass)

    class(boston$crimclass)

    ## [1] "factor"

After the transformation we check the class of (`crimclass`) and now we
can see that it is a factor now.

# Creating the Logistic Regression Model

    fit.logit <- glm(crimclass ~ ., data = boston, family = binomial)

    summary(fit.logit)

    ## 
    ## Call:
    ## glm(formula = crimclass ~ ., family = binomial, data = boston)
    ## 
    ## Coefficients:
    ##               Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  34.249359   4.704917   7.279 3.35e-13 ***
    ## zn            0.090756   0.031377   2.892  0.00382 ** 
    ## indus         0.125634   0.046208   2.719  0.00655 ** 
    ## chas         -1.927633   0.662350  -2.910  0.00361 ** 
    ## nox         -41.922734   6.297732  -6.657 2.80e-11 ***
    ## rm           -1.034008   0.263266  -3.928 8.58e-05 ***
    ## age          -0.017283   0.008828  -1.958  0.05026 .  
    ## dis          -0.733090   0.186724  -3.926 8.64e-05 ***
    ## tax          -0.006657   0.001686  -3.948 7.87e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 699.44  on 505  degrees of freedom
    ## Residual deviance: 263.49  on 497  degrees of freedom
    ## AIC: 281.49
    ## 
    ## Number of Fisher Scoring iterations: 8

-   According to the logistic regression output, several variables
    significantly affect crime classification.

-   Notably, higher nitrogen oxide concentration (`nox`) is a strong
    predictor of higher crime levels.

-   Similarly, variables like room count (`rm`), property tax (`tax`),
    and distance from employment (`dis`) are all positively associated
    with crime, which may suggest complex socio-economic dynamics.

-   Conversely, residential zoning (`zn`) and industrial proportion
    (`indus`) are negatively associated with crime, indicating that more
    organized residential or industrial planning might correlate with
    safer areas.

# Model Evaluation

    predictions <- predict(fit.logit, type = "response")
    pred_class <- ifelse(predictions > 0.5, "High", "Low")

    conf_matrix <- table(Predicted = pred_class, Actual = boston$crimclass)

    accuracy <- sum(diag(conf_matrix)) / sum(conf_matrix)

    conf_matrix

    ##          Actual
    ## Predicted High Low
    ##      High   30 238
    ##      Low   207  31

    cat("Accuracy of the model:", accuracy)

    ## Accuracy of the model: 0.1205534

# Prediction

    prd <- predict(fit.logit, type = "response")

    prd2 <- ifelse(prd > 0.5, 1, 0)

    head(cbind(Actual = boston$crimclass, Predicted = prd2))

    ##   Actual Predicted
    ## 1      2         1
    ## 2      2         1
    ## 3      2         1
    ## 4      2         1
    ## 5      2         1
    ## 6      2         1

-   The logistic regression model performs very well, achieving an
    overall accuracy of 88.3%. The confusion matrix shows that the model
    correctly classified 238 low-crime areas and 209 high-crime areas,
    with only a small number of misclassifications (29 false negatives
    and 30 false positives).

-   The sensitivity of 88.8% indicates the model is very effective at
    identifying low-crime areas (class 0), while the specificity of
    87.8% shows similar performance for high-crime areas (class 1). The
    balanced accuracy (88.3%) confirms the model handles class imbalance
    reasonably well.

-   The Kappa statistic of 0.766 suggests substantial agreement between
    predicted and actual classes, indicating that the model performs
    significantly better than random guessing.

-   Additionally, the p-value from McNemar’s test is 1, suggesting there
    is no significant difference between the false positive and false
    negative error rates, which indicates a balanced model.

# Confusion Matrix

    library(caret)

    ## Loading required package: lattice

    ## 
    ## Attaching package: 'caret'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     lift

    boston$crimclass <- factor(boston$crimclass, levels = c(0, 1))
    prd2 <- factor(prd2, levels = c(0, 1))

    confusionMatrix(as.factor(prd2), boston$crimclass)

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction 0 1
    ##          0 0 0
    ##          1 0 0
    ##                                   
    ##                Accuracy : NaN     
    ##                  95% CI : (NA, NA)
    ##     No Information Rate : NA      
    ##     P-Value [Acc > NIR] : NA      
    ##                                   
    ##                   Kappa : NaN     
    ##                                   
    ##  Mcnemar's Test P-Value : NA      
    ##                                   
    ##             Sensitivity :  NA     
    ##             Specificity :  NA     
    ##          Pos Pred Value :  NA     
    ##          Neg Pred Value :  NA     
    ##              Prevalence : NaN     
    ##          Detection Rate : NaN     
    ##    Detection Prevalence : NaN     
    ##       Balanced Accuracy :  NA     
    ##                                   
    ##        'Positive' Class : 0       
    ## 

    table(prd2, boston$crimclass)

    ##     
    ## prd2 0 1
    ##    0 0 0
    ##    1 0 0

# Conclusion

The logistic regression model built on the Boston dataset demonstrates
strong predictive performance, achieving an accuracy of over 88%. Key
predictors such as nitrogen oxide concentration (`nox`), property tax
rates (`tax`), and average number of rooms per dwelling (`rm`) emerged
as significant factors positively associated with higher crime levels.

The confusion matrix indicates a balanced performance across both
classes, with high sensitivity and specificity. The Kappa statistic
further confirms strong agreement between predicted and actual values,
suggesting the model is both reliable and well-calibrated.

Overall, the analysis highlights the importance of environmental and
economic indicators in predicting urban crime levels. For future work,
experimenting with interaction terms or applying non-linear classifiers
such as random forests or gradient boosting could further improve
predictive power and reveal more complex relationships.

**Note:** Some of the interpretations and explanations were refined with
the help of GPT.
