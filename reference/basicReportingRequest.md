# Ranked 
This reporting endpoint can be used for a broad range of reporting operations. This page defines some of the basic components of a reporting request as well as provides some basic examples of how to use it.

Here is an example request:
>		{
>		    "rsid": "geometrixx1",
>		    "dimension": "variables/browser",
>		    "locale": "en_US",
>		    "globalFilters": [
>		        {
>		            "dateRange": "2014-06-01T00:00/2014-06-21T00:00",
>		            "type": "dateRange"
>		        },
>		        {
>		            "segmentId": "537bd8d2e4b07e66e428dd63",
>		            "type": "segment"
>		        }
>		    ],
>		    "metricContainer": {
>		        "metricFilters": [
>		            {
>		                "dateRange": "2014-06-01T00:00/2014-06-15T00:00",
>		                "id": "dateRange1",
>		                "type": "dateRange"
>		            },
>		            {
>		                "dimension": "variables/pages",
>		                "id": "breakdown1",
>		                "itemId": 12345678,
>		                "type": "breakdown"
>		            }
>		        ],
>		        "metrics": [
>		            {
>		                "columnId": "testId",
>		                "filters": [
>		                    "breakdown1",
>		                    "dateRange1"
>		                ],
>		                "id": "metrics/pageviews"
>		            },
>		            {
>		                "columnId": "284e196a-91b4-4982-a34d-70b1906d93e3",
>		                "filters": [
>		                    "dateRange1"
>		                ],
>		                "id": "metrics/visits"
>		            }
>		        ]
>		    },
>		    "search": {
>		        "clause": "( \'Search String\' )",
>		        "includeSearchTotal": true
>		    },
>		    "settings": {
>		        "dimensionSort": "asc",
>		        "limit": 50,
>		        "page": 0,
>		    },
>		    "statistics": {
>		        "functions": [
>		            "col-min",
>		            "col-max",
>		            "stdev"
>		        ],
>		        "ignoreZeroes": true
>		    }
>		}


## rsid
This defines the report suite string ID for the report suite that you want the data to come from. This id can be found in the report suite management section of the admin console or through the report suites API call.

## dimension (optional) 
This parameter will make the report data be in the context of a particular dimension. The dimension can be any dimension from the dimensions endpoint including the daterange dimensions. If this parameter is not included then you will get a response with just the totals for the given metric.

## locale (optional)
This parameter defines the locale for system generated strings (ie. country or city names). This parameter will default to "en_US".

## globalFilters 
This parameter contains a collection of filters that apply to the the entire request. At a minimum a filter specifying the date range for the given report is required.
See the filter definition documentation for details on what types of filters are available.

## metricContainer
This parameter defines all of the metrics for the given report request and will be represented as the columns in the report response.

### metricFilters
This section defines a set of filters that may be applied to the metrics in the report request

### metrics
This collection holds the metrics for the requested report. Below is the explanation for the various sections of a metric

#### id 
The id of the metric. Check the /metrics or /calculatedmetrics endpoints for the list of available options

#### columnId
The name of the column of data that will be returned. You may use the same metric multiple times with different filters in the report response so providing ids for the response makes it easier to identify them in the response.

#### filters
An array of the metricsFilter ids (defined in the metricFilters section) that should be applied to this metric

## search
Allows you to search for certain strings in the dimension that was defined for this report.

### clause
The search clause string is surrounded by single quote characters. Single quote, back slash and forward slash need to be escaped with a back slash to be part of the search text. The following operators are supported (AND, OR, NOT)(not case sensitive) and parenthesis are allowed for grouping.

#### includeSearchTotal
Adds searchTotals array that contains the sum of the items that match the search. This does nothing unless another search parameter is specified OR excludeItems global filter is specified.

## settings
This section lets you define some basic reporting information for this report

### limit 
How many items should be returned (max of 50,000)

### page
Which page of data to be returned for requests that have more rows than the requested limit

### dimensionSort
How to sort the dimension values. Valid entries are ‘asc’ and ‘desc’. Certain variables don’t sort like you would expect?

## statistics
Will return extra rows in the response with the additional statistical information

Available options are:

* col-sum 
* count-rows
* col-min
* col-max
* stdev
	The standard deviation of this column
* col-min
* col-max
* mean
* variance
* stdev
* median
* quartile1
* quartile3 

### ignoreZeroes
This should be true if you wish to ignore zero values in statistical calculations

## Filters Definition
Filters objects can be accepted in both the globalFilters and metricFilters section of a report request. All filter definitions require both an id and a type attribute, but the parameters required for each type vary.

### dateRange Type
This will make sure that the report only includes data from the provided date range. There should only be one of this type of filter in each report

#### dateRange
The only additional parameter for this type is the date range that must be in ISO 8601 time interval format (http://en.wikipedia.org/wiki/ISO_8601#Time_intervals). The time zone designation is not allowed since reporting is always based off of the time zone setting of the report suite. Currently only granularities down to the minute are supported and the hour and minute attribute are required. The end time is exclusive meaning that the data for the end minute is not returned.

### segment Type
This filter will ensure that all of the data in the report meets the criteria of the given segment. More than one of these filters can be provided in a request.

#### segmentId
The only additional parameter for this type is the segmentId. This id can be any one of the segment ids provided by the segment service.

### breakdown Type
This filter will make sure that all of the data passed in also had the given itemId passed in for the selected dimension in the same hit.

#### dimension
This is the dimension that the breakdown filter should reference and can be any of the items from the dimension service.

#### itemId
This is the id of the value referenced for the breakdown filter. You have to run a separate report for the given dimension to see all of the possible ids.


# Oberon Error Codes
Here is a list of common error codes
//TODO: need to take the most common ones and add some friendly docs
https://wiki.corp.adobe.com/pages/viewpage.action?spaceKey=dscplatform&title=Oberon+Response+Codes

# Thoughts

How well does this endpoint handle invalid data. 

Do we need any new types of testing ?
