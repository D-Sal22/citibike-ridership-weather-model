# Citibike Ridership Model

This model explores how weather and weekly rhythms influence  New York City’s Citibike daily ridership program. In this project we build a daily ridership dataset for NYC Citibike by combining trip records with NOAA weather data. Then in part 2, we will train a model to predict daily demand.

## Repository Structure

- **code/**  
  Contains Jupyter notebooks for EDA, preprocessing, and modeling.

- **data/**  
  Stores the exported CSV dataset created in Step 9.

- **queries/**  
  Contains the final SQL queries used to build the modeling dataset.

- **docs/**  
  Notes, data dictionary, and supporting materials.

## Overview

Part 1: Use BigQuery public datasets to aggregate daily ridership and join weather data.  
Part 2: Build a predictive model using the engineered dataset.

## M7: Project Summary ##
This project builds a daily Citi Bike ridership dataset by combining NYC trip data with NOAA GSOD daily weather observations. The goal was to engineer a clean modeling dataset, diagnose data quality issues, build a linear regression model, and evaluate how well weather and calendar features explain daily ridership.

                            Data Quality Issues & How They Were Handled
The NOAA weather data contained several inconsistencies that required cleaning before modeling. The most significant issue was the presence of sentinel values, including a precipitation value of 99.99, which is not physically realistic and would distort a linear model. Precipitation was also heavily zero inflated, with most days reporting no rainfall. To address this, I removed sentinel values and later capped precipitation at 5 inches, since NOAA GSOD rarely reports more than ~5 inches of daily rainfall and the dataset included a realistic maximum of 4.88 inches. This prevented extreme outliers from influencing the model.

On the ridership side, I aggregated millions of trip records into daily counts and ensured that each date aligned correctly with its corresponding weather observations. Missing weather days were handled so the dataset remained consistent and complete.

I engineered several features to capture meaningful patterns in ridership:

•	Temperature features (average, max, min) to represent comfort and seasonal effects
•	Wind speed and precipitation to capture additional weather impacts
•	Day-of-week encoding to model weekday vs. weekend behavior
•	days_since_launch, a trend feature to represent long-term system growth

These features were chosen based on EDA findings showing strong seasonal patterns, weekend effects, and upward ridership trends over time.

                                        Model Performance 
The baseline linear regression model achieved a test R^2 of 0.726, meaning it explains about 73% of the variance in daily ridership. Residual diagnostics showed no major structural issues: residuals were centered around zero, with no long-term drift and no nonlinear patterns. The largest residual spikes corresponded to holidays, storms, outages, or potentially major NYC events that the model cannot capture with the available features.

After capping precipitation at 5 inches, the improved model achieved a slightly higher test R^2 of 0.733, indicating a modest reduction in noise from extreme values.

                              Biggest Weakness & Needed Future Data 
The model’s biggest weakness is its inability to capture special-event days—holidays, extreme weather events, system outages, and large-scale city events that dramatically affect ridership. These anomalies create large residual spikes that a simple linear model cannot explain. To improve future versions, the dataset should include:

•	Holiday indicators
•	Severe weather alerts
•	Snowfall and storm severity
•	System outage logs
•	Major NYC event calendars

Adding these features would significantly improve predictive accuracy and reduce unexplained variability.

