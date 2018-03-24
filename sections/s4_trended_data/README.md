Section 4 – Trended Data
====

**Go Back to [Section 3](../s3_filtering_segmentation) | Skip to [Section 5](../s5_tips_tricks) »**

Objectives
----
* Learn the two different ways to run a trended report
* Understand the data format for requesting trended data   

Instructor Demo - Running a report with a time granularity as dimension
-----
You can use a time granularity as a dimension in Analysis Workspace report to get a trended report

Understanding a Trended Report Request
-----
```javacript
{
    "rsid": "geo1metrixxprod",
    "globalFilters": [
        {
            "type": "dateRange",
            "dateRange": "2018-03-01T00:00:00.000/2018-03-04T00:00:00.000"
        }
    ],
    "metricContainer": {
        "metrics": [
            {
                "id": "metrics/occurrences"
            }
        ]
    },
    "dimension": "variables/daterangehour",    <-- The dimension is a time granularity

    "settings": {                              <-- We need to add a "settings" section
        "dimensionSort": "asc",	               <-- to specify sorting by dimension value instead of sorting by metric value
    }	
}
```

Valid time granularities are: 
```
Minute:  variables/daterangeminute
Hour:    variables/daterangehour
Day:     variables/daterangeday
Week:    variables/daterangeweek
Month:   variables/daterangemonth
Quarter: variables/daterangequarter
Year:    variables/daterangeyear
```
### settings
The ranked settings provide several parameters that control how the data is returned in the response.

#### limit (optional)
Sets an upper limit on the number of data rows to return. The default is 50.

#### page (optional)
The page number when you want to chunk up the requests. Example: If the date range we are reporting against has 90 days worth of data setting the page attribute to 0 with a limit of 50 would return the first 50 days. Setting page to 1 would return the remaining 40 days.

#### dimensionSort (optional)
The sort direction of the data ("asc" for ascending, "desc" for descending). Including this attribute will sort the data by the dimension values instead of by a metric. 

Section 4, Exercise 1 - Trend a metric 
-----
Trend the **Occurrences** metric by hour.

1. Using Swagger and the **`POST /reports/ranked`** endpoint as in past exercises, paste the following JSON report request into the body text box:

```javascript
{
    "rsid": "geo1metrixxprod",
    "globalFilters": [
        {
            "type": "dateRange",
            "dateRange": "2018-03-01T00:00:00.000/2018-03-04T00:00:00.000"
        }
    ],
    "metricContainer": {
        "metrics": [
            {
                "id": "metrics/occurrences"
            }
        ]
    },
    "dimension": "variables/daterangehour",
    "settings": {
        "dimensionSort": "asc"
    }	
}
```

2. Click the **Try it out!** button to submit the request
3. Do your results match Analysis Workspace?

![s4_exercise1_results](../../images/s4_exercise1_results.png?raw=true)

Section 4 Complete
-----
Congratulations! You've completed Section 4 and should understand how to request trended reports. You can **Continue to [Section 5](../s5_tips_tricks) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.


Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. What day of the month recieved the most traffic from the apple iphone?
2. Which product is most popular during the hour of the day that gets the most traffic?

**Go Back to [Section 3](../s3_filtering_segmentation) | Continue to [Section 5](../s5_tips_tricks) »**
