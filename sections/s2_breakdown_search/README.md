Section 2 – Breakdowns and Searching
====

**Go back to [Section 1](../s1_api_intro) | Skip to [Section 3](../s3_filtering_segmentation)**

Objectives
----
*    Run a breakdown report
*    Perform a search 

A breakdown report filters a dimension based on the specific value of another dimension

Understanding a Breakdown Request
-----

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
                "columnId": "Product Views",
                "id": "metrics/productinstances",
                "filters": [
                    "0"
                ]
            }
        ],
        "metricFilters": [
            {
                "id": "0",
                "type": "breakdown",
                "dimension": "variables/evar6",
                "itemId": "1664911617"
            }
        ]
    },
    "dimension": "variables/product"
}
```
* A breakdown requires a metricFilters array in the metricContainer object
	* The filter has an integer ID so it can be referenced on specific metrics
	* The filters object is of type breakdown
	* The dimension that this breakdown refers to is the variables/evar6 dimension 
	* The itemId is is the specific id for Boots from Product Type report in the previous step
* The metrics array now has a filter on the pageviews metric and the id of that metric matches the id of the filter defined in the metric filters array.

Exercise 1 - Performing a Breakdown
-----

You will need to make multiple report requests in this exercise. The first request will get the values for Product Type:

1.    Using the `/reports/ranked` endpoint and the **Try it out!** button like in past exercises, first request the top values for the **Product Type** dimension with the **Product Views** metric. The **Product Type** dimension is stored in **variables/evar6** in the following report request:
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
                "columnId": "Product Views",
                "id": "metrics/productinstances",
                "sort": "desc"
            }
        ]
    },
    "dimension": "variables/evar6"
}
```
The returned data should match the following:
```javascript
{
  "totalPages": 1,
  "firstPage": true,
  "lastPage": true,
  "numberOfElements": 34,
  "number": 0,
  "totalElements": 34,
  "columns": {
    "dimension": {
      "id": "variables/evar6",
      "type": "string"
    },
    "columnIds": [
      "Product Views"
    ]
  },
  "rows": [
    {
      "itemId": "1664911617",
      "value": "Boots",
      "data": [
        2313
      ]
    },
    {
      "itemId": "2271066348",
      "value": "Jackets",
      "data": [
        1984
      ]
    },
    {
      "itemId": "2306440506",
      "value": "Hats",
      "data": [
        1397
      ]
    },
 
 ...
 
    {
      "itemId": "3642090695",
      "value": "Helmets",
      "data": [
        50
      ]
    }
  ],
  "summaryData": {
    "totals": [
      12609
    ]
  }
}
```

We want to break down the Boots product type by the Products in that category. We need to know the itemId for the value "Boots", which is 1664911617 as shown in the above result. The second request will do the actual breakdown:

2. Using the itemID of `1664911617` obtained from the first request, run a second request breaking down the **Boots** item by **Product** with the following report request:

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
                "columnId": "Product Views",
                "id": "metrics/productinstances",
                "filters": [
                    "0"
                ]
            }
        ],
        "metricFilters": [
            {
                "id": "0",
                "type": "breakdown",
                "dimension": "variables/evar6",
                "itemId": "1664911617"
            }
        ]
    },
    "dimension": "variables/product"
}
```
the results match the Analysis Workspace report:

![s2_exercise1_results](../../images/s2_exercise1_results.png?raw=true)


Exercise 2 - Breakdown Page by Browser 
-----
1.    Using the `/reports/ranked` endpoint and the **Try it out!** button like in past exercises, first request the list of **Pages** using Analysis Workspace's default metric of **Occurrences**. **You will need to edit the following JavaScript before pasting into the body text box**:
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
                "columnId": "Ocurrences",
                "id": "<edit this>",
                "sort": "desc"
            }
        ]
    },
    "dimension": "<edit this>"
}
```

Do your results for Page values match Analysis Workspace? 


![s2_exercise2_results1](../../images/s2_exercise2_results1.png?raw=true)

