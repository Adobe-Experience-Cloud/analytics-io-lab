Section 3 – Segmentation
====

**Go back to [Section 2](../s2_breakdown_search) | Skip to [Section 4](../s4_trended_data) »**

Objectives
----
* Get lists of segments
* Run a report with a segment
* Create a segment

Instructor Demo - Getting Segments
-----
Analysis Workspace presents users with lists of segments in the left rail. It uses the `GET /segments` API method to get the list.

Section 3, Exercise 1 - Getting segments
-----
1. Using the Swagger interface as in past exercise, locate and expand the **segments** section
2. Select the **`GET /segments`** API method
3. Specify **`shared`** in the includType field
4. Click the **Try it out!** button to submit the request
5. You result should look something like this:

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
6. Notice the Segment's **`id`** returned from this response. We will need to use segment ids to reference segments in the ranked report request.

Instructor Demo - Global Segment
-----
A global segment applies to the entire report request

Section 3, Exercise 2 - Running a Report with a Global Segment
-----
1. Using the **`POST /reports/ranked`** API method and **Try it out!** button as in previous exercises, run a report on the **Browsers** dimension and **Occurrences** metric using the segment "Browser Contains Chrome" as a global segment: 

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

![s3_exercise2_results](../../images/s3_exercise2_results.png?raw=true)

Instructor Demo - Segment as Metric Filter
-----
A segment can be applied as a filter on one or more metrics rather than the entire report request

Section 3, Exercise 3 - Running a Report with a Segment Metric Filter
-----
1. Using the **`POST /reports/ranked`** API method and **Try it out!** button as in previous exercises, run a report on the **Browsers** dimension and **Occurrences** metric using the segment "Browser Contains Chrome" as a metric filter:

**You will need to edit the following JSON before pasting into the body box**
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

![s3_exercise3_results](../../images/s3_exercise3_results.png?raw=true)


Section 3 Complete
-----
Congratulations! You've completed Section 3 and should understand how to use segments in the new Analytics V2 API. You can **Continue to [Section 4](../s4_trended_data) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. What is the most popular mobile device for users in the segment of "Browser contains Chrome"?
2. What is the least popular search engine for users in the segment of "Browser contains Chrome"?

Further Reading (optional)
-----
* [Segments Guide](https://github.com/AdobeDocs/analytics-2.0-apis/blob/master/segments-guide.md)

**Go back to [Section 2](../s2_breakdown_search) | Continue to [Section 4](../s4_trended_data) »**
