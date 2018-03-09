Section 3 – Filtering and Segmentation
====

Objectives
----
* Run a report with a segment
* Understand other forms of filtering

Getting segments
-----
1. Make sure that you have followed the steps above to [Access the Swagger UI](../s1_api_intro#accessing-the-swagger-interface)
2. Expand the GET /segments API endpoint
3. Click the "Try it out!" button to submit the request
4. You result should look something like this:
```javascript
[
  {
    "id": "s300007301_5aa1af8b7f0bfd2308804475",
    "name": "Browser contains Chrome",
    "description": "",
    "rsid": "geo1metrixxprod",
    "owner": {
      "id": 722124
    }
  }
]
```
5. Notice the `id` returned from this response. We will need to use this to reference this segment in the ranked report request.

Running a report with a segment
-----
1. Expand the POST /reports/ranked API endpoint
2. Paste the following json report request into the body text box:
```javascript
{
   "rsid":"geo1metrixxprod",
   "dimension":"variables/browser",
   "globalFilters":[
      {
         "type":"dateRange",
         "dateRange":"2018-01-01T00:00/2018-02-28T00:00"
      },
      {
         "type":"segment",
         "segmentId":"s300007301_5aa1af8b7f0bfd2308804475"
      }
   ],
   "metricContainer":{
      "metrics":[
         {
            "id":"metrics/visitors",
            "sort":"desc"
         }
      ]
   }
}
```
3. Click the "Try it out!" button to submit the request
4. Take a look at the results. You should have data where the browser contains "Chrome" sort by visitors descending.

Other forms of filtering
-----
1. Expand the POST /reports/ranked API endpoint
2. Paste the following json report request into the body text box:
```javascript
{
  "rsid": "geo1metrixxprod",
  "dimension": "variables/page",
  "globalFilters": [
    {
      "type": "dateRange",
      "dateRange": "2018-01-01T00:00/2018-02-28T00:00"
    }
  ],
  "search": {
    "clause": "home",
    "includeSearchTotal": true
  },
  "metricContainer": {
    "metrics": [
      {
        "id": "metrics/visitors",
        "sort": "desc"
      }
    ]
  }
}
```
3. Click the "Try it out!" button to submit the request
4. Take a look at the results. You should have data where the page dimension value contains the clause "home" sorted by visitors descending. You should also have a searchTotals array in the response with the totals of your search for "home".

Further Reading (optional)
-----
** TODO - insert links to Segment documentation

**Continue to [Section 4](../s4_trended_data) »**
