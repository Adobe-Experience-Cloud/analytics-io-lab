Section 4 – Trended Data
====

Objectives
----
* Run a basic overtime report
* Run a report with trended data
* Understand the data format for requesting trended data   

Running a basic overtime report
-----
1. Make sure that you have followed the steps above to [Access the Swagger UI](../s1_api_intro#accessing-the-swagger-interface)
2. Expand the reports section
3. Expand the `POST /reports/ranked` API endpoint
4. Paste the following json report request into the body text box:
```javascript
{
  "rsid": "geo1metrixxprod",
  "dimension": "variables/daterangeday",
  "globalFilters": [
    {
      "type": "dateRange",
      "dateRange": "2018-01-01T00:00/2018-02-28T00:00"
    }
  ],
  "settings": {
    "limit": 50,
    "page": 0,
    "dimensionSort": "asc"
  },
  "metricContainer": {
    "metrics": [
      {
        "id": "metrics/pageviews"
      },
      {
        "id": "metrics/visits"
      },
      {
        "id": "metrics/visitors"
      }
    ]
  }
}
```
5. Click the "Try it out!" button to submit the request
6. Take a look at the results.

Understanding the report request
-----
The report request for requesting trended data is very similar to the ranked requests covered in the earlier sections of the lab. It uses the same attributes covered in section 1 of this lab but uses a different reporting dimension: `variables/daterangeday`

### Overtime Granularity
In order to change the granularity of the report request you just need to use the appropriate reporting dimension. Below is a list of the available overtime reporting dimensions:
```variables/daterangehour
variables/daterangeday
variables/daterangeweek
variables/daterangemonth
variables/daterangequarter
variables/daterangeyear
```

### settings
The ranked settings provide several parameters that control how the data is returned in the response.

#### limit
Sets an upper limit on the number of data rows to return. The default is 50.

#### page
The page number when you want to chunk up the requests. Example: If the date range we are reporting against has 90 days worth of data setting the page attribute to 0 with a limit of 50 would return the first 50 days. Setting page to 1 would return the remaining 40 days.

#### dimensionSort
The sort direction of the data ("asc" for ascending, "desc" for descending). Including this attribute will sort the data by the dimension values instead of by a metric. 

Understanding the report response
-----
The response has the same format as other ranked report request which we covered in previous sections. The main difference you will notice is that you get dates back as the row values when using the `variables/daterangeday` dimension in the report request.

Running a trended report
-----
1. Expand the reports section
2. Expand the `POST /reports/ranked` API endpoint
3. Paste the following json report request into the body text box:
```javascript
{
     "rsid":"geo1metrixxprod",
     "dimension":"variables/daterangeday",
     "globalFilters":[
        {
           "type":"dateRange",
           "dateRange":"2018-01-01T00:00/2018-02-28T00:00"
        }
     ],
     "settings":{
         "dimensionSort":"asc"
     },
     "metricContainer":{
        "metricFilters":[
           {
              "id":"filter0",
              "type":"breakdown",
              "dimension":"variables/page",
              "itemId":2390504924
           },
           {
              "id":"filter1",
              "type":"breakdown",
              "dimension":"variables/page",
              "itemId":3786354655
           },
           {
              "id":"filter2",
              "type":"breakdown",
              "dimension":"variables/page",
              "itemId":2933795879
           }
        ],
        "metrics":[
           {
              "id":"metrics/pageviews",
              "filters":[
                 "filter0"
              ]
           },
           {
              "id":"metrics/pageviews",
              "filters":[
                 "filter1"
              ]
           },
           {
              "id":"metrics/pageviews",
              "filters":[
                 "filter2"
              ]
           }
        ]
     }
}
```
4. Click the "Try it out!" button to submit the request
5. Take a look at the results.

Understanding the report request
-----
Notice that the report request here looks a lot like the requests from section 2 where we covered breakdowns. In our example each metric filter is a specific page from our data set for which we would like to request trended data.

Understanding the report response
-----
Notice that we have 3 columns of data in our response. There is a column for each of the specific pages from our request.


Congratulations! You've completed Section 4 and should understand how to request trended reports. You can **Continue to [Section 5](../s5_tips_tricks) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.


Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. What day of the month recieved the most traffic from the apple iphone?
2. Which product is most popular during the hour of the day that gets the most traffic?

**Continue to [Section 5](../s5_tips_tricks) »**
