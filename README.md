## Installing Necesarry Libraries

    library(dplyr)
    library(tidyverse)
    library(janitor)
    library(GGally)
    library(corrplot)
    library(ggplot2)

## Importing dataset

    bike <- readr::read_csv("bike_data.csv")

## Looking for the first and last few observation

    head(bike)

    ## # A tibble: 6 × 14
    ##   Date    `Rented Bike Count`  Hour Temperature Humidity `Wind speed` Visibility
    ##   <chr>                 <dbl> <dbl>       <dbl>    <dbl>        <dbl>      <dbl>
    ## 1 1/12/2…                 254     0        -5.2       37          2.2       2000
    ## 2 1/12/2…                 204     1        -5.5       38          0.8       2000
    ## 3 1/12/2…                 173     2        -6         39          1         2000
    ## 4 1/12/2…                 107     3        -6.2       40          0.9       2000
    ## 5 1/12/2…                  78     4        -6         36          2.3       2000
    ## 6 1/12/2…                 100     5        -6.4       37          1.5       2000
    ## # ℹ 7 more variables: `Dew point temperature` <dbl>, `Solar Radiation` <dbl>,
    ## #   Rainfall <dbl>, Snowfall <dbl>, Seasons <chr>, Holiday <chr>,
    ## #   `Functioning Day` <chr>

    tail(bike)

    ## # A tibble: 6 × 14
    ##   Date    `Rented Bike Count`  Hour Temperature Humidity `Wind speed` Visibility
    ##   <chr>                 <dbl> <dbl>       <dbl>    <dbl>        <dbl>      <dbl>
    ## 1 30/11/…                1384    18         4.7       34          1.9       1661
    ## 2 30/11/…                1003    19         4.2       34          2.6       1894
    ## 3 30/11/…                 764    20         3.4       37          2.3       2000
    ## 4 30/11/…                 694    21         2.6       39          0.3       1968
    ## 5 30/11/…                 712    22         2.1       41          1         1859
    ## 6 30/11/…                 584    23         1.9       43          1.3       1909
    ## # ℹ 7 more variables: `Dew point temperature` <dbl>, `Solar Radiation` <dbl>,
    ## #   Rainfall <dbl>, Snowfall <dbl>, Seasons <chr>, Holiday <chr>,
    ## #   `Functioning Day` <chr>

