# Explore the New Adobe Analytics 2.0 APIs

## Table of Contents

* [Lab Overview](#lab-overview)
* [Lesson 1 - Getting Started](#lesson-1---getting-started)
* [Lesson 2 - Introduction to Reporting](#lesson-2---introduction-to-reporting)
* [Lesson 3 - Performing Breakdowns and Searching](#lesson-3---performing-breakdowns-and-searching)
* [Lesson 4 - Filtering Data and Segmentation](#lesson-4---filtering-data-and-segmentation)
* [Lesson 5 - Trended Data](#lesson-5---trended-data)
* [Lesson 6 - Tips and Tricks](#lesson-6---tips-and-tricks)
* [Appendix](#appendix)

## Lab Overview
Adobe Analytics APIs offer limitless ways to integrate your most important customer data into key business processes. The next-generation data query API is faster, more flexible, and more RESTful. In fact, it is the same API used by Adobe engineers to power the popular Analysis Workspace interface.

Learn how to apply the new Analytics API to your business needs by:
* Querying the same metrics and dimensions exposed in Analysis Workspace
* Diving deep through data with unlimited levels of breakdowns
* Applying search and segment filters and date ranges

### Key Takeaways
* Understand the newly released Analytics Reporting API (v2)
* Learn which authentication mechanisms to use
* Make API calls for the same data Analysis Workspace displays

### Prerequisites
* Adobe Analytics

## Lesson 1 - Getting Started

### Objective

1. Log in and get to know Analysis Workspace
2. Learn how to authenticate API calls in the lab

### Experience Cloud Accounts
The new Analytics APIs require users and organizations to be configured for login via the Experience Cloud at https://experiencecloud.adobe.com, rather than the traditional Analytics login page at my.omniture.com.

In this lab you will be using an Experience Cloud organization and Analytics login company that is already configured for Experience Cloud Authentication.

#### Exercise 1.1 Accessing Analysis Workspace
**Let's sign in!**

In a new browser tab or window, go to https://experiencecloud.adobe.com and select **Sign In with an Adobe ID**
![s0_exp_cloud_login](images/s0_exp_cloud_login.png?raw=true)

The username will be of the following format:

`apilab18+<User ID>@adobetest.com`

where the `<User ID >` section is replaced by your lab computer number.  For example, apilab18+37@adobetest.com

Enter your assigned username and the password provided to you and click **Sign In**
![s0_exp_cloud_login2](images/s0_exp_cloud_login2.png?raw=true)

You should be taken directly to the Analysis Workspace landing page:
![s1_create_new_project](images/s1_create_new_project.png?raw=true)

You are now ready to work in Analysis Workspace!

#### Exercise 1.2 Accessing the Swagger Interface
We will be using a Swagger interface to generate API requests in this lab. To access Swagger, in a new tab or window in your web browser navigate to [Analytics Swagger](https://adobedocs.github.io/analytics-2.0-apis/)

To make an API request in Swagger, you will need to make sure you are authenticated to provide an access token on each request. To authenticate through swagger click on the login button at the top of the page.
![s0_swagger_ui](images/s0_swagger_ui.png?raw=true)

Go to https://experiencecloud.adobe.com and select **Sign In with an Adobe ID**
![s0_exp_cloud_login](images/s0_exp_cloud_login.png?raw=true)

The username will be of the following format:

`apilab18+<User ID>@adobetest.com`

where the `<User ID >` section is replaced by your lab computer number.  For example, apilab18+37@adobetest.com

Enter your assigned username and the password provided to you and click **Sign In**
![s0_exp_cloud_login2](images/s0_exp_cloud_login2.png?raw=true)

#### Exercise 1.2 Validate API Connectivity
Let's make sure everything works correctly. Click on the **Users** section in the Swagger interface and then select **GET /users/me**, then click **Try it out!**:

![s0_swagger_users_me](images/s0_swagger_users_me_1.png?raw=true)

Then click **Execute**

![s0_swagger_users_me2](images/s0_swagger_users_me_2.png?raw=true)

If you have authenticated correctly you should get a JSON response from the /users/me endpoint with information about your current user:

/users/me response
```javascript
{
  "companyid": 300007301,
  "loginId": 709153,
  "login": "apilab18+37@adobetest.com",
  "createDate": "2018-01-22T12:39:16",
  "disabled": false,
  "email": "apilab18+37@adobetest.com",
  "firstName": "API Lab 37",
  "fullName": "API Lab 37 API Lab 37",
  "imsUserId": "EBCA2A9C5A664BAA0A495C94@AdobeID",
  "lastName": "API Lab 37",
  "lastLogin": "1970-01-01T00:00:00Z",
  "lastAccess": "2019-02-04T21:38:41Z",
  "phoneNumber": null,
  "tempLoginEnd": null,
  "title": null
}
```

For rest of this lab you will use Swagger and make the API requests in the same way with the **Try it out!** and **Execute** buttons for each API method.


## Lesson 2 - Introduction to Reporting
### Objectives
*    Use the metrics and dimensions API methods to request lists of metrics and dimensions
*    Understand the data format for reporting
*    Run your first report against the analytics APIs

### Instructor Demo - Dimensions and Metrics
Analysis Workspace presents users with lists of dimensions and metrics in the left rail. It uses the /dimensions and /metrics API methods to get those lists.

#### Exercise 2.1 - Querying Lists of Dimensions
Programmatically get a list of dimensions available in a report suite by calling the **/dimensions** API method according to the following steps: 

1.    Make sure that you have followed the steps in [Lesson 1 - Getting Started](#lesson-1---getting-started) and validated that you have API connectivity
2.    Locate and expand the **dimensions** section in the Swagger interface
3.    Click on **`GET /dimension`**
4.    Click the **Try it out!** button
5.    Enter **`geo1metrixxprod`** in the rsid box
6.    Click on the **Execute** button to run the API request

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

Information on the other fields in this request can be found in our Swagger documentation.
[Dimensions](https://adobedocs.github.io/analytics-2.0-apis/#/dimensions/dimensions_getDimensions)

#### Exercise 2.2 - Querying Lists of Metrics
Querying lists of metrics is similar to querying lists of dimensions. Request the list of available metrics for the report suite by calling the **/metrics** API method according to the following steps:

1.    Make sure that you have followed the steps in [Lesson 1 - Getting Started](#lesson-1---getting-started) and validated that you have API connectivity
2.    Locate and expand the **metrics** section of the documentation
3.    Click on **`GET /metrics`**
4.    Click the **Try it out!** button
5.    Enter **`geo1metrixxprod`** in the rsid box
6.    Click on the **Execute** button to run the API request

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

Information on the other fields in this request can be found in our Swagger documentation.
[Metrics](https://adobedocs.github.io/analytics-2.0-apis/#/metrics/getMetrics)

### Instructor Demo - Analysis Workspace Report
Now that you know how to get lists of metrics and dimensions, you are ready to run your first report. In Analysis Workspace, you simply drag dimensions and metrics into the table panel.

### Understanding the report request
In order to run a report programmatically you need to construct a report request. Review the following request:
 
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
        "id": "metrics/occurrences"       <-- These metric id values come from the /metrics API method
      }
    ]
  },
  "dimension": "variables/page"           <-- This dimension id value comes from the /dimensions API method
}
```

A report request has several important parts:

#### rsid 
The required rsid parameter specifies the report suite ID for the report suite that you want the data to come from.

#### globalFilters 
The required globalFilters parameter contains a collection of filters that apply to the entire request. At a minimum a filter specifying the date range for the given report is required. 

#### metricContainer
This required parameter defines all the metrics for the given report request and will be represented as the columns in the report response. 

##### metrics
The array holds the metrics for the requested report. Metrics have two required parts to their definition: id and columnId. There is also an optional sort part of a metric definition. 

###### id 
The id of the metric. Use the /metrics or /calculatedmetrics API methods for the list of available options

#### dimension (optional) 
The optional dimension parameter will cause the report to return data for a particular dimension. The dimension can be any dimension from the /dimensions API method, including the daterange dimensions.

#### Exercise 2.3 - Running Your First Report
1.    Make sure that you have followed the steps in [Lesson 1 - Getting Started](#lesson-1---getting-started) and validated that you have API connectivity
2.    Scroll down and expand the **reports** section 
3.    Click on **`/reports`** to expand the documentation for that method
4.    Click the **Try it out!** button
5.    Paste the following JSON report request into the body text box
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
6.   Click the **Execute** button


Take a look at the results.  Do they match this Analysis Workspace report?

![s1_exercise3_results](images/s1_exercise3_results.png?raw=true)


### Understanding the Report Response
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

#### totalPages
How many pages of data are in this response. 

#### firstPage
boolean value set to true if this is the first page

#### lastPage
boolean value set to true if this is the last page. 

#### numberOfElements
Integer value specifying the number of rows in the current page

#### number
Integer value specifying the current page number

#### totalElements
This is the total number of rows in this response

#### columns
This object holds the description of the columns in the response.

##### dimension
This describes the dimension of the report response which will be represented as the itemId and value in the rows section of the response. The dimension has an id and a type.

##### columnIds
This is an array of the columns or metrics represented in the report. The values in this array match the columnIds passed in on the request. If you didn't specify any columnIds in the request, the are generated for you.

#### rows
This is an array of rows for the report response.
Each row object has the following attributes

##### itemId
This is the id for this particular value of the dimension. This s the id value that you will need if you are going to perform breakdowns or reference this dimension value in future report requests.

##### value
This is the value of the dimension and will be the value that was passed into this dimension during data collection.

##### data
This is an array of data for the metrics that were requested in this report request. Reference the columnIds array in the columns object to understand what this data represents.

#### SummaryData
This is an array of aggregated or calculated data for this report.

##### totals
This array holds the totals over the requested date range for the metrics in this report request. Reference the columnIds array in the columns object to understand what each item in the totals array represents. 

#### Exercise 2.4 - Multiple Metrics in a Single Request and Sorting
1.    Make sure that you have followed the steps in [Lesson 1 - Getting Started](#lesson-1---getting-started) and validated that you have API connectivity
2. Scroll down and expand the **reports** section 
3. Click on **`/reports`** to expand the documentation for that method
4. Click the **Try it out!** button
5. Modify the report request from Exercise 4 so that you are requesting the **Unique Visitors**, **Product Views**, and **Cart Additions** metrics, sorted **descending** by **Unique Visitors**.

 **You will need to edit the following JSON request before pasting into the body text box**:
  * The id for the **Unique Visitors** metric is **`metrics/visitors`** 
  * The **sort** is **`desc`** to sort the **Unique Visitors** metric descending
  * The id for the **Product Views** metric is **`metrics/productinstances`** 
  * The id for the **Cart Additions** metric is **`metrics/cartadditions`** 
  * The id for the **dimension** is **`variables/product`**

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
                "columnId": "1",
                "id": "<edit this>",
                "sort": "<edit this>"
            },
            {
                "columnId": "2",
                "id": "<edit this>"
            },
            {
                "columnId": "3",
                "id": "<edit this>"
            }
        ]
    },
    "dimension": "<edit this>"
}
```
**Hint:** A sort on column can be ascending ("asc") or descending ("desc")

6.   Click on **Execute**


Take a look at the results.  Do they match the following Analysis Workspace report?

![s1_exercise5_results](images/s1_exercise5_results.png?raw=true)

### Lesson 2 Complete
Congratulations! You've completed Lesson 2 and should understand the basic elements of running a report using the new Analytics V2 API. If you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

#### Extra Credit Challenge (optional)
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. How many unique visitors from the U.S. state of California visited our site on February 28, 2018?
2. How many unique visitors looked at the "Slickrock Trail Bike" product on February 28, 2018 and how many times was it added to a shopping cart that day?

#### Further Reading (optional)
* [Reports](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/reporting-guide.md)
* [Dimensions](https://adobedocs.github.io/analytics-2.0-apis/#/dimensions/dimensions_getDimensions)
* [Metrics](https://adobedocs.github.io/analytics-2.0-apis/#/metrics/getMetrics)

## Lesson 3 - Performing Breakdowns and Searching
### Objectives
*    Run a breakdown report
*    Perform a search 

### Instructor Demo - Breakdowns
A breakdown report filters a dimension based on the specific value of another dimension

### Understanding a Breakdown Request

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

#### Exercise 3.1 - Performing a Breakdown
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

![s2_exercise1_results](images/s2_exercise1_results.png?raw=true)


#### Exercise 3.2 - Breakdown Page by Browser 
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

![s2_exercise2_results1](images/s2_exercise2_results1.png?raw=true)

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

![s2_exercise2_results2](images/s2_exercise2_results2.png?raw=true) 

### Instructor Demo - Searching
Analysis Workspace can also do searches on dimension values. 

### Understanding a Search Attribute
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


#### Exercise 3.3 - Peforming a Search
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

#### Exercise 3.4 - Searching with Operators
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

![s2_exercise4_results](images/s2_exercise4_results.png?raw=true) 


#### Exercise 3.5 - Searching with different criteria
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

![s2_exercise5_results](images/s2_exercise5_results.png?raw=true) 

### Lesson 3 Complete
Congratulations! You've completed Lesson 3 and should understand breakdowns and searching in the new Analytics V2 API. If you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

#### Extra Credit Challenge (optional)
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. Try to do a multiple-level deep breakdown by finding out how many **Unique Visitors** viewed the "UltraTech Socks" **Product** while searching with the "outdoor gear" keyword on a mobile device manufactured by "Samsung". Hint - You will need more than one filter in the filters array.
2. What is the total number of unique visitors that looked at "Timberline GTX Boots" while using a mobile device manufacured by a company with the letter 'a' in it's name?

#### Further Reading (optional)
* [Reports](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/reporting-guide.md)  Includes information on Breakdowns and Searches

## Lesson 4 - Filtering Data and Segmentation
### Objectives
* Get lists of segments
* Run a report with a segment
* Create a segment

### Instructor Demo - Getting Segments
Analysis Workspace presents users with lists of segments in the left rail. It uses the `GET /segments` API method to get the list.

#### Exercise 4.1 - Getting segments
1. Using the Swagger interface as in past exercise, locate and expand the **segments** section
2. Select the **`GET /segments`** API method
3. Click the **Try it out!** button
4. Specify **`shared`** in the includeType field
5. Click the **Execute** button to submit the request
6. You result should look something like this:

```javascript
[
  {
    "id": "s300007301_5aa1af8b7f0bfd2308804475",              <-- segment id
    "name": "Browser contains Chrome",
    "description": "",
    "rsid": "geo1metrixxgeometrixx.outdoors",
    "owner": {
      "id": 722124					      
    }
  },
  {
    "id": "s300007301_5ab1b4fc5ef3a55946dce0d0",
    "name": "Mobile Hits",
    "description": "Mobile Hits",
    "rsid": "geo1metrixxprod",
    "owner": {
      "id": 706903
    }
  }
]
```
6. Notice the Segment's **`id`** returned from this response. We will need to use segment ids to reference segments in the report request.

### Instructor Demo - Global Segment
A global segment applies to the entire report request

#### Exercise 4.2 - Running a Report with a Global Segment
1. Using the **`POST /reports`** API method as in previous exercises, run a report on the **Browsers** dimension and **Occurrences** metric using the segment "Browser Contains Chrome" as a global segment: 

```javascript
{
    "rsid": "geo1metrixxprod",
    "globalFilters": [
        {
            "type": "segment",
            "segmentId": "s300007301_5aa1af8b7f0bfd2308804475"
        },
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
    "dimension": "variables/browser"
}
```

Does your result match Analysis Workspace?

![s3_exercise2_results](images/s3_exercise2_results.png?raw=true)

### Instructor Demo - Segment as Metric Filter
A segment can be applied as a filter on one or more metrics rather than the entire report request

#### Exercise 4.3 - Running a Report with a Segment Metric Filter
1. Using the **`POST /reports`** API method as in previous exercises, run a report on the **Browsers** dimension and **Occurrences** metric using the segment "Browser Contains Chrome" as a metric filter:

**You will need to edit the following JSON request before pasting into the body box**
  * The **type** of the metric filter should be **`segment`**
  * The **segmentId** in the metric filter should be **`s300007301_5aa1af8b7f0bfd2308804475`**

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
                "sort": "desc",
                "filters": [
                    "0"
                ]
            }
        ],
        "metricFilters": [
            {
                "id": "0",
                "type": "<edit this>",
                "segmentId": "<edit this>"
            }
        ]
    },
    "dimension": "variables/browser"
}
```

Does your result match Analysis Workspace?

![s3_exercise3_results](images/s3_exercise3_results.png?raw=true)


### Section 4 Complete
Congratulations! You've completed Section 4 and should understand how to use segments in the new Analytics V2 API. If you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

#### Extra Credit Challenge (optional)
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. What is the most popular mobile device for users in the segment of "Browser contains Chrome"?
2. What is the least popular search engine for users in the segment of "Browser contains Chrome"?

#### Further Reading (optional)
* [Segments Guide](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/segments-guide.md)

## Lesson 5 - Trended Data

### Objectives
* Understand the data format for requesting trended data   

### Instructor Demo - Running a report with a time granularity as dimension
You can use a time granularity as a dimension in Analysis Workspace report to get a trended report

### Understanding a Trended Report Request
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
The settings provide several parameters that control how the data is returned in the response.

#### limit (optional)
Sets an upper limit on the number of data rows to return. The default is 50.

#### page (optional)
The page number when you want to chunk up the requests. Example: If the date range we are reporting against has 90 days worth of data setting the page attribute to 0 with a limit of 50 would return the first 50 days. Setting page to 1 would return the remaining 40 days.

#### dimensionSort (optional)
The sort direction of the data ("asc" for ascending, "desc" for descending). Including this attribute will sort the data by the dimension values instead of by a metric. 

#### Exercise 5.1 - Trend a metric 
Trend the **Occurrences** metric by hour.

1. Using Swagger and the **`POST /reports`** endpoint as in past exercises, paste the following JSON report request into the body text box:

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

2. Click the **Execute** button to submit the request
3. Do your results match Analysis Workspace?

![s4_exercise1_results](images/s4_exercise1_results.png?raw=true)

### Lesson 5 Complete
Congratulations! You've completed Lesson 5 and should understand how to request trended reports. If you have some extra time you can try the Optional Challenge below for some "extra credit", as well as explore the documentation further.

#### Extra Credit Challenge (optional)
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. What day of the month recieved the most traffic from the apple iphone?
2. Which product is most popular during the hour of the day that gets the most traffic?

## Lesson 6 - Tips and Tricks

### Objectives
*    Learn how to run report requests from the Analysis Workspace UI in the API

### Analysis Workspace Debugger
The new Analytics V2 API powers Analysis Workspace. Analysis Workspace includes a debugger that will let you view the different API requests it makes. This debugger is very helpful in learning how to craft report requests and exploring the full power of the new Analytics API.

#### Exercise 6.1 
1. Log into Analysis Workspace as described in Lesson 1

![s1_landing_page](images/s1_landing_page.png?raw=true)

2. Click on the **Create New Project** button to create a new project

![s1_create_new_project](images/s1_create_new_project.png?raw=true)

3. Scroll down and select the **Products** template

![s1_retail_template](images/s1_retail_template.png?raw=true)

4. Open the browser's developer tools 

On a Mac:

![s5_open_dev_tools](images/s5_open_dev_tools.png?raw=true)

On a PC:

![s5_open_dev_tools_pc](images/s5_open_dev_tools_pc.png?raw=true)

5. Select the **Console** tab and enter `adobe.tools.debug.includeOberonXml = true` into the console and press Enter.

![s1_debug_text](images/s1_debug_text.png?raw=true)

6. Refresh the page

7. Notice that you now have a debugger icon on every panel!

8. Locate the **Product Performance Grid** panel in the Analysis Workspace project and click on the debugger icon, then click on **Freeform Table**

9. Notice that there are possibly multiple requests per panel
![s1_debug_link](images/s1_debug_link.png?raw=true)

10. Click on one of the requests

11. Copy the text from the **JSON REQUEST** box by either manually selecting the text or using the handy **Copy to Clipboard** button
![s1_copy_json](images/s1_copy_json.png?raw=true)

12. Click the **Try it out!** button

12. Paste the JSON into the **`/reports`** endpoint in the Swagger interface

14. Click the **Execute**

15. Do your results match Analysis Workspace?

16. Open the browser's developer tools again

On a Mac:

![s5_open_dev_tools](images/s5_open_dev_tools.png?raw=true)

On a PC:

![s5_open_dev_tools_pc](images/s5_open_dev_tools_pc.png?raw=true)

17. Select the **Console** tab and enter `adobe.tools.debug.includeOberonXml = false` into the console and press Enter.

18. Refresh the page. The debug icon should be gone.

### Lesson 6 Complete
Congratulations! You have completed Lesson 6 and should know how to use the Analysis Workspace debugger to help you format report requests.

### Lab Complete
You have completed the lab! If you have additional time, you can try some of the Extra Credit Challenges in the previous sections.

## Appendix
* [Adobe IO](https://www.adobe.io)
* [Reports](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/reporting-guide.md)
* [Dimensions](https://adobedocs.github.io/analytics-2.0-apis/#/dimensions/dimensions_getDimensions)
* [Metrics](https://adobedocs.github.io/analytics-2.0-apis/#/metrics/getMetrics)
* [Segments Guide](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/segments-guide.md)