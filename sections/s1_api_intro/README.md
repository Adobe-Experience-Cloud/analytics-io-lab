Section 1 – Introduction to the new reporting APIs
====

Objectives
----
*    Run your first report against the analytics APIs
*    Understand the data format for reporting
*    Use the metrics and dimensions API methods to request additional data

Running Your First Report
-----
1.    Make sure that you have followed the steps in [Section 0 - Getting Started] [Accessing the Swagger Interface](../s0_getting_started#accessing-the-swagger-interface) and [Validate API Connectivity](../s0_getting_started#validate-api-connectivity)
2.    Scroll down and expand the reports section 
3.    Click on **/reports/ranked** to expand the documentation for that method
4.    Paste the following json report request into the body text box
```javascript
{
  "rsid": "geo1metrixxprod",
  "dimension": "variables/page",
  "globalFilters": [
    {
      "dateRange": "2018-03-03T00:00:00.000/2018-03-04T00:00:00.000",
      "type": "dateRange"
    }
  ],
  "metricContainer": {
    "metrics": [
      {
        "columnId": "pageviews",
        "id": "metrics/pageviews"
      },
      {
        "columnId": "visits",
        "id": "metrics/visits"
      }
    ]
  }
}
```
6.   Click on submit  


Take a look at the results. 


Understanding the report request
-----
The report request you just made is requesting the pages in the geo1metrixxprod report suite ID with the pageviews and visits metrics for the full day of March 3, 2018.

Below is a more detailed explanation of each of the portions of this report request. The sections of the report request are not required to be in any particular order.

### rsid 
The required rsid parameter specifies the report suite ID for the report suite that you want the data to come from. This id can be found in the Report Suite Management section of the Analytics Admin Console or through the /collections/reportsuites API call. Make sure not to confuse the Report Suite ID with the Report Suite Name, also referred to as the "friendly name" or "display name".

### dimension (optional) 
The optional dimension parameter will cause the report to return data for a particular dimension. The dimension can be any dimension from the /dimensions API method, including the daterange dimensions. If this parameter is not included then you will get a response with just the totals for the given metrics. Although multiple metrics can be specified in a single report request, only a single dimension is allowed. If you want additional dimensions you will need to make additional requests.

### globalFilters 
The required globalFilters parameter contains a collection of filters that apply to the entire request. At a minimum a filter specifying the date range for the given report is required. 

### metricContainer
This parameter defines all the metrics for the given report request and will be represented as the columns in the report response.

#### metrics
The collection holds the metrics for the requested report. Metrics have two required parts to their definition: id and columnId. There is also an optional sort part of a metric definition. 

##### id 
The id of the metric. Use the /metrics or /calculatedmetrics API methods for the list of available options

##### columnId
The name of the column of data that will be returned. You may use the same metric multiple times with different filters in the report response so providing columnIds for the response makes it easier to identify them in the response.


Understanding the report response
-----
Now let's look at the report response and make sure that we understand what this data means.

Here is a partial example response:

```javascript
{
  "totalPages": 3,
  "firstPage": true,
  "lastPage": false,
  "numberOfElements": 50,
  "number": 0,
  "totalElements": 142,
  "columns": {
    "dimension": {
      "id": "variables/page",
      "type": "string"
    },
    "columnIds": [
      "pageviews",
      "visits"
    ]
  },
  "rows": [
    {
      "itemId": "2897271828",
      "value": "Search Results",
      "data": [
        1892,
        942
      ]
    },
    {
      "itemId": "2080918144",
      "value": "Shopping Cart: Cart Details",
      "data": [
        1572,
        874
      ]
    }
  ], 
  ...
  "summaryData": {
    "totals": [
      16338,
      5659
    ]
  }
}
```

### totalPages
How many pages of data are in this response. 

### firstPage
boolean value set to true if this is the first page

### lastPage
boolean value set to true if this is the last page. Note that a page can be both the first page and the last page if there is only a single page of data.

### numberOfElements
Integer value specifying the number of elements in the current page

### number
Integer value specifying the current page number

### total Elements
This is the total number of rows in this response

### columns
This object holds the description of the columns in the response.

#### dimension
This describes the dimension of the report response which will be represented as the itemId and value in the rows section of the response. The dimension has an id and a type.

#### columnIds
This is an array of the columns or metrics represented in the report. The values in this array match the columnIds passed in on the request.

### rows
This is an array of rows for the report response.
Each row object has the following attributes

#### itemId
This is the id for this particular value of the dimension. This s the id value that you will need if you are going to perform breakdowns or reference this dimension value in future report requests.

#### value
This is the value of the dimension and will be the value that was passed into this dimension during data collection.

#### data
This is an array of data for the metrics that were requested in this report request. Reference the columnIds array in the columns object to understand what this data represents.

### SummaryData
This is an array of aggregated or calculated data for this report.

#### totals
This array holds the totals over the requested date range for the metrics in this report request. Reference the columnIds array in the columns object to understand what each item in the totals array represents. If the report request contained a dimension then this is the count of every time the metric was incremented when the dimension was sent in.  If the report did not include a dimension then this is a count of every time the metric was incremented.


Changing the Dimension of the Report
-----
Now that you know how to run a basic report, let's run some reports on different dimensions. You can programmatically get a list of dimensions available in a report suite by calling the /dimensions API method according to the following steps: 

1.    Locate and expand the **dimensions** section of the documentation
2.    Click on **GET /dimension**
3.    Enter 'geo1metrixxprod' in the rsid box
4.    Click on the **Try it out!** button to run the API request

This method returns a list of available dimensions for the report suite. The response will look something like the following.
```javascript
[
  {
    "id": "variables/geocountry",
    "title": "Countries",
    "name": "Countries",
    "type": "enum",
    "category": "Audience",
    "support": [
      "oberon",
      "dataWarehouse"
    ],
    "pathable": false,
    "segmentable": true,
    "reportable": [
      "oberon"
    ],
    ...
  }
]
```

There are a lot of available details about a dimension, but for the purpose of this lab we are interested in the **`title`** field which is the friendly name of the dimension and the **`id`** which is the identifier needed to refer to this dimension.
Information on the other fields in this request can be found in our documentation.
[Dimensions](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_dimensions_resource)

Now we are going to take our original report request and use a different dimension.

Repeat the steps that you followed in running your first report, but this time use a different dimension. It can be any dimension returned by the **GET /dimensions** API method. (e.g. variables/geocountry). [Running Your First Report](sections/s1_api_intro#running-your-first-report)

The report data should reflect your new dimension instead of the Page variable of the original report.

Changing the Metrics on the Report
-----
Now let's query some different metrics. Changing the metric is similar to changing the dimension. You request the list of available metrics for the report suite by calling the /metrics API method according to the following steps:

1.    Locate and expand the **metrics** section of the documentation
2.    Click on **GET /metrics**
3.    Enter 'geo1metrixxprod' in the rsid box
4.    Click on the **Try it out!** button to run the API request

This method returns a list of available metrics for the report suite.
The response will look something like the following.
```javascript
[
  {
    "id": "metrics/orders",
    "title": "Orders",
    "name": "Orders",
    "type": "int",
    "category": "Conversion",
    "support": [
      "oberon",
      "dataWarehouse"
    ],
    "allocation": true,
    "precision": 0,
    "calculated": false,
    "segmentable": true,
    "polarity": "positive"
  },
  ...
]
```

For our current purposes we are interested in the `title` field which is the friendly name of the metric and the `id` which is the identifier needed to refer to this metric.
Information on the other fields in this request can be found in our documentation.
TODO: Include link to the documentation here
[Metrics](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_metrics_resource)

Now we are going to take our original report request and use some different metrics.

Repeat the steps that you followed in running your first report, but this time include some metrics. It can be any metric or combination of metrics returned by the **GET /metrics** API method. (e.g. metrics/orders). [Running Your First Report](sections/s1_api_intro#running-your-first-report)

The report data should reflect your new metrics instead of the visits metric of the original report.

Congratulations! You've completed Section 1 and should understand the basic elements of running a report using the new Analytics V2 API. You can **Continue to [Section 2](../s2_breakdown_search) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. How many unique visitors from the U.S. state of California visited our site on February 28, 2018?
2. How many unique visitors looked at the "Slickrock Trail Bike" product on February 28, 2018 and how many times was it added to a shopping cart that day?


Further Reading (optional)
-----

[Ranked Reports](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_reports_resource)
[Dimensions](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_dimensions_resource)
[Metrics](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_metrics_resource)

**Continue to [Section 2](../s2_breakdown_search) »**