2. Find the `itemId` value for the "Search Results" page in the results from step 1. 
3. Construct a breakdown request to breakdown the "Search Results" page by **Browser**. **You will need to edit the following JavaScript before pasting into the body
text box**:
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
                "columnId": "Ocurrences",
                "id": "<edit this>",
                "filters": [
                    "<edit this>"
                ]
            }
        ],
        "metricFilters": [
            {
                "id": "<edit this>",
                "type": "<edit this>",
                "dimension": "<edit this>",
                "itemId": "<edit this>"
            }
        ]
    },
    "dimension": "<edit this>"
}
```

Do your results for the Search Results page broken down by Browser match Analysis Workspace?

![s2_exercise2_results2](../../images/s2_exercise2_results2.png?raw=true) 

Searching
-----
Analysis Workspace can also do searches on dimension values. 

Understanding a Search atribute
-----
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
                "columnId": "0",
                "id": "metrics/occurrences",
                "sort": "desc"
            }
        ]
    },
    "dimension": "variables/page",
    "search": {
        "clause": "( CONTAINS 'kids' )",
	"includeSearchTotal" : true
    }
}
```

A search added to the report request will filter the dimensions in the report by the search clause. A search attribute must contain a clause attribute. All other attributes are optional. Search clauses can be very flexible and use the AND, OR, and NOT logical operators. 

*   "clause": "( CONTAINS 'Kids' ) OR ( CONTAINS 'Home' )"
*   "clause": "( CONTAINS 'Kids' ) AND ( NOT CONTAINS 'Home' )"
*   "clause": "( 'Kids' ) AND ( 'Home' )"
*   "clause": "'Kids' OR 'Home'"
*   "clause": "'Kids'"


There are other search criteria besides CONTAINS:
* MATCH
* ENDS-WITH
* BEGINS-WITH


Exercise 3 - Peforming a Search
----- 

1.    Using the `/reports/ranked` endpoint and the **Try it out!** button like in past exercises, perform a search for any **Page** dimension to find results that contain the string `kids`
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
                "columnId": "0",
                "id": "metrics/occurrences",
                "sort": "desc"
            }
        ]
    },
    "dimension": "variables/page",
    "search": {
        "clause": "( CONTAINS 'kids' )",
	"includeSearchTotal" : true
    }
}
```

The response data will look something like this 
```javascript
{
  "totalPages": 1,
  "firstPage": true,
  "lastPage": true,
  "numberOfElements": 1,
  "number": 0,
  "totalElements": 1,
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
      "itemId": "3857221589",
      "value": "Kids",
      "data": [
        1316
      ]
    }
  ],
  "summaryData": {
    "totals": [
      39700
    ],
    "searchTotals": [
      1316
    ]
  }
}
```

Exercise 4 - Searching with Operators
-----
1.    Using the `/reports/ranked` endpoint and the **Try it out!** button like in past exercises, perform a search for any **Page** dimensions values that **contain** the string `kids` **OR** the string `home`. **You will need to edit the following JavaScript before pasting into the body text box**:
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
                "columnId": "0",
                "id": "metrics/occurrences",
                "sort": "desc"
            }
        ]
    },
    "dimension": "variables/page",
    "search": {
        "clause": "<edit this>",
	"includeSearchTotal" : true
    }
}
```

Do your results match Analysis Workspace?

![s2_exercise4_results](../../images/s2_exercise4_results.png?raw=true) 


Exercise 5 - Searching with different criteria
-----
1.    Using the `/reports/ranked` endpoint and the **Try it out!** button like in past exercises, perform a search for any **Page** dimensions that **start with** the value of `Product` **You will need to edit the following JavaScript before pasting into the body text box**:
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
                "columnId": "0",
                "id": "metrics/occurrences",
                "sort": "desc"
            }
        ]
    },
    "dimension": "variables/page",
    "search": {
        "clause": "<edit this>",
	"includeSearchTotal" : true
    }
}
```

Do your results match Analysis Workspace?

![s2_exercise5_results](../../images/s2_exercise5_results.png?raw=true) 

Congratulations! You've completed Section 2 and should understand more about breakdowns and searching in the new Analytics V2 API. You can **Continue to [Section 3](../s3_filtering_segmentation) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. Try to do a multiple level deep breakdown by finding out how many unique visitors viewed the "UltraTech Socks" product while searching with the "outdoor gear" keyword on a mobile device manufactured by "Samsung". Hint - You will need more than one filter in the filters array.
2. What is the total number of unique visitors that looked at "Timberline GTX Boots" while using a mobile device manufacured by a company with the letter 'a' in it's name?


**Back to [Section 1](..s1_api_intro) | Continue to [Section 3](../s3_filtering_segmentation) »**
