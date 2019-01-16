Calculated metrics in reporting
====

Objectives
----
* Create a calculated metric
* Run a report with a calculated metric

Creating a new calculated metric
-----
1. Make sure that you have followed the steps above to [Access the Swagger UI](../s1_api_intro#accessing-the-swagger-interface)
2. Expand the `calculatedmetrics` section
3. Expand the `POST /calculatedmetrics` API endpoint
4. Click the "Try it out!" button
5. Paste the following json segment definition into the body text box. This will create a new calculated metric that gives the average visits per visitor by dividing `visits` by `visitor`.
```javascript
{
  "name": "Average Visits per Visitor",
  "description": "Visits divided by Visitors",
  "rsid": "sistr2",
  "polarity": "positive",
  "precision": 0,
  "type": "decimal",
  "definition": {
    "formula": {
      "col1": {
        "name": "metrics\/visits",
        "func": "metric"
      },
      "col2": {
        "name": "metrics\/visitors",
        "func": "metric"
      },
      "func": "divide"
    },
    "func": "calc-metric",
    "version": [1,0,0]
  }
}
```
6. Click the "Execute" button to submit the request
7. You result should look something like this:
```javascript
{
  "id": "cm6638_5a9986e0dc0e82002168db6e",
  "name": "Average Visits per Visitor",
  "description": "Visits divided by Visitors",
  "rsid": "sistr2",
  "owner": {
    "id": 82365
  },
  "polarity": "positive",
  "precision": 0,
  "type": "decimal",
  ...
}
```
7. Notice the `id` returned from this response. We will need to use this to reference this calculated metric in the ranked report request.

Running a report with a calculated metric
-----
1. Expand the reports section
2. Expand the POST /reports API endpoint
3. Click the "Try it out!" button
4. Paste the following json report request into the body text box:
```javascript
{
   "rsid":"geometrixx1",
   "dimension":"variables/daterangeday",
   "globalFilters":[
      {
         "type":"dateRange",
         "dateRange":"2018-01-01T00:00/2018-02-28T00:00"
      }
   ],
   "settings":{
      "limit":50,
      "page":0,
      "dimensionSort":"asc"
   },
   "metricContainer":{
      "metrics":[
         {
            "id":"INSERT_CALCULATED_METRIC_ID_HERE"
         },
         {
            "id":"metrics/visits"
         },
         {
            "id":"metrics/visitors"
         }
      ]
   }
}
```
5. Replace the `INSERT_CALCULATED_METRIC_ID_HERE` text with the id of the calculated metric created above.
6. Click the "Execute" button to submit the request
7. Take a look at the results. Your data should look something like this:
```javascript
{
  "totalPages": 2,
  "firstPage": true,
  "lastPage": false,
  "numberOfElements": 50,
  "number": 0,
  "totalElements": 58,
  "columns": {
    "dimension": {
      "id": "variables/daterangeday",
      "type": "time"
    },
    "columnIds": [
      "ebbecc46-88b5-4eed-9650-6cc98c6aabd1",
      "c02ab2eb-e953-4945-8d7f-d9de32233245",
      "46e9b8d5-4e54-41ae-aa00-126bdde3717c"
    ]
  },
  "rows": [
    {
      "itemId": "1180001",
      "value": "Jan 1, 2018",
      "data": [
        1.0017642907551165,
        2839,
        2834
      ]
    },
    {
      "itemId": "1180002",
      "value": "Jan 2, 2018",
      "data": [
        1.0014109347442681,
        2839,
        2835
      ]
    },
    ...
```
8. Notice the first column of data is the value of the visits column divided by the visitors column.

Further Reading (optional)
-----
* [Calculated Metrics](https://adobedocs.github.io/analytics-2.0-apis/#/calculatedmetrics)

**Continue to [Components](../components) »**
