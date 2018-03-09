# Ranked Response
This page defines the components of a report response and gives some background on how to interpret these responses.

Every request through the ranked reporting endpoint comes back in the form of a table consisting of rows and columns. If there are more rows of than the selected page size then the response will also be broken into multiple pages to make it easier to download the data in smaller chunks.

Here is a partial example response:
>		{
>		  "totalPages": 3,
>		  "firstPage": true,
>		  "lastPage": false,
>		  "numberOfElements": 50,
>		  "number": 0,
>		  "totalElements": 121,
>		  "columns": {
>		    "dimension": {
>		      "id": "variables/page",
>		      "type": "string"
>		    },
>		    "columnIds": [
>		      "pageviews",
>		      "visits"
>		    ]
>		  },
>		  "rows": [
>		    {
>		      "itemId": "2390504924",
>		      "value": "Shopping Cart|Cart Details",
>		      "data": [
>		        40842,
>		        19791
>		      ]
>		    },
>		    {
>		      "itemId": "2897271828",
>		      "value": "=Search Result",
>		      "data": [
>		        27901,
>		        16534
>		      ]
>		    }
>		  ],
>		  "summaryData": {
>		    "totals": [
>		      343852,
>		      34035
>		    ]
>		  }
>		}

## totalPages
How many pages of data are in this response. 

## firstPage
boolean values stating if this is the first page

## lastPage
boolean values stating if this is the last page

## numberOfElements
This tells you how many elements are in this page

## number
this tell you the current page represented in this response

## total Elements
This is the total number of rows in this response

## columns
This object holds the description of the columns in the response.

### dimension
This describes the dimension of the report response which will be represented as the itemId and value in the rows section of the response. The dimension has an id and a type.

### columnIds
This is an array of the columns or metrics represented in the report. The values in this array match the columnIds passed in on the request.

## rows
This is an array of rows for the report response.
Each row object has the following atributes

### itemId
This is the id for this particular value of the dimension. This the id that you will need if you are going to perform breakdowns or reference this dimension value in future report requests.

### value
This is the value of the dimension and will be the value that was passed into this dimension during data collection.

### data
This is an array of data for the metrics that were requested in this report request. Reference the columnIds array in the columns object to understand what this data represents.

## SummaryData
This is an array of aggregated or calculated data for this report.

### totals
This array holds the totals for the metrics that were requested in this report request for the requested time period. Reference the columnIds array in the columns object to understand what each item in the totals array represents. If the report request contained a dimension then this is the count of every time the metric was incremented when the dimension was sent in.  If the report did not include a dimension then this is a count of every time the metric was incremented.