## First Look: Summary and Glimpse

    glimpse(bike)

    ## Rows: 8,760
    ## Columns: 14
    ## $ Date                    <chr> "1/12/2017", "1/12/2017", "1/12/2017", "1/12/2…
    ## $ `Rented Bike Count`     <dbl> 254, 204, 173, 107, 78, 100, 181, 460, 930, 49…
    ## $ Hour                    <dbl> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, …
    ## $ Temperature             <dbl> -5.2, -5.5, -6.0, -6.2, -6.0, -6.4, -6.6, -7.4…
    ## $ Humidity                <dbl> 37, 38, 39, 40, 36, 37, 35, 38, 37, 27, 24, 21…
    ## $ `Wind speed`            <dbl> 2.2, 0.8, 1.0, 0.9, 2.3, 1.5, 1.3, 0.9, 1.1, 0…
    ## $ Visibility              <dbl> 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000…
    ## $ `Dew point temperature` <dbl> -17.6, -17.6, -17.7, -17.6, -18.6, -18.7, -19.…
    ## $ `Solar Radiation`       <dbl> 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00…
    ## $ Rainfall                <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ Snowfall                <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ Seasons                 <chr> "Winter", "Winter", "Winter", "Winter", "Winte…
    ## $ Holiday                 <chr> "No Holiday", "No Holiday", "No Holiday", "No …
    ## $ `Functioning Day`       <chr> "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes…

    summary(bike)

    ##      Date           Rented Bike Count      Hour        Temperature    
    ##  Length:8760        Min.   :   0.0    Min.   : 0.00   Min.   :-17.80  
    ##  Class :character   1st Qu.: 191.0    1st Qu.: 5.75   1st Qu.:  3.50  
    ##  Mode  :character   Median : 504.5    Median :11.50   Median : 13.70  
    ##                     Mean   : 704.6    Mean   :11.50   Mean   : 12.88  
    ##                     3rd Qu.:1065.2    3rd Qu.:17.25   3rd Qu.: 22.50  
    ##                     Max.   :3556.0    Max.   :23.00   Max.   : 39.40  
    ##     Humidity       Wind speed      Visibility   Dew point temperature
    ##  Min.   : 0.00   Min.   :0.000   Min.   :  27   Min.   :-30.600      
    ##  1st Qu.:42.00   1st Qu.:0.900   1st Qu.: 940   1st Qu.: -4.700      
    ##  Median :57.00   Median :1.500   Median :1698   Median :  5.100      
    ##  Mean   :58.23   Mean   :1.725   Mean   :1437   Mean   :  4.074      
    ##  3rd Qu.:74.00   3rd Qu.:2.300   3rd Qu.:2000   3rd Qu.: 14.800      
    ##  Max.   :98.00   Max.   :7.400   Max.   :2000   Max.   : 27.200      
    ##  Solar Radiation     Rainfall          Snowfall         Seasons         
    ##  Min.   :0.0000   Min.   : 0.0000   Min.   :0.00000   Length:8760       
    ##  1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.00000   Class :character  
    ##  Median :0.0100   Median : 0.0000   Median :0.00000   Mode  :character  
    ##  Mean   :0.5691   Mean   : 0.1487   Mean   :0.07507                     
    ##  3rd Qu.:0.9300   3rd Qu.: 0.0000   3rd Qu.:0.00000                     
    ##  Max.   :3.5200   Max.   :35.0000   Max.   :8.80000                     
    ##    Holiday          Functioning Day   
    ##  Length:8760        Length:8760       
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ## 

From here we can see that we have three categorical variables which are
seasons, holiday and functioning day.

## Looking for the number of NA’s

    sum(is.na(bike))

    ## [1] 0

We don’t have any NA in our data. So we don’t need to clean NA’s from
data.

# Cleaning & Looking for the structure of the categorical variables

    bike <- bike |> 
      clean_names()

    table(bike$seasons)

    ## 
    ## Autumn Spring Summer Winter 
    ##   2184   2208   2208   2160

    table(bike$holiday)

    ## 
    ##    Holiday No Holiday 
    ##        432       8328

    table(bike$functioning_day)

    ## 
    ##   No  Yes 
    ##  295 8465

## Looking for the structure of the rainfall and snowfall columns

    table(bike$rainfall)

    ## 
    ##    0  0.1  0.2  0.3  0.4  0.5  0.7  0.8  0.9    1  1.1  1.2  1.3  1.4  1.5  1.6 
    ## 8232   46   20    9   16  116    1    3    3   66    2    1    1    1   56    3 
    ##  1.8  1.9    2  2.4  2.5    3  3.3  3.5  3.7    4  4.5  4.9    5  5.4  5.5    6 
    ##    1    1   31    1   23   14    1   18    1   14    7    1    5    1    8    6 
    ##  6.4  6.5    7  7.3  7.5    8  8.5    9  9.1  9.5   10 10.5 11.5   12 12.5   13 
    ##    2    5    3    1    1    3    2    4    1    6    1    1    1    1    1    2 
    ## 13.5 14.5 15.5   16   17   18 18.5   19   21 21.5   24 29.5   35 
    ##    2    1    1    1    1    2    2    1    1    1    1    1    1

    table(bike$snowfall)

    ## 
    ##    0  0.1  0.2  0.3  0.4  0.5  0.6  0.7  0.8  0.9    1  1.1  1.2  1.3  1.4  1.5 
    ## 8317    2   15   42   21   34   15   31   22   34   39    3    8    4    2    1 
    ##  1.6  1.7  1.8  1.9    2  2.1  2.2  2.3  2.4  2.5  2.6  2.7  2.8  2.9    3  3.1 
    ##   19    3    5    3   22    3   18    3    3   10   12    6    2    2    5    1 
    ##  3.2  3.3  3.4  3.5  3.6  3.7  3.8  3.9    4  4.1  4.2  4.3  4.8    5  5.1    6 
    ##    4    3    2   14    1    3    3    2    4    4    1    2    2    2    1    1 
    ##    7  7.1  8.8 
    ##    1    1    2

