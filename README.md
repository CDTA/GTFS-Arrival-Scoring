# Overview 
 
The overarching idea is to confirm how accurate our real time arrival predictions are from the INIT system. We have reports from customers of inaccuracies and on-street observations that CDTA has performed but not systemic way to check the accuracy. This will hopefully also allow us to dig into recurring issues that may prevent arrivals from being correct. The long-term goal is to build upon this data set and add more data to allow for planning to use as another tool. 
 
The overall process involves Ingesting the real time arrival predications and location information for all routes and buses which is generated every 30 seconds from the INIT system. Import the actual arrival information from the INIT system (takes 3 days to fully update). Import the static GTFS route and stop information. Clean all sets of data. Score the real time arrival prediction based on the actual arrival information and the scheduled arrival in two ways. First is basic simple prediction scoring &gt; +/- 2 minutes is Bad less than +/- 30 seconds off is Great. The second is a sliding scale based on how far the prediction is generated from the scheduled arrivals. So, the real time prediction is greater than 10 minutes away from the scheduled arrival a Great score would be less than +/-2 minutes off from actual arrival. 
 
Once the predictions of scored, there would be further data cleaning to remove anomalous predictions or find patterns of anomalous predictions. 
 
All this data would then be available via a Power BI Report. 
 
## Overall Results 
 
The first results are not terrible in terms of accuracy if you use the sliding score that we created which works like the table below only 25% of the predictions, we have 35.5 million already collected, are considered Bad. So, 75% are Ok or better with 48% of those 35.5 million being Great. 

| **SCALE** | **GREAT** | **GOOD** | **OK** | **BAD** |
| --- | --- | --- | --- | --- |
| &gt;10min | &lt;2min | 2-3min | 3-4min | &gt;4min |
| &lt;10m&gt;5min | &lt;1.5min | 1.5-2min | 2-3min | &gt;3min |
| &lt;5m&gt;2min | &lt;1min | 1-1.5min | 1.5-2min | &gt;2min |
| &lt;2min | &lt;30sec | 30s-45s | 45s-60s | &gt;1min |

power-bi-report.png

## Potential Data Issues 

There are still three data issues we are running down as we dig into this more: 
 
1. Old Predictions, sometime predictions do not get updated promptly and they should be thrown out. I am working on a way to do that on Import and in the existing data.
2. Anomalous predictions, digging deeper into the data sometimes the predicted arrival time can be like over 30 minutes off from the actual arrival time and it affects that prediction and all predictions after it for that Trip. It may be related to #1 above but it doesn’t seem like it based on the timestamps so I may “categorize” these differently.
3. Questions about when a prediction changes from the scheduled time to an actual prediction. It seems like any predictions over 10 minutes are BAD and this looks like because the INIT system publishes the scheduled arrival time until the bus reaches a certain point (distance, time or #stops away) then starts generating a prediction. This reflects almost 17% of the overall data and 19% of the BAD scores.

Either way a lot more digging to go through in terms of data and some cleanup but End to End this really shows it is possible to build something relatively easily in the cloud.  

![power-bi-report](https://github.com/user-attachments/assets/59a18812-6aad-4140-9808-b86aaf03c97b)

# Technical Overview 
 
## Architecture 
 
The overall architecture looks like this. 
 
![technical-overview](https://github.com/user-attachments/assets/97d618ff-fe71-4192-bcdc-10b4c4b2f5c8)


### How did we do this? 

- Microsoft Fast Track resources to help us understand how to setup and use Data Bricks, Azure Synapse, Azure Data Lake, Power BI, Security, Overall Architecture
- Microsoft Fast Track wrote the first example code for us to understand how to use Data Bricks for the processing of the feeds.
- Took about 8 weeks total and about 50-60 hours of actual time, there were many iterations of ingestion as we cleaned up the data coming in and found issues, this can take a few days because the actual arrival data needs to be at 3 days old to consider it fully updated from INIT
- We took in depth training on CDTA Synapse and Azure Data Lakes

### Next Steps 

- More in depth Training on Power BI and Data Bricks scheduled for December, January
- Look into the data issues listed above.
- Generate a list of reasons for BAD predictions and follow up with INIT.
- Talk with a group from MTA who is doing something similar.
- More in depth Training on Power BI and Data Bricks scheduled for December, January
- Look into the data issues listed above.
- Generate a list of reasons for BAD predictions and follow up with INIT.
- Talk with a group from MTA who is doing something similar.
