# Objective

The aim of this study is to analyze bike rental data and identify the
factors that affect the number of rentals. We start by cleaning and
transforming the dataset to ensure accuracy. Then, we perform an
exploratory data analysis (EDA) to understand patterns and relationships
between variables. Various visualizations help us observe trends, such
as the impact of temperature, time of day, and weather conditions on
bike rentals. Finally, we build multiple linear regression models to
examine how different factors influence rental numbers. This analysis
provide insights into bike rental behavior and help predict future
trends.

# Data Import

    bike <- readr::read_csv("bike_data.csv")

# Data Cleaning & Transforming

## Checking Column Names

    colnames(bike)

    ##  [1] "Date"                  "Rented Bike Count"     "Hour"                 
    ##  [4] "Temperature"           "Humidity"              "Wind speed"           
    ##  [7] "Visibility"            "Dew point temperature" "Solar Radiation"      
    ## [10] "Rainfall"              "Snowfall"              "Seasons"              
    ## [13] "Holiday"               "Functioning Day"

Rented (`Bike Count`), (`Dew Point Temperature`), (`Solar Radiation`),
(`Wind Speed`) and (`Functioning Day`) columns has a blank character and
all columns has uppercase characters. We need to transform them into the
right format.

## Data Cleaning

    bike <- bike |> 
      clean_names()

    colnames(bike)

    ##  [1] "date"                  "rented_bike_count"     "hour"                 
    ##  [4] "temperature"           "humidity"              "wind_speed"           
    ##  [7] "visibility"            "dew_point_temperature" "solar_radiation"      
    ## [10] "rainfall"              "snowfall"              "seasons"              
    ## [13] "holiday"               "functioning_day"

    cat("Number of NA's:", sum(is.na(bike)), "\n")

    ## Number of NA's: 0

Now we don’t have any blank or uppercase characters in our column names.
Also we don’t have any NA’s in our data.

## Glimpse and Summary

    glimpse(bike)

    ## Rows: 8,760
    ## Columns: 14
    ## $ date                  <chr> "1/12/2017", "1/12/2017", "1/12/2017", "1/12/201…
    ## $ rented_bike_count     <dbl> 254, 204, 173, 107, 78, 100, 181, 460, 930, 490,…
    ## $ hour                  <dbl> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14…
    ## $ temperature           <dbl> -5.2, -5.5, -6.0, -6.2, -6.0, -6.4, -6.6, -7.4, …
    ## $ humidity              <dbl> 37, 38, 39, 40, 36, 37, 35, 38, 37, 27, 24, 21, …
    ## $ wind_speed            <dbl> 2.2, 0.8, 1.0, 0.9, 2.3, 1.5, 1.3, 0.9, 1.1, 0.5…
    ## $ visibility            <dbl> 2000, 2000, 2000, 2000, 2000, 2000, 2000, 2000, …
    ## $ dew_point_temperature <dbl> -17.6, -17.6, -17.7, -17.6, -18.6, -18.7, -19.5,…
    ## $ solar_radiation       <dbl> 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, …
    ## $ rainfall              <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ snowfall              <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ seasons               <chr> "Winter", "Winter", "Winter", "Winter", "Winter"…
    ## $ holiday               <chr> "No Holiday", "No Holiday", "No Holiday", "No Ho…
    ## $ functioning_day       <chr> "Yes", "Yes", "Yes", "Yes", "Yes", "Yes", "Yes",…

