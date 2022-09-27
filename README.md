Problem : NYPD conducts too many stop-and-frisks that do not lead to an arrest. 92% of them in the last 10 years have not lead to arrest.


![2011-2021 Stop and Frisk Outcomes](https://user-images.githubusercontent.com/8728172/192606536-a3481bd9-c2f6-4413-bb65-cabf7c4ecdf5.png)

There's a racial disparity problem with stop-and-frisk, making it especially important that NYPD lowers the stops it does that do not lead to an arrest. 



![Not Arrested By Race](https://user-images.githubusercontent.com/8728172/192607158-66c40036-7fdf-4e74-89e4-935aa0df5361.png)


Hypothetical Business Case : NYPD, in partnership with the city cousil has hired me, a data expert to do data analysis and use machine learning to try and limit stops that do not lead to an arrest. They are looking for actionable insights and reccomendations.

Proposed Solution : A time series model which can predict seasonaility of stop-and-frisks that do not lead to an arrest. NYPD can act by limiting stops during times when stops are unlikey to lead to arrest. 

Why time series modeling with this data:
- Crime has provable seasonality[https://pinkerton.com/our-insights/blog/the-seasonality-of-crime]

- Time data doesn't contain demegraphic information. It's unethical to predict which demegraphics of people will commit future crimes. 


Dashboards for Data Analysis: 
- 1,545,827 stops
- 10 Years Since 2011
Features: Precinct, Race, Outcome, Time 


Results: 
![2011 - 2021 NYPD Stop and Frisks by Race, Precinct and Outcome (1)](https://user-images.githubusercontent.com/8728172/192608798-453a0326-4ca3-460b-8960-c2f2b70a4beb.png)[https://public.tableau.com/views/2011-2021NYPDStopandFrisksbyRacePrecinctandOutcome/2011-2021NYPDStopandFrisksbyRacePrecinctandOutcome?:language=en-US&:display_count=n&:origin=viz_share_link&:device=desktop]

- 8,947 stops
- 2021 only
Features: Precinct, Race, Outcome, Time, Suspected Crime, Age, Physical Force Used


Results: 
![Stop and Frisk 2021 Demographic Overview (1)](https://user-images.githubusercontent.com/8728172/192609621-8f8c7133-059f-4000-8a81-9ecb3a5f7036.png)[https://public.tableau.com/views/StopandFrisk2021NYPD/StopandFrisk2021DemographicOverview?:language=en-US&:display_count=n&:origin=viz_share_link&:device=desktop]


MODELING METHODS:

- Predict 6 month's rate of stops that do not lead to an arrest based on monthly resample from 2014-2021
- Target: Last 6 months of data
- Machines ranked by MAE score 

Machines Used:
- Naive
- Random Walk
- ARI Model 
- IMA Model 
- SARIMA Model

Tuning Methods: 
- ACF and PCF Plots to hypothesize term maximum 
- AutoArima to itterate over SARIMA model to test different combinations of terms


MODELING RESULTS:
NAIVE MODEL: 2% off on average 
RANDOM WALK MODEL: 10% off on average 
ARI MODEL: 10% off on average 
IMA MODEL: 10% off on average (rounded slightly lower than other non-naive models) 
SARIMA MODEL: 10% off on average 

MODELING CONCLUSIONS:
Sophisticated modeling does NOT offer predictive capabilities for spikes in not-arrest rates only based on timing of stop. 
Sophisticated modeling DOES offer anomaly detection inferentially for when spikes in not-arrest rates are historically unprecidented. 
Naive modeling's sucsess is plain evidence of NYPD needing to be more receptive to when last month's stops did not lead to arrest and do less stops during those months as a resonable reaction. 

RECOMMENDATIONS TO NYPD: 
- Do less stops if not-arrested rates spiked last month
- Use IMA model to recognize outliers as is shown below

![IMA_in_action](https://user-images.githubusercontent.com/8728172/192613511-a0ab4dd6-3f2b-4848-94b4-098d732bf378.png)

- Retrain the following 10 precinct where the difference is biggest between stops that do and don't lead to arrest: 

![Top_10_Precicnts_Failing_To_Minimize](https://user-images.githubusercontent.com/8728172/192612851-9c2ab9e9-55f3-4e6a-84f1-b0fd398ecb4b.png)


**FUTURE WORK: **
- Try adding more variable to make SARIMAX model (esp interested in Precinct and Suspected Crime) 
- Try a categorical model without using time as a factor


Stop and Frisk Data: https://www1.nyc.gov/site/nypd/stats/reports-analysis/stopfrisk.page

Precinct geojson file: https://www1.nyc.gov/site/planning/data-maps/open-data/districts-download-metadata.page

``` 
├── README.md
├── Crimes_Notebook
├── Stop_and_Frisk_Timeseries_Data_Prep.ipynb                       *** DATA PREP NOTEBOOK
├── Stop_and_Frisk_Timeseries_Notebook.ipynb
├── Stop_and_Frisk_Timeseries_Notebook_Monthly.ipynb                *** MAIN NOTEBOOK
├── images
   ├──NYPD_budget.webp
   ├──2011_2021_SAF_Outcomes.png
   ├──NYPD_budget.webp
   ├──nypd_badge.png
   ├──Not_Arrested_By_Race.png
   ├──seasonal-bars-1200.webp
   ├──2021_SAF_Overview.png
   ├──2011-2021_SAF_Overview.png
   ├──Top_10_Precicnts_Failing_To_Minimize.png
   ├──IMA_in_action.png
├── raw_data
   ├──SF_2011.csv.gz
   ├──SF_2012.csv.gz
   ├──SF_2013.csv.gz
   ├──SF_2014.csv.gz
   ├──SF_2015.csv.gz 
   ├──SF_2016.csv.gz 
   ├──SF_2017.csv.gz
   ├──SF_2018.csv.gz
   ├──SF_2019.csv.gz
   ├──SF_2020.csv.gz
   ├──SF_2021.csv.gz
├── tableau_data
   ├──precincts.json
   ├──2021_analysis.ipynb
   ├──2021_data.csv 
   ├──ten_years_tableau_data.csv 
├── new_data
   ├──stop_and_frisk_no_race.csv 
   ├──stop_and_frisk_w_race.csv 
├── .DS_Store
├── .gitattributes
├── .gitignore
```
