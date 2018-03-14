Section 2 – Breakdowns and Searching
====

Objectives
----
*    Run a breakdown report
*    Perform a search 

Performing a Breakdown
-----
A breakdown report is where you filter a dimension by a specific value of another dimension. The data will show you all values of dimension 2 where the value of dimension 1 was also present in the same data collection request (not counting evar persistence).

The first thing that we are going to do is view the values of dimension 2.
1.    Make sure that you have followed the steps above to [Access the Swagger UI](../s1_api_intro#accessing-the-swagger-interface)
2.    Scroll down and expand the reports section of the documentation
3.    Click on the `/reports/ranked` text to expand the documentation for that endpoint
4.    Paste the following json report request into the body text box
```javascript
{
  "rsid": "geo1metrixxprod",
  "dimension": "variables/browser",
  "globalFilters": [
    {
      "dateRange": "2014-06-01T00:00/2014-06-21T00:00",
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
6.   Click on the submit  


The data should look something like the following:
```javascript
{
  "totalPages": 4,
  "firstPage": true,
  "lastPage": false,
  "numberOfElements": 50,
  "number": 0,
  "totalElements": 157,
  "columns": {
    "dimension": {
      "id": "variables/browser",
      "type": "string"
    },
    "columnIds": [
      "pageviews",
      "visits"
    ]
  },
  "rows": [
    {
      "itemId": "497",
      "value": "Microsoft Internet Explorer 8",
      "data": [
        57548,
        5647
      ]
    },
    {
      "itemId": "594",
      "value": "Microsoft Internet Explorer 10",
      "data": [
        49741,
        4922
      ]
    },
    {
      "itemId": "634",
      "value": "Google Chrome 29.0",
      "data": [
        45973,
        4585
      ]
    }
  ],
  "summaryData": {
    "totals": [
      343852,
      34035
    ]
  }
}
```

Now we are going to run the second report with a breakdown filter of a specific browser.
```javascript
{
  "rsid": "geo1metrixxprod",
  "dimension": "variables/page",
  "globalFilters": [
    {
      "dateRange": "2014-06-01T00:00/2014-06-21T00:00",
      "type": "dateRange"
    }
  ],
  "metricContainer": {
    "metricFilters": [
      {
        "dimension": "variables/browser",
        "id": "breakdown1",
        "itemId": 634,
        "type": "breakdown"
      }
    ],
    "metrics": [
      {
        "columnId": "pageviews",
        "filters": [
          "breakdown1"
        ],
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

There are a few new things that you should pay attention to on the above request. 
* We now have a metricFilters array in the metricContainer object
	* The filters object is of type breakdown (There are other types as well which we will show in a future example)
	* The dimension that this breakdown refers to is the browser dimension (you can see all the dimensions in the /dimensions endpoint that we had shown in last section)
	* The itemId is a specific id from the browsers report that we had run earlier
* The metrics array now has a filter on the pageviews metric and the id of that metric matches the id defined in the metric filters object.

When you run this request you will see that the page views metric is much smaller since it is only representing traffic from a specific version of the Google Chrome browser.

Here is a second example that shows some of the flexibility of metricFilters:
```javascript
{
  "rsid": "geo1metrixxprod",
  "dimension": "variables/page",
  "globalFilters": [
    {
      "dateRange": "2014-06-01T00:00/2014-06-21T00:00",
      "type": "dateRange"
    }
  ],
  "metricContainer": {
    "metricFilters": [
      {
        "dateRange": "2014-06-01T00:00/2014-06-15T00:00",
        "id": "dateRange1",
        "type": "dateRange"
      },
      {
        "dimension": "variables/pages",
        "id": "breakdown1",
        "itemId": 12345678,
        "type": "breakdown"
      }
    ],
    "metrics": [
      {
        "columnId": "pageviews",
        "filters": [
          "breakdown1"
        ],
        "id": "metrics/pageviews"
      },
      {
        "columnId": "visits",
        "filters": [
          "breakdown1",
          "dateRange1"
        ],
        "id": "metrics/visits"
      }
    ]
  }
}
```

There are a few new things in the above request:
* We have a metric filter of type dateRange that has a date range that is smaller than the global filter
* We have applied the breakdown1 filter to both metrics
* We have applied the daterange1 filter to only one metric

Performing a Search
-----
In the above examples we introduced metric filters and showed some of their uses. Now we are going to cover methods of filtering a request by the values of a dimension.

The following API request could be used if you wanted to see all pages that related to your shopping cart.
```javascript
{
  "dimension": "variables/page",
  "globalFilters": [
    {
      "dateRange": "2014-06-01T00:00/2014-06-21T00:00",
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
  },
  "rsid": "geo1metrixxprod",
  "search": {
    "clause": "( 'Cart' )",
    "includeSearchTotal": true
  }
}
```

The response data will look something like this 
```javascript
{
  "totalPages": 1,
  "firstPage": true,
  "lastPage": true,
  "numberOfElements": 5,
  "number": 0,
  "totalElements": 5,
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
      "itemId": "2390504924",
      "value": "Shopping Cart|Cart Details",
      "data": [
        40842,
        19791
      ]
    },
    {
      "itemId": "3786354655",
      "value": "Shopping Cart|Shipping Information",
      "data": [
        20095,
        13972
      ]
    },
    {
      "itemId": "4086749071",
      "value": "Shopping Cart|Billing Information",
      "data": [
        14615,
        11433
      ]
    },
    {
      "itemId": "3961702603",
      "value": "Shopping Cart|Order Review",
      "data": [
        8736,
        7877
      ]
    },
    {
      "itemId": "2105155662",
      "value": "Shopping Cart|Order Confirmation",
      "data": [
        6012,
        5658
      ]
    }
  ],
  "summaryData": {
    "totals": [
      343852,
      34035
    ],
    "searchTotals": [
      90300,
      19793
    ]
  }
}
```

Since we asked for searchTotals in our request you will see that extra value in the summary field. You can add the values related to each metric from each response and it will equal the value of the search total. The standard totals include the counts for all the pages. 

When searching some of the settings options can be particularly handy.
```javascript
{
  "dimension": "variables/page",
  "globalFilters": [
    {
      "dateRange": "2014-06-01T00:00/2014-06-21T00:00",
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
  },
  "rsid": "geo1metrixxprod",
  "settings": {
    "dimensionSort": "asc",
    "limit": 50,
    "page": 0
  },
  "search": {
    "clause": "( 'Cart' )",
    "includeSearchTotal": true
  }
}
```

Setting the sort order and the number of rows for each request can make it much easier to find the specific data you are looking for and can also make processing the data in a script more efficient.

Congratulations! You've completed Section 2 and should understand more about breakdowns and searching in the new Analytics V2 API. You can **Continue to [Section 3](../s3_filtering_segmentation) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. Try to do a multiple level deep breakdown by finding out how many unique visitors looked the "UltraTech Socks" product while searching with the "outdoor gear" keyword on a mobile device manufactured by "Samsung". Hint - You will need more than one filter in the filters array.
2. What is the total number of unique visitors that looked at "Timberline GTX Boots" while using a mobile device manufacured by a company with the letter 'a' in it's name?


**Continue to [Section 3](../s3_filtering_segmentation) »**
