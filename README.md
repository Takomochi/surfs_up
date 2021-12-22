# Surfs up Analysis

## Overview of the statistical analysis
The client wants information about temperature trends before opening the surf shop. Specifically, the client wants temperature data for the months of June and December in Oahu, in order to determine if the surf and ice cream shop business is sustainable year-round.

- Task 1 : Determine the Summary Statistics for June
- Task 2 : Determine the Summary Statistics for December

The notebook for the analysis is located [here](https://github.com/Takomochi/surfs_up/blob/main/SurfsUp_Challenge.ipynb).

## Resources
- Data: [hawaii.sqlite](https://github.com/Takomochi/surfs_up/blob/main/hawaii.sqlite)
- Software: Python 3.7.10, Jupyter Notebook

## Results
Images below are statisics of temperature for both June and December.

<span>
<img width="132" alt="june_temps" src="https://user-images.githubusercontent.com/85041697/147132288-77fa9b3e-8a9c-46c8-908c-8213011f2561.png">
<img width="146" alt="dec_temps" src="https://user-images.githubusercontent.com/85041697/147132326-dc90a19d-8da0-4709-8e0f-4cc57992a354.png">
</span>

- June has about 2000 more temperature data than December.

- There is about 3°F difference of average temperature between June and December.

- Minimum temperature is 56°F in December while June is 64°F.

## Summary
### Additional Analysis
1. Overall, there is no significant difference in temperature between June and December.<br>
For better understanding about the data, I created precipitation dataframes for June and December.

    ```
    # Extract precipitation data for June and December.
    prcp_june = session.query(Measurement.prcp).filter(extract('month',Measurement.date)==6).all()
    prcp_dec = session.query(Measurement.prcp).filter(extract('month',Measurement.date)==12).all()
    ```

    ```
    # Create dataframes for each month
    prcp_june_df = pd.DataFrame(prcp_june, columns=['June prcps'])
    prcp_dec_df = pd.DataFrame(prcp_dec, columns=['December prcps'])

    # Statistics of each dataframe
    prcp_june_df.describe()
    prcp_dec_df.describe()
    ```

    <span>
    <img width="133" alt="june_prcps" src="https://user-images.githubusercontent.com/85041697/147132363-c9e77c09-8b36-47a0-95ca-6a7da417ba09.png">
    <img width="146" alt="dec_prcps" src="https://user-images.githubusercontent.com/85041697/147132370-0980c13b-1cfb-40d8-8ebd-8061f8ead5be.png"></span>

    On average, December has much higher precipitation than June.


2. By looking at this data, however, it is hard to determine that the surf and ice cream shop business is sustainable year-round. So I created another query that shows average precipitaion for every month.

    ```
    # Get average temperature for every month.
    temps = session.query(Measurement.date, func.avg(Measurement.tobs)).group_by(extract('month',Measurement.date)).all()
    temps = pd.DataFrame(temps, columns=['Month','temp'])

    # Get average precipitation for every month.
    prcp = session.query(Measurement.date, func.avg(Measurement.prcp)).group_by(extract('month',Measurement.date)).all()
    prcp = pd.DataFrame(prcp, columns=['Month','prcp'])
    ```

    Here is the datasets and charts.<br>
    <span>
    <img width="157" alt="avg_temp" src="https://user-images.githubusercontent.com/85041697/147162137-8cf41928-ae71-4bee-97d1-e33dd3c7279b.png">
    <img src="https://user-images.githubusercontent.com/85041697/147162145-12d0ba71-6d4f-4588-86ab-d5832b935958.png"> </span>

    <span>
    <img width="159" alt="avg_prcp" src="https://user-images.githubusercontent.com/85041697/147162173-3852c07c-758e-4d30-ab20-cad3df8d5220.png">
    <img src="https://user-images.githubusercontent.com/85041697/147162175-e4c72b74-f3aa-43b2-9a50-045e789fcc9b.png"></span>

    ### Conclusion
    Temparature is stable as we can see in the first chart. The range is between 68°F and 76°F. The average precipitation in March and December is more than 20mm. Other months such as November and July, also have high average precipitation. 

    Overall, we could say that the weather in Hawaii is stable. However, the business might have a hard time in March and December as we may have high precipitation.