From here we see that we have 8760 observations. We have 14 columns as 4
character and 10 numeric. So, (`seasons`), (`holiday`) and
(`functioning_day`) is our categorical variables.

    summary(bike)

    ##      date           rented_bike_count      hour        temperature    
    ##  Length:8760        Min.   :   0.0    Min.   : 0.00   Min.   :-17.80  
    ##  Class :character   1st Qu.: 191.0    1st Qu.: 5.75   1st Qu.:  3.50  
    ##  Mode  :character   Median : 504.5    Median :11.50   Median : 13.70  
    ##                     Mean   : 704.6    Mean   :11.50   Mean   : 12.88  
    ##                     3rd Qu.:1065.2    3rd Qu.:17.25   3rd Qu.: 22.50  
    ##                     Max.   :3556.0    Max.   :23.00   Max.   : 39.40  
    ##     humidity       wind_speed      visibility   dew_point_temperature
    ##  Min.   : 0.00   Min.   :0.000   Min.   :  27   Min.   :-30.600      
    ##  1st Qu.:42.00   1st Qu.:0.900   1st Qu.: 940   1st Qu.: -4.700      
    ##  Median :57.00   Median :1.500   Median :1698   Median :  5.100      
    ##  Mean   :58.23   Mean   :1.725   Mean   :1437   Mean   :  4.074      
    ##  3rd Qu.:74.00   3rd Qu.:2.300   3rd Qu.:2000   3rd Qu.: 14.800      
    ##  Max.   :98.00   Max.   :7.400   Max.   :2000   Max.   : 27.200      
    ##  solar_radiation     rainfall          snowfall         seasons         
    ##  Min.   :0.0000   Min.   : 0.0000   Min.   :0.00000   Length:8760       
    ##  1st Qu.:0.0000   1st Qu.: 0.0000   1st Qu.:0.00000   Class :character  
    ##  Median :0.0100   Median : 0.0000   Median :0.00000   Mode  :character  
    ##  Mean   :0.5691   Mean   : 0.1487   Mean   :0.07507                     
    ##  3rd Qu.:0.9300   3rd Qu.: 0.0000   3rd Qu.:0.00000                     
    ##  Max.   :3.5200   Max.   :35.0000   Max.   :8.80000                     
    ##    holiday          functioning_day   
    ##  Length:8760        Length:8760       
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ## 

We see that solar (`radiation`), (`rainfall`) and (`snowfall`) has so
many observations that equal to zero. When we look at the 1st and 3rd
quartile values of (`snowfall`) and (`rainfall`) we can say that at
least 75% of the observations are equal to zero. When we look at the
median (0.0100) and mean (0.5691) values of (`solar_radiation`) we see a
noticeable difference, with the mean being much higher than the median.
This indicates that the distribution is right-skewed.

## Categorical Variables

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

-   Seasonal Rentals: The total bike rentals are slightly higher during
    Spring (2208) and Summer (2208), with Autumn close behind (2184).
    Winter shows the lowest rental numbers (2160), which aligns with
    colder temperatures affecting demand.

-   Holiday Effect: Rentals during holidays (432) are significantly
    fewer compared to non-holiday periods (8328). This suggests reduced
    demand on holidays, potentially due to travel or leisure
    preferences.

-   Functioning Days: The bikes are operational for most days (8465
    functioning days), while they are non-functional on a minimal number
    of days (295). This indicates a highly reliable service.

## Finding the days with rainfall or snawfall

    bike_filtered <- bike |>
      filter(rainfall > 0 | snowfall > 0) |> 
      select(rainfall, snowfall)

    rain_and_snow_days <- nrow(bike_filtered)
    print(paste("Days with rainfall or snowfall:", rain_and_snow_days))

    ## [1] "Days with rainfall or snowfall: 943"

## Removing dew point temperature data

    bike <- bike |>
      select(-dew_point_temperature)

    colnames(bike)

    ##  [1] "date"              "rented_bike_count" "hour"             
    ##  [4] "temperature"       "humidity"          "wind_speed"       
    ##  [7] "visibility"        "solar_radiation"   "rainfall"         
    ## [10] "snowfall"          "seasons"           "holiday"          
    ## [13] "functioning_day"

As we can see (`dew_point_temperature`) is gone.

# Creating Some Visual Summaries

## ggpairs

    bike |> select(rented_bike_count, temperature, humidity) |> 
          ggpairs(upper = list(continous = wrap("cor", family="sans")))

          
