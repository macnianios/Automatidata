# Automatidata
Google Advanced Data Analytics Project: Analyzing taxi fares for a New York-based company and building a predictive model.

# Overview
Automatidata specializes in providing data-driven insights to help businesses optimize their operations and make informed decisions.

For this case study, Automatidata has been hired by a client in the transportation industry to analyze and improve their pricing strategy. The client operates a fleet of taxis and wants to better understand the factors that influence taxi fares in New York City. The ultimate goal is to develop a model that can predict taxi fares based on trip details such as distance, time, and other relevant variables.

# Business Understanding
The client has identified inconsistencies in fare pricing, leading to customer complaints and potential revenue loss. They need a predictive model to estimate fares more accurately, ensuring fair pricing for both passengers and drivers. By leveraging historical trip data, we aim to:
* Identify key factors influencing taxi fares.
* Build a machine learning model to predict fares.
* Provide actionable insights to improve pricing strategies.

# Data Understanding
| Column Name            | Description  |
|------------------------|-------------|
| **ID**                | Trip identification number |
| **VendorID**          | A code indicating the TPEP provider that provided the record. <br> 1 = Creative Mobile Technologies, LLC <br> 2 = VeriFone Inc. |
| **tpep_pickup_datetime** | The date and time when the meter was engaged. |
| **tpep_dropoff_datetime** | The date and time when the meter was disengaged. |
| **Passenger_count**    | The number of passengers in the vehicle. This is a driver-entered value. |
| **Trip_distance**      | The elapsed trip distance in miles reported by the taximeter. |
| **PULocationID**      | TLC Taxi Zone in which the taximeter was engaged. |
| **DOLocationID**      | TLC Taxi Zone in which the taximeter was disengaged. |
| **RateCodeID**        | The final rate code in effect at the end of the trip. <br> 1 = Standard rate <br> 2 = JFK <br> 3 = Newark <br> 4 = Nassau or Westchester <br> 5 = Negotiated fare <br> 6 = Group ride |
| **Store_and_fwd_flag** | This flag indicates whether the trip record was held in vehicle memory before being sent to the vendor (store and forward). <br> Y = store and forward trip <br> N = not a store and forward trip |
| **Payment_type**      | A numeric code signifying how the passenger paid for the trip. <br> 1 = Credit card <br> 2 = Cash <br> 3 = No charge <br> 4 = Dispute <br> 5 = Unknown <br> 6 = Voided trip |
| **Fare_amount**       | The time-and-distance fare calculated by the meter. |
| **Extra**            | Miscellaneous extras and surcharges, including the $0.50  and $1 rush hour and overnight charges. |
| **MTA_tax**          | \$0.50 MTA tax that is automatically triggered based on the metered rate in use. |
| **Improvement_surcharge** | $0.30 improvement surcharge assessed at the flag drop. Began in 2015. |
| **Tip_amount**     | Tip amount – Automatically populated for credit card tips. Cash tips are not included. |
| **Tolls_amount**     | Total amount of all tolls paid in the trip. |
| **Total_amount**     | The total amount charged to passengers (excluding cash tips). |


* **Ride Distribution** Wednesday through Saturday had the highest number of daily rides, while Sunday and Monday had the least.
* **Seasonal Patterns:** July and August had the lowest ride counts, while March saw the highest with over 2,000 rides.
* **Tipping Behavior:** Only customers who paid with a card left tips, with an average tip rate of 15.5%.
* **Payment Method:** Trips paid with a card had an average duration 1 minute longer than those paid with cash.
* **Pickup and Drop-off Locations:** The top 10 pickup locations matched the top 10 drop-off locations, suggesting they are high-traffic areas like business districts and transit hubs.
* **Fare Distribution:** The fare amount distribution is highly skewed with a concentration at lower values, indicating that most trips are short, with a few long-distance trips driving higher fare amounts.
* **Airport Trips:** Trips to the airport have a fixed fare of 52.
* **Linear Correlation:** There is some linear correlation between fare amount, duration, and distance, indicating that longer trips and those covering greater distances tend to have higher fares.
* **Outliers:** There are many outliers in the data that may need further examination. 

<img src="https://github.com/user-attachments/assets/af2a8a2f-24ec-43b1-9353-eef39478e557" width="600" height="200">

<img src="https://github.com/user-attachments/assets/cc6eb095-ebb4-4e2b-abce-9c3e89294226" width="300" height="200"> <img src="https://github.com/user-attachments/assets/4d85ea7e-8dff-43aa-9c0d-392c32ef03c4" width="300" height="200">

# Modeling and Evaluation
Since fare_amount is a continuous numerical variable, we chose a linear regression model.


| Metric | Value |
|--------|-------|
| R²     | 0.9485 |
| MAE    | 0.8282 |
| MSE    | 5.6688 |
| RMSE   | 2.3809 |


<img src="https://github.com/user-attachments/assets/3f541ea1-ce26-4e13-b500-d13cc0697f44" width="450" height="300">

# Analysis of Model Performance:

Our model demonstrates strong overall performance metrics. However, a significant performance degradation is observed for fare amounts exceeding \$25. This is attributed to the data's highly skewed distribution, with approximately 90% of observations falling below \$20. Furthermore, a distinct concentration of actual values is identified at \$62, likely a result of outlier treatment.

Despite these limitations, the model exhibits satisfactory performance within the dominant lower-value range. To address the performance disparity for higher fare amounts, we propose the following strategies:

* Oversampling: Implement oversampling techniques to increase the representation
of data points with fare amounts greater than \$25, thereby mitigating the impact of the skewed distribution.
* Post-Prediction Scaling: Investigate and potentially apply a scaling factor (e.g., 5%) to predicted values above $25 to improve accuracy in this range.

**Key Observations and Limitations:**

The following contextual factors should be explicitly documented alongside the model results:

* Tip Correlation: Tip amounts are exclusively associated with credit card payment transactions.
* Airport Fare Constraint: Fares originating from or destined for airport locations are fixed at $52.
* Fare Amount Definition: The 'fare amount' variable represents a subset of the total cost and does not encompass additional charges such as tolls and taxes. This should be made clear to anyone reviewing the models results.


**Next Steps:**
* Log Transformation: We could explore applying a log transformation to the fare_amount to reduce skewness and potentially improve model performance.
* Excluding Airport Fares: Another potential step is to exclude airport fares, as they may disproportionately affect the fare amount due to their typically higher values.
* Feature Engineering: We could create additional features related to time zones, such as higher values for rides during peak hours.