## Finding the days with rainfall or snawfall

    bike_filtered <- bike |>
      filter(rainfall > 0 | snowfall > 0) |> 
      select(rainfall, snowfall)

    rain_and_snow_days <- nrow(bike_filtered)
    print(paste("Days with rainfall or snowfall:", rain_and_snow_days))

    ## [1] "Days with rainfall or snowfall: 943"

## Removing dew point temperature data

    bike_wo_dew <- bike |> select(-dew_point_temperature)
    glimpse(bike_wo_dew)

    ## Rows: 8,760
    ## Columns: 13
    ## $ date              <chr> "1/12/2017", "1/12/2017", "1/12/2017", "1/12/2017", …
    ## $ rented_bike_count <dbl> 254, 204, 173, 107, 78, 100, 181, 460, 930, 490, 339…
    ## $ hour              <dbl> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15…
    ## $ temperature       <dbl> -5.2, -5.5, -6.0, -6.2, -6.0, -6.4, -6.6, -7.4, -7.6…
    ## $ humidity          <dbl> 37, 38, 39, 40, 36, 37, 35, 38, 37, 27, 24, 21, 23, …
    ## $ wind_speed        <dbl> 2.2, 0.8, 1.0, 0.9, 2.3, 1.5, 1.3, 0.9, 1.1, 0.5, 1.…
    ## $ visibility        <dbl> 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000…
    ## $ solar_radiation   <dbl> 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.01…
    ## $ rainfall          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ snowfall          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ seasons           <chr> "Winter", "Winter", "Winter", "Winter", "Winter", "W…
    ## $ holiday           <chr> "No Holiday", "No Holiday", "No Holiday", "No Holida…
    ## $ functioning_day   <chr> "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Ye…

## Creating Some Plots

    bike |> select(rented_bike_count:temperature) |> 
          ggpairs(upper = list(continous = wrap("cor", family="sans")))

