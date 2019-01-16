Section 2 – Breakdowns and Searching
====

**Go back to [Section 1](../s1_api_intro) | Skip to [Section 3](../s3_filtering_segmentation)**

Objectives
----
*    Run a breakdown report
*    Perform a search 

Instructor Demo - Breakdowns
-----
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
                    "0"                               <-- This references the filter by ID from the metricFilters section below
                ]
            }
        ],
        "metricFilters": [
            {
                "id": "0",
                "type": "breakdown",
                "dimension": "variables/evar6",       <-- We are breaking down Product Type
                "itemId": "1664911617"
            }
        ]
    },
    "dimension": "variables/product"                  <-- by Product
}
```

When we breakdown Product Type by Product, we are actually **filtering** Product by a specific value of Product Type.

* A breakdown requires a metricFilters array in the metricContainer object
	* The filter has an integer Id so it can be referenced on specific metrics
	* The filters object is of type breakdown
	* The dimension attribute refers to the dimension Id that we are breaking down
	* The itemId is the specific Id of the dimension value we are breaking down 
* The metrics array now has a filter on the pageviews metric and the Id of that metric matches the Id of the filter defined in the metric filters array.

Section 2, Exercise 1 - Performing a Breakdown
-----

Breakdown the Product Type value of Boots by Product. You will need to make multiple report requests in this exercise. The first request will get the values for Product Type:

1.    Using the `/reports` endpoint like in past exercises, first request the top values for the **Product Type** dimension with the **Product Views** metric.

  * The **Product Type** dimension is stored in **`variables/evar6`**
  * The **Product Views** metric Id is **`metrics/productinstances`**

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
      "itemId": "1664911617",                     <-- We want to break down Boots so we need its specific itemId
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

We want to break down the "Boots" **Product Type** by the **Products** in that category. We need to know the itemId for the value "Boots", which is "1664911617" as shown in the above result. The second request will do the actual breakdown:

2. Using the itemID of **`1664911617`** obtained from the first request, run a second request breaking down the "Boots" item by **Product** with the following report request:

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
Do the results match Analysis Workspace?

![s2_exercise1_results](../../images/s2_exercise1_results.png?raw=true)


Section 2, Exercise 2 - Breakdown Page by Browser 
-----
1.    Using the `/reports` endpoint like in past exercises, first request the list of **Pages** using Analysis Workspace's default metric of **Occurrences**.

**You will need to edit the following JSON request before pasting into the body text box**:
  * The **id** for the **Occurrences** metric is **`metrics/occurrences`**
  * The **dimension** id for the **Pages** dimension is **`variables/page`**

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
                "columnId": "Occurrences",
                "id": "<edit this>",
                "sort": "desc"
            }
        ]
    },
    "dimension": "<edit this>"
}
```

Do your results for **Page** values match Analysis Workspace? 

![s2_exercise2_results1](../../images/s2_exercise2_results1.png?raw=true)

2. Find the `itemId` value for the "Search Results" page in the results from step 1. 
3. Construct a breakdown request to breakdown the "Search Results" page by **Browser**.

 **You will need to edit the following JSON request before pasting into the body text box**:
  * The **id** for the **Occurrences** metric is **`metrics/occurrences`**
  * Make sure the metric's filters include filter id **`0`**
  * The **id** for the metric filter is **`0`**
  * The **type** of the metric filter is **`breakdown`**
  * The **dimension** id of the **Page** dimension is **`variables/page`** 
  * The **itemId** of the "Search Results" **Page** is **`2897271828`**
  * The **dimension** id of the **Browser** dimension is **`variables/browser`**

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
                "columnId": "Occurrences",
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

Instructor Demo - Searching
-----
Analysis Workspace can also do searches on dimension values. 

Understanding a Search Attribute
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
                "id": "metrics/occurrences",
                "sort": "desc"
            }
        ]
    },
    "dimension": "variables/page",
    "search": {                                      <-- You need a search attribute
        "clause": "( CONTAINS 'kids' )",             <-- with a search clause
	"includeSearchTotal" : true
    }
}
```

A search attribute added to the report request will filter the dimensions in the report by the search clause. A search attribute must contain a clause attribute. All other attributes are optional. Search clauses can be very flexible and use the AND, OR, and NOT logical operators. 

*   "clause": "( CONTAINS 'Kids' ) OR ( CONTAINS 'Home' )"
*   "clause": "( CONTAINS 'Kids' ) AND ( NOT CONTAINS 'Home' )"
*   "clause": "( 'Kids' ) AND ( 'Home' )"
*   "clause": "'Kids' OR 'Home'"
*   "clause": "'Kids'"


There are other search criteria besides CONTAINS:
* MATCH
* ENDS-WITH
* BEGINS-WITH


Section 2, Exercise 3 - Peforming a Search
----- 

1.    Using the `/reports` endpoint like in past exercises, perform a search for any **Page** dimension values that contain the string `kids`

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

The response data will look something like this:

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

Section 2, Exercise 4 - Searching with Operators
-----
1.    Using the `/reports` endpoint like in past exercises, perform a search for any **Page** dimensions values that **contain** the string `kids` **OR** the string `home`.

**You will need to edit the following JSON request before pasting into the body text box**:
  * The search **clause** is **`( CONTAINS 'kids' ) OR ( CONTAINS 'home' )`**

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


Section 2, Exercise 5 - Searching with different criteria
-----
1.    Using the `/reports` endpoint like in past exercises, perform a search for any **Page** dimensions that **start with** the value of `Product`

**You will need to edit the following JSON request before pasting into the body text box**:
  * The search **clause** is **( `BEGINS-WITH 'Product' )`**

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

Section 2 Complete
-----
Congratulations! You've completed Section 2 and should understand breakdowns and searching in the new Analytics V2 API. You can **Continue to [Section 3](../s3_filtering_segmentation) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. Try to do a multiple-level deep breakdown by finding out how many **Unique Visitors** viewed the "UltraTech Socks" **Product** while searching with the "outdoor gear" keyword on a mobile device manufactured by "Samsung". Hint - You will need more than one filter in the filters array.
2. What is the total number of unique visitors that looked at "Timberline GTX Boots" while using a mobile device manufacured by a company with the letter 'a' in it's name?

Further Reading (optional)
-----
* [Reports](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/reporting-guide.md)  Includes information on Breakdowns and Searches

**Back to [Section 1](../s1_api_intro) | Continue to [Section 3](../s3_filtering_segmentation) »**