![000012](https://github.com/user-attachments/assets/634368b8-d024-4710-95d5-29c08172571f)
Bike rentals increase with warmer temperatures (correlation: 0.539) but
slightly decrease as humidity rises (correlation: -0.200). Temperature
shows a weak positive link with humidity (correlation: 0.159). Most
values for rentals, temperature, and humidity fall within moderate
ranges. Warmer weather favors rentals, while high humidity slightly
discourages them.

## Bar Plot: Average Rented Bike Count by Hour

    bike |> 
      group_by(hour) |>
      summarise(a_rent=mean(rented_bike_count, na.rm = TRUE)) |> 
      ungroup() |> 
      ggplot(aes(x=hour, y=a_rent)) +
      geom_col(just = 0.5, fill = "steelblue") + 
      labs(
        title = "Average Rented Bike Count by Hour",
        x = "Hours of Day",
        y = "Average Rented Bike Count"
      ) + 
      theme_minimal() +
      theme(
        panel.grid = element_blank(),
        axis.line = element_line(linewidth = 1, colour = "grey80"))

        
![000019](https://github.com/user-attachments/assets/11fb8c67-e025-40f6-82e5-18fb4bb58a9d)
Rentals are high in the early morning, decrease between 3 AM and 6 AM,
and rise again after 7 AM, peaking around 5 PM. This suggests that bike
rentals are most common during commuting hours, likely when people go to
and return from work. Rentals remain relatively high in the evening but
drop after 9 PM. Overall, the data shows a clear pattern of higher
demand during morning and evening rush hours.

## Line Plot: Average Rented Bike Count by Temperature

    bike |>
      mutate(temp_bin = cut(temperature, breaks = seq(min(temperature), max(temperature), by = 2))) |>
      group_by(temp_bin) %>%
      summarise(a_rent = mean(rented_bike_count, na.rm = TRUE)) |>
      ggplot(aes(x = as.numeric(temp_bin), y = a_rent)) +
      geom_line(size = 0.7, color = "steelblue") +
      xlim(0, 30) +
      labs(
        title = "Average Rented Bike Count by Temperature",
        x = "Temperature",
        y = "Average Rented Bike Count"
      ) +
      theme_minimal() +
      theme(
        panel.grid = element_blank(),
        axis.line = element_line(linewidth = 1, colour = "grey80"))

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_line()`).

![000010](https://github.com/user-attachments/assets/27b8884c-ce86-42e3-8ccb-ee8234e56c66)
This graph shows that as the temperature rises, the number of bike
rentals increases, reaching a peak at around 20 degrees Celsius. After
this point, rentals slightly decline. The graph suggests that moderate
temperatures encourage bike rentals, while extreme temperatures might
lead to a decrease in demand. This information could help bike rental
services plan based on weather conditions.

## Dodge Bar Plot: Bike Rentals by Temperature & Seasons

    bike |>
      mutate(temp_bin = cut(temperature, breaks = seq(floor(min(temperature)), ceiling(max(temperature)), by = 2))) |>
      ggplot(aes(x = temp_bin, y = rented_bike_count, fill = seasons)) +
      geom_bar(stat = "identity", position = "dodge") +
      labs(title = "Bike Rentals by Temperature & Seasons",
           x = "Temperature",
           y = "Total Rented Bike Count",
           fill = "Seasons") +
      theme_minimal() +
      theme(panel.grid = element_blank(),
        axis.line = element_line(linewidth = 1, colour = "grey80"),
        axis.text.x = element_text(angle = 90, hjust = 1))
        
![000016](https://github.com/user-attachments/assets/533ab4ee-ac19-4dbe-b575-5e293eea7259)
As we can see the temperature increases, bike rentals grow
significantly, reaching their peak in the range of 24–28°C. However,
beyond this range, there is a slight decline in rentals. Seasonal
variations also play a crucial role in rental numbers. Summer emerges as
the season with the highest rentals, particularly at warmer
temperatures, while Spring and Autumn follow with moderate rentals, with
Spring slightly outperforming Autumn. Winter, on the other hand,
consistently shows the lowest rental numbers, even when temperatures are
relatively higher. Based on these insights, companies can plan bike
availability according to seasonal and temperature trends to efficiently
meet demand. Additionally, marketing strategies can be tailored to
promote bike rentals more actively during Spring and Summer, when the
popularity of biking is at its peak.

# Linear Models

## Creating Dummy Variables

    bike <- bike |>
      mutate(solar_radiation_dummy = ifelse(solar_radiation > 0, 1, 0)) |>
      mutate(rainfall_dummy = ifelse(rainfall > 0, 1, 0)) |>
      mutate(snowfall_dummy = ifelse(snowfall > 0, 1, 0))

## Model I

    lm_1 <- lm(rented_bike_count ~ hour + temperature + humidity + solar_radiation_dummy + snowfall_dummy + rainfall_dummy, data = bike)
    summary(lm_1)

    ## 
    ## Call:
    ## lm(formula = rented_bike_count ~ hour + temperature + humidity + 
    ##     solar_radiation_dummy + snowfall_dummy + rainfall_dummy, 
    ##     data = bike)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1368.97  -286.18   -50.12   223.41  2269.32 
    ## 
    ## Coefficients:
    ##                        Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)            191.7079    21.8590   8.770   <2e-16 ***
    ## hour                    27.6183     0.7519  36.733   <2e-16 ***
    ## temperature             27.5559     0.4786  57.581   <2e-16 ***
    ## humidity                -3.2237     0.2990 -10.782   <2e-16 ***
    ## solar_radiation_dummy  117.5030    11.2011  10.490   <2e-16 ***
    ## snowfall_dummy          13.9581    24.1165   0.579    0.563    
    ## rainfall_dummy        -539.8573    22.7826 -23.696   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 461.5 on 8753 degrees of freedom
    ## Multiple R-squared:  0.4884, Adjusted R-squared:  0.488 
    ## F-statistic:  1393 on 6 and 8753 DF,  p-value: < 2.2e-16

-   Hour and Temperature have significant positive effects on bike
    rentals, with each additional hour or higher temperature leading to
    more rentals.

-   Humidity has a negative effect: as humidity increases, bike rentals
    tend to decrease.,

-   Solar radiation increases bike rentals, while snowfall has no
    significant effect.

-   Rainfall drastically reduces bike rentals, as shown by the negative
    coefficient.

-   The model explains around 48.8% of the variance in bike rentals,
    indicating it’s fairly effective at predicting bike usage.

## Model II

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

-   Working day has a strong positive effect on rentals—bikes are rented
    more on working days.

-   Season effects show that Spring and Winter tend to have fewer
    rentals compared to Summer, which is associated with more rentals.

-   The model explains 26.8% of the variance, showing that while it
    captures some seasonality and holiday effects, it’s less powerful
    than Model I.

## Model III

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

-   Temperature and Wind Speed both have positive effects on rentals:
    warmer temperatures and higher wind speeds are linked to more bike
    usage.

-   Solar radiation remains positively significant, indicating that
    sunny weather encourages more rentals.

-   This model has an R-squared of 31.1%, meaning weather variables are
    important but not sufficient to fully explain the variation in bike
    rentals.

## Model IV

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

-   Hour has a significant positive effect on rentals, with the
    coefficient indicating that bike rentals tend to increase as the day
    progresses.

-   This model explains only 16.8% of the variation, showing that hour
    alone cannot fully predict bike rentals without additional factors.

# Conclusion

In all models, temperature, hour, and weather conditions like rainfall
or solar radiation are crucial in predicting bike rentals. Models that
include seasonal or holiday effects provide additional insights, while
the hour-only model shows that time of day also plays a role, but it’s
not sufficient by itself.

**Note:** Some of the interpretations and explanations were refined with
the help of GPT.