![1](https://github.com/user-attachments/assets/198af9a6-aaa0-4416-9ead-db5233bb3f79)

There is a moderate positive correlation (0.539) between temperature and
bike rentals, meaning more bikes are rented when the temperature is
higher, but this effect slows down around 30°C. The correlation between
bike rentals and hour (0.410) suggests that rentals increase at certain
times, probably during commuting hours. The relationship between hour
and temperature (0.124) is weak, so temperature changes do not strongly
depend on the time of day. In conclusion, bike rentals are affected by
both temperature and time, with commuting hours being an important
factor.

    bike_new <- bike |> select(-humidity, -wind_speed, -visibility, -dew_point_temperature, -solar_radiation)
    bike_new |> 
        select_if(is.numeric) |>
        cor () |>
        corrplot( type = "upper",
                insig = "blank",
                diag = FALSE,
                addCoef.col = "grey40")

![2](https://github.com/user-attachments/assets/0d9cd22a-e3cf-424d-bfa7-924b8f7db64e)

Rainfall (-0.12) and snowfall (-0.14) have a small negative effect,
meaning bad weather slightly reduces rentals. Temperature and snowfall
are negatively related (-0.22), as lower temperatures lead to more snow.
In summary, bike rentals depend mostly on temperature and time, while
rain and snow have a smaller impact.

    bike |> 
      group_by(hour) |>
      summarise(a_rent=mean(rented_bike_count, na.rm = TRUE)) |> 
      ungroup() |> 
      ggplot(aes(x=hour, y=a_rent)) +
      geom_col(just = 0.5, fill = "#c77cff") + 
      labs(
        title = "Average Rented Bike Count by Hour",
        x = "Hours of Day",
        y = "Average Rented Bike Count"
      ) + 
      theme_minimal() +
      theme(
        panel.grid = element_blank(),
        axis.line = element_line(linewidth = 1, colour = "grey80"))

![3](https://github.com/user-attachments/assets/080476ec-f9e1-4275-bc85-a88e6a09e522)

Rentals are high in the early morning, decrease between 3 AM and 6 AM,
and rise again after 7 AM, peaking around 5 PM. This suggests that bike
rentals are most common during commuting hours, likely when people go to
and return from work. Rentals remain relatively high in the evening but
drop after 9 PM. Overall, the data shows a clear pattern of higher
demand during morning and evening rush hours.

    bike |> 
      group_by(temperature) |>
      summarise(a_rent=mean(rented_bike_count, na.rm = TRUE)) |> 
      ungroup() |> 
      ggplot(aes(x=temperature, y=a_rent)) +
      geom_line(just = 0.5, color = "#c77cff") + 
      labs(
        title = "Average Rented Bike Count by Temperature",
        x = "Temperature",
        y = "Average Rented Bike Count"
      ) + 
      theme_minimal() +
      theme(
        panel.grid = element_blank(),
        axis.line = element_line(linewidth = 1, colour = "grey80"))

    ## Warning in geom_line(just = 0.5, color = "#c77cff"): Ignoring unknown
    ## parameters: `just`

![5](https://github.com/user-attachments/assets/7782d188-b7b1-45e3-92c8-5512f7d30578)

As the temperature rises, the number of rented bikes increases steadily,
reaching a peak between 25 and 30 degrees Celsius. After this peak, the
number of rented bikes starts to decline slightly as the temperature
approaches 40 degrees Celsius. This trend indicates that people prefer
renting bikes in moderate to warm weather, while very cold or extremely
hot temperatures reduce their interest.

    ggplot(bike, aes(x="", y=rented_bike_count, fill=seasons)) +
      geom_bar(stat="identity", width=1) +
      coord_polar("y", start=0) +
      labs(title = "Rented Bike Count by Seasons") +
      theme_void()

![6](https://github.com/user-attachments/assets/2a1555ee-6fa5-40f3-816e-f0aad4f28889)

The largest portion belongs to summer, indicating that most people
prefer to rent bikes during this season. Spring and autumn follow, with
a significant but smaller share of rentals compared to summer. Winter
has the smallest share, meaning bike rentals are less common during
colder months. This pattern suggests that there is a seasonality with
the bike rental numbers.

## Linear Models

    lm_1 <- lm(rented_bike_count ~ hour + temperature + humidity, data = bike)
    summary(lm_1)

    ## 
    ## Call:
    ## lm(formula = rented_bike_count ~ hour + temperature + humidity, 
    ##     data = bike)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1383.41  -287.75   -46.21   217.35  2341.35 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 414.8749    19.8371   20.91   <2e-16 ***
    ## hour         27.1838     0.7711   35.25   <2e-16 ***
    ## temperature  28.9742     0.4393   65.96   <2e-16 ***
    ## humidity     -6.8038     0.2635  -25.82   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 477.7 on 8756 degrees of freedom
    ## Multiple R-squared:  0.4516, Adjusted R-squared:  0.4514 
    ## F-statistic:  2403 on 3 and 8756 DF,  p-value: < 2.2e-16

This linear model explains the relationship between bike rental counts
and three factors: hour, temperature, and humidity. All three factors
are highly significant and influence the bike rental count differently.
Rentals tend to increase with higher values for hour and temperature,
while higher humidity decreases rentals. The model shows a moderate
level of accuracy in explaining the variation in bike rental counts.
Additionally, the results support the idea that time, weather, and
humidity play crucial roles in bike rental behavior.

    lm_2 <- lm(rented_bike_count ~ seasons + holiday + functioning_day, data = bike)
    summary(lm_2)

    ## 
    ## Call:
    ## lm(formula = rented_bike_count ~ seasons + holiday + functioning_day, 
    ##     data = bike)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1026.15  -352.11   -41.94   214.25  2520.85 
    ## 
    ## Coefficients:
    ##                    Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          -17.51      40.87  -0.428    0.668    
    ## seasonsSpring       -172.08      16.94 -10.155  < 2e-16 ***
    ## seasonsSummer        112.06      17.11   6.551 6.04e-11 ***
    ## seasonsWinter       -693.15      17.21 -40.284  < 2e-16 ***
    ## holidayNo Holiday     49.54      27.43   1.806    0.071 .  
    ## functioning_dayYes   891.07      33.84  26.330  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 551.8 on 8754 degrees of freedom
    ## Multiple R-squared:  0.2686, Adjusted R-squared:  0.2682 
    ## F-statistic: 642.9 on 5 and 8754 DF,  p-value: < 2.2e-16

This linear model explores the relationship between bike rentals and
three factors: seasons, holidays, and functioning days. The analysis
shows that the number of bike rentals changes significantly based on the
seasons. Rentals are higher in summer but much lower in winter, while
spring has a negative impact compared to autumn, which is used as the
reference season. Functioning days have a strong positive effect on
rentals, meaning bikes are rented more on days when services are fully
operating. Holidays show a weaker and less consistent effect on rentals.
Overall, the model highlights how seasonal and operational factors
influence bike rental behavior.

    lm_3 <- lm(rented_bike_count ~ temperature + wind_speed + solar_radiation, data = bike)
    summary(lm_3)

    ## 
    ## Call:
    ## lm(formula = rented_bike_count ~ temperature + wind_speed + solar_radiation, 
    ##     data = bike)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1419.83  -323.88   -59.47   223.88  2433.66 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     181.3633    13.0519  13.896  < 2e-16 ***
    ## temperature      28.7589     0.5202  55.286  < 2e-16 ***
    ## wind_speed       81.1404     5.9466  13.645  < 2e-16 ***
    ## solar_radiation  22.4556     7.5782   2.963  0.00305 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 535.7 on 8756 degrees of freedom
    ## Multiple R-squared:  0.3105, Adjusted R-squared:  0.3103 
    ## F-statistic:  1315 on 3 and 8756 DF,  p-value: < 2.2e-16

This linear model examines how bike rentals are influenced by
temperature, wind speed, and solar radiation. All three factors are
significant and impact the rental counts in different ways. Higher
temperature and solar radiation lead to more rentals, while increased
wind speed also positively affects rental counts, though the reason
could relate to better weather conditions overall. The model explains
about 31% of the variation in rental counts, indicating a moderate level
of fit. This analysis shows how environmental factors play a key role in
bike rental behaviors.

    lm_4 <- lm(rented_bike_count ~ hour, data = bike)
    summary(lm_4)

    ## 
    ## Call:
    ## lm(formula = rented_bike_count ~ hour, data = bike)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1144.2  -396.1  -101.8   325.7  2602.9 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  265.016     12.187   21.75   <2e-16 ***
    ## hour          38.225      0.908   42.10   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 588.3 on 8758 degrees of freedom
    ## Multiple R-squared:  0.1683, Adjusted R-squared:  0.1682 
    ## F-statistic:  1772 on 1 and 8758 DF,  p-value: < 2.2e-16

This linear model examines the relationship between bike rental count
and the hour of the day. The analysis shows that hour has a significant
and positive effect on rental counts, meaning more bikes are rented
during later hours of the day. However, the model explains only a small
part of the variation in rental counts, indicating that other factors
also play a major role in influencing bike rentals. Overall, this
analysis highlights the importance of time in rental patterns but
suggests the need for additional predictors for a better understanding
of rental behavior.

