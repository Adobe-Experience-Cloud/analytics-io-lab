Section 1 – Introduction to Reporting
====

**Go back to [Section 0](../s0_getting_started) | Skip to [Section 2](../s2_breakdown_search)**


Objectives
----
*    Use the metrics and dimensions API methods to request lists of metrics and dimensions
*    Understand the data format for reporting
*    Run your first report against the analytics APIs

Analysis Workspace presents users with lists of dimensions and metrics in the left rail. It uses the /dimensions and /metrics API methods to get those lists.

Exercise 1 - Querying Lists of Dimensions
-----

Programmatically get a list of dimensions available in a report suite by calling the /dimensions API method according to the following steps: 

1.    Make sure that you have followed the steps in [Section 0 - Getting Started] [Accessing the Swagger Interface](../s0_getting_started#accessing-the-swagger-interface) and [Validate API Connectivity](../s0_getting_started#validate-api-connectivity)
2.    Locate and expand the **dimensions** section in the Swagger interface
3.    Click on **`GET /dimension`**
4.    Enter `geo1metrixxprod` in the rsid box
5.    Click on the **Try it out!** button to run the API request

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

There are a lot of available details about a dimension, but for the purpose of this lab we are interested in the **`title`** field which is the friendly name of the dimension and the **`id`** which is the identifier needed to refer to this dimension in a report request.

Information on the other fields in this request can be found in the documentation.
[Dimensions](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_dimensions_resource)

Exercise 2 - Querying Lists of Metrics
-----
Querying lists of metrics is similar to querying lists of dimensions. Request the list of available metrics for the report suite by calling the /metrics API method according to the following steps:

1.    Make sure that you have followed the steps in [Section 0 - Getting Started] [Accessing the Swagger Interface](../s0_getting_started#accessing-the-swagger-interface) and [Validate API Connectivity](../s0_getting_started#validate-api-connectivity)
2.    Locate and expand the **metrics** section of the documentation
3.    Click on **`GET /metrics`**
4.    Enter `geo1metrixxprod` in the rsid box
5.    Click on the **Try it out!** button to run the API request

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

We are interested in the **`title`** field which is the friendly name of the metric and the **`id`** which is the identifier needed to refer to this metric in a report request.

Information on the other fields in this request can be found in our documentation.
[Metrics](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_metrics_resource)

Understanding the report request
-----
Now that you know how to get lists of metrics and dimensions, you are ready to run your first report. In Analysis Workspace, you simply drag dimensions and metrics into the table panel. In order to do this programmatically you need to construct a report request. Review the following request:
 
```javascript
{
  "rsid": "geo1metrixxprod",
  "globalFilters": [
    {
      "dateRange": "2018-03-01T00:00:00.000/2018-03-04T00:00:00.000",
      "type": "dateRange"
    }
  ],
  "metricContainer": {
    "metrics": [
      {
        "columnId": "occurrences",
        "id": "metrics/occurrences"
      }
    ]
  },
  "dimension": "variables/page"
}
```

A report request has several important parts:

### rsid 
The required rsid parameter specifies the report suite ID for the report suite that you want the data to come from.

### globalFilters 
The required globalFilters parameter contains a collection of filters that apply to the entire request. At a minimum a filter specifying the date range for the given report is required. 

### metricContainer
This required parameter defines all the metrics for the given report request and will be represented as the columns in the report response. 

#### metrics
The array holds the metrics for the requested report. Metrics have two required parts to their definition: id and columnId. There is also an optional sort part of a metric definition. 

##### id 
The id of the metric. Use the /metrics or /calculatedmetrics API methods for the list of available options

##### columnId
The name of the column of data that will be returned. You may use the same metric multiple times with different filters in the report response so providing columnIds for the response makes it easier to identify them in the response.

### dimension (optional) 
The optional dimension parameter will cause the report to return data for a particular dimension. The dimension can be any dimension from the /dimensions API method, including the daterange dimensions.

Exercise 3 - Running Your First Report
-----
1.    Make sure that you have followed the steps in [Section 0 - Getting Started] [Accessing the Swagger Interface](../s0_getting_started#accessing-the-swagger-interface) and [Validate API Connectivity](../s0_getting_started#validate-api-connectivity)
2.    Scroll down and expand the reports section 
3.    Click on **`/reports/ranked`** to expand the documentation for that method
4.    Paste the following JSON report request into the body text box
```javascript
{
  "rsid": "geo1metrixxprod",
  "globalFilters": [
    {
      "dateRange": "2018-03-01T00:00:00.000/2018-03-04T00:00:00.000",
      "type": "dateRange"
    }
  ],
  "metricContainer": {
    "metrics": [
      {
        "columnId": "occurrences",
        "id": "metrics/occurrences"
      }
    ]
  },
  "dimension": "variables/page"
}
```
6.   Click on Try it Out!


Take a look at the results.  Do they match this Analysis Workspace report?

![s1_exercise3_results](../../images/s1_exercise3_results.png?raw=true)


Understanding the Report Response
-----
Here is a partial example response:

```javascript
{
    "totalPages": 3,
    "firstPage": true,
    "lastPage": false,
    "numberOfElements": 50,
    "number": 0,
    "totalElements": 149,
    "columns": {
        "dimension": {
            "id": "variables/page",
            "type": "string"
        },
        "columnIds": [
            "0"
        ]
    },
    "rows": [
        {
            "itemId": "2897271828",
            "value": "Search Results",
            "data": [
                4618
            ]
        },
        {
            "itemId": "2080918144",
            "value": "Shopping Cart: Cart Details",
            "data": [
                3853
            ]
        },
        {
            "itemId": "3306266643",
            "value": "Home",
            "data": [
                3612
            ]
        }
		...
		
     ],
    "summaryData": {
        "totals": [
            39700
        ]
    }
}
```

### totalPages
How many pages of data are in this response. 

### firstPage
boolean value set to true if this is the first page

### lastPage
boolean value set to true if this is the last page. 

### numberOfElements
Integer value specifying the number of rows in the current page

### number
Integer value specifying the current page number

### totalElements
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
This array holds the totals over the requested date range for the metrics in this report request. Reference the columnIds array in the columns object to understand what each item in the totals array represents. 

Exercise 4 - Changing the Dimension and Metric
-----
1. Make sure that you have followed the steps in [Section 0 - Getting Started] [Accessing the Swagger Interface](../s0_getting_started#accessing-the-swagger-interface) and [Validate API Connectivity](../s0_getting_started#validate-api-connectivity)
2. Scroll down and expand the reports section 
3. Click on **`/reports/ranked`** to expand the documentation for that method
4. Using the same basic report request from Exercise 3, change the dimension so that you are requesting the Product dimension and the metric so you are requesting the Product Views metric. **You will need to edit the following JavaScript before pasting into the body text box**:

```javascript
{
  "rsid": "geo1metrixxprod",
  "globalFilters": [
    {
      "dateRange": "2018-03-01T00:00:00.000/2018-03-04T00:00:00.000",
      "type": "dateRange"
    }
  ],
  "metricContainer": {
    "metrics": [
      {
        "columnId": "<edit this>",
        "id": "<edit this>"
      }
    ]
  },
  "dimension": "<edit this>"
}
```

**Hint:**  The name "Product Views" is only a display name, the actual metric ID is different. You may need to use the /metrics endpoint from Exercise 2 to look up the actual metric ID for the Product Views metric.

6.   Click on **Try it out!**


Take a look at the results.  Do they match the following Analysis Workspace report?

![s1_exercise4_results](../../images/s1_exercise4_results.png?raw=true)

Exercise 5 - Multiple Metrics in a Single Request and Sorting
-----
1. Make sure that you have followed the steps in [Section 0 - Getting Started] [Accessing the Swagger Interface](../s0_getting_started#accessing-the-swagger-interface) and [Validate API Connectivity](../s0_getting_started#validate-api-connectivity)
2. Scroll down and expand the reports section 
3. Click on **/reports/ranked** to expand the documentation for that method
4. Using the same basic report request from Exercise 4, change the metrics so that you are requesting the Unique Visitors, Product Views, and Cart Additions metrics, sorted descending by Unique Visitors. **You will need to edit the following JavaScript before pasting into the body text box**:

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
                "columnId": "<edit this>",
                "id": "<edit this>",
                "sort": "<edit this>"
            },
            {
                "columnId": "<edit this>",
                "id": "<edit this>"
            },
            {
                "columnId": "2",
                "id": "<edit this>"
            }
        ]
    },
    "dimension": "variables/product"
}
```

**Hint:** A sort on column can be ascending ("asc") or descending ("desc")

6.   Click on **Try it out!**


Take a look at the results.  Do they match the following Analysis Workspace report?

![s1_exercise5_results](../../images/s1_exercise5_results.png?raw=true)


Congratulations! You've completed Section 1 and should understand the basic elements of running a report using the new Analytics V2 API. You can **Continue to [Section 2](../s2_breakdown_search) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. How many unique visitors from the U.S. state of California visited our site on February 28, 2018?
2. How many unique visitors looked at the "Slickrock Trail Bike" product on February 28, 2018 and how many times was it added to a shopping cart that day?


Further Reading (optional)
-----

* [Ranked Reports](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_reports_resource)

* [Dimensions](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_dimensions_resource)

* [Metrics](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_metrics_resource)


**Go back to [Section 0](../s0_getting_started) | Continue to [Section 2](../s2_breakdown_search) »**
