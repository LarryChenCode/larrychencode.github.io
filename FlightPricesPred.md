# Predicting Flight Prices using PySpark’s MLlib

![Flight Fare Prediction Using Pyspark Mllib Project Banner](images/fligh-price-pred/flight-price-pred.gif)

## Goal:
The goal of the analysis is to predict flight prices with different regression models using Spark’s MLLib.  We chose the topic, “Predicting Flight Prices by Airline, Source City, Destination City, Class and Others Using Multiple Machine Learning Models” 

## Define the Business problem:
We picked this topic because the data was good and could tell us a lot through different machine learning techniques. For example, we could figure out which airlines might charge more and which cities might have more flight delays. This information is useful for companies wanting to better their services and for travelers wanting to avoid delays. We built and tested several models using the data's details, then chose the best one based on how well it predicted outcomes.

## Dateset:
The dataset “[Flight Price Prediction](https://www.kaggle.com/datasets/shubhambathwal/flight-price-prediction/data)” is used from Kaggle. Dataset contains information about flight booking options from the website Easemytrip for flight travel between India's top 6 metro cities. There are 300261 datapoints and 11 features in the cleaned dataset.

## Workflow:
<img src="images/fligh-price-pred/WORKFLOW.png" width="1000" />

## EDA - Key Insights
<img src="images/fligh-price-pred/periodheatmap.png" width="1000" />

1. Early Booking Benefits: There is a clear trend that shows prices are generally lower when the tickets are booked well in advance. For instance, across all departure times, the prices are visibly lower on the right side of the heatmap, which corresponds to a higher number of days left before departure.
2. Time of Day Price Variation: Different times of day have different pricing patterns. Evening and night tend to have higher average prices when booked last minute. However, as the days left increase, the prices for these times decrease. This suggests that for cheaper fares, one should avoid booking evening or night flights at the last minute.

Code for the Heatmap:

```Python
from pandas.api.types import CategoricalDtype

# Define the order for departure times
time_order = ["Late_Night", "Night", "Evening", "Afternoon", "Morning", "Early_Morning"]

# Converting departure_time to an ordered categorical type
pdf['departure_time'] = pd.Categorical(pdf['departure_time'], categories=time_order, ordered=True)

# Sorting the DataFrame based on the ordered category
pdf.sort_values('departure_time', inplace=True)

# Creating a pivot table for the heatmap
pivot_table = pdf.pivot_table(index='departure_time', columns='days_left', values='price', aggfunc='mean')

# To improve readability of large numbers, we can divide by 1000 and add 'K' to represent thousands
pivot_table = pivot_table / 1000

plt.figure(figsize=(20, 12))
ax = sns.heatmap(pivot_table, annot=True, fmt=".1f", cmap="YlGnBu", annot_kws={"size": 7})
plt.title("Average Price Heatmap by Departure Time and Days Left (in K)")
plt.xlabel("Days Left")
plt.ylabel("Departure Time")

# Fix for the missing labels in the y-axis
ax.set_yticklabels(ax.get_yticklabels(), rotation=0)

plt.show()
```

<img src="images/fligh-price-pred/PricevsDuration.png" width="1000" />

1. Price Distribution by Airline: Each airline has a different price range and distribution. For instance, Vistara appears to have a relatively broad price range, while airlines like IndiGo, SpiceJet, AirAsia show a denser concentration of points at the lower end of the price scale, indicating a larger number of more affordable flights.
2. Flight Duration: There is a visible increase in price with the duration of the flight for some airlines, which is expected as longer flights generally cost more. However, this trend is not uniform across all airlines. Some, like AirAsia and IndiGo, seem to offer more consistently priced tickets across different flight durations.

Code for the interactive scatter plot:
```Python
import plotly.express as px

fig = px.scatter(pdf, x="duration", y="price", color="airline", hover_data=['days_left'])
fig.update_layout(title="Interactive Scatter Plot: Price vs Duration by Airline")
fig.show()
```

## Best Model Overview:
 - Model Type: Gradient Boosting Regressor
 - Features Used: Airline, Source City, Departure Time, Stops, Arrival Time, Destination City, Class, Duration, Days Left
 - Framework: PySpark’s MLlib

## Model Performance Analysis:
Price Prediction Pattern: The model exhibits a linear correlation between actual and predicted flight prices, consistent up to approximately INR 70,000. However, there might be deviations beyond this price range.

<img src="images/fligh-price-pred/prediction vs actual.png" width="700" />

## Accuracy Metrics:
 - RMSE (Root Mean Square Error): 1520.00 - Indicates a relatively small average prediction error, suggesting good model precision.
 - R² (Coefficient of Determination): 0.995 - Demonstrates that the model accounts for approximately 99.5% of the variance in the observed data, indicating high predictive power.
 - MAE (Mean Absolute Error): 601.44 - Provides an additional perspective on the average magnitude of the errors in predictions.

## Model Parameters:
 - Best Parameters Identified:
   - Number of Trees: 60
   - Number of Features: 3

## Conclusion:
The Gradient Boosting Regressor model shows a high degree of accuracy in predicting flight prices within the considered feature set. The excellent R² score coupled with a low RMSE and MAE reflects the model's effectiveness. While the model's predictions are mostly linear up to a certain price point, a slight deviation for higher-priced flights is noted, suggesting an area for further investigation and potential refinement of the model.

### Check out more detail on [Github](https://github.com/LarryChenCode/flight_fare_prediction_using_pyspark_mllib)
