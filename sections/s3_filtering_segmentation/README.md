Section 3 – Segmentation
====

**Go back to [Section 2](../s2_breakdown_search) | Skip to [Section 4](../s4_trended_data) »**

Objectives
----
* Get lists of segments
* Run a report with a segment
* Programmatically create a segment

Instructor Demo - Getting Segments
-----
Analysis Workspace presents users with lists of segments in the left rail. It uses the `GET /segments` API method to get the list.

Section 3, Exercise 1 - Getting segments
-----
1. Using the Swagger interface as in past exercise, locate and expand the **segments** section
2. Select the **`GET /segments`** API method
3. Specify **`shared`** in the includType field
4. Click the "Try it out!" button to submit the request
5. You result should look something like this:

```javascript
[
  {
    "id": "s300007301_5aa1af8b7f0bfd2308804475",              <-- Segment id
    "name": "Browser contains Chrome",
    "description": "",
    "rsid": "geo1metrixxgeometrixx.outdoors",
    "owner": {
      "id": 722124					      <-- owner id
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
                "type": "segment",
                "segmentId": "s300007301_5aa1af8b7f0bfd2308804475"
            }
        ]
    },
    "dimension": "variables/browser"
}
```

Does your result match Analysis Workspace?

![s3_exercise3_results](../../images/s3_exercise3_results.png?raw=true)

Instructor Demo - Segment as Dimension
-----
A segment can be applied as a filter on one or more metrics rather than the entire report request Section 3, Exercise 4 - Running a Report with a Segment Metric Filter

Section 3, Exercise 4 - Segment as Dimension
-----
1. Using the **`POST /reports/ranked`** API method and **Try it out!** button as in previous exercises, run a report on the **Browsers** dimension and **Occurrences** metric using the segment "Browser Contains Chrome" as a metric filter:

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
                "filters": [
                    "0"
                ]
            }
        ],
        "metricFilters": [
            {
                "id": "0",
                "type": "segment",
                "segmentId": "s300007301_5aa1af8b7f0bfd2308804475"
            }
        ]
    }
}
```

Does your result match Analysis Workspace?

![s3_exercise4_results](../../images/s3_exercise4_results.png?raw=true)

Instructor Demo - Segment Definition
-----
Segment definitions can be quite complex. It's easiest to build them in the UI.

Section 3, Exercise 5 - Getting a Segment Definition
-----
Using the Swagger interface, get the segment defition for a segment with the ID of **`s300007301_5ab28b65f30aae1bc76078ab`**

1. Locate and expand the **segments** section
2. Select the **`GET /segments/{id}`** API method.
3. The segment **id** paramaeter should be **`s300007301_5ab28b65f30aae1bc76078ab`**
4. The **expansion** parameter should be **`definition`**.  If you don't specifiy you want the definition then you'll just get back summary information about the segment.
5. Click the "Try it out!" button to submit the request
6. Your result should look something like this:

```javascript
{
  "id": "s300007301_5ab28b65f30aae1bc76078ab",
  "name": "Female Mobile Visitors Interested in Boots",
  "description": "Female Mobile Visitors Interested in Boots",
  "rsid": "geo1metrixxprod",
  "owner": {
    "id": 709117
  },
  "definition": {
    "container": {
      "func": "container",
      "pred": {
        "func": "container",
        "pred": {
          "func": "and",
          "preds": [
            {
              "val": {
                "func": "attr",
                "name": "variables/mobiledevicename"
              },
              "func": "exists",
              "description": "Mobile Device"
            },
            {
              "str": "Boots",
              "val": {
                "func": "attr",
                "name": "variables/evar6"
              },
              "func": "streq",
              "description": "Product Type"
            },
            {
              "str": "Female",
              "val": {
                "func": "attr",
                "name": "variables/evar9"
              },
              "func": "streq",
              "description": "Gender"
            }
          ]
        },
        "context": "hits",
        "description": "Mobile Hits"
      },
      "context": "hits"
    },
    "func": "segment",
    "version": [
      1,
      0,
      0
    ]
  }
}
```

Section 3, Exercise 6 - Saving a Segment
-----
Using the Swagger interface, create a new segment for **Male Mobile Visitors Interested in Socks** based on the existing segment definition for the segment **Female Mobile Visitors Interested in Boots**:

1. Locate and expand the **segments** section
2. Select the **`POST /segments`** API method.
3. Edit the following segment definition to change the segment name and the TODO 

** You will need to edit the following JSON before pasting into the body box**
  * The **name** should be **`Male Mobile Visitors Interested in Socks`**
  * The **description** should be **`Male Mobile Visitors Interested in Socks`**
  * The **str** value for the **Product Type** condition should be **`Socks`**
  * The **str** value for the **Gender** condition should be **`Male`**

```javascript
{
  "name": "<edit this>",
  "description": "<edit this>",
  "rsid": "geo1metrixxprod",
  "definition": {
    "container": {
      "func": "container",
      "pred": {
        "func": "container",
        "pred": {
          "func": "and",
          "preds": [
            {
              "val": {
                "func": "attr",
                "name": "variables/mobiledevicename"
              },
              "func": "exists",
              "description": "Mobile Device"
            },
            {
              "str": "<edit this>",
              "val": {
                "func": "attr",
                "name": "variables/evar6"
              },
              "func": "streq",
              "description": "Product Type"
            },
            {
              "str": "<edit this>",
              "val": {
                "func": "attr",
                "name": "variables/evar9"
              },
              "func": "streq",
              "description": "Gender"
            }
          ]
        },
        "context": "hits",
        "description": "Mobile Hits"
      },
      "context": "hits"
    },
    "func": "segment",
    "version": [
      1,
      0,
      0
    ]
  }
}
```
5. Click the "Try it out!" button to create the request.
6. Your result should look something like this:

```javascript
{
  "id": "s300007301_5ab2c2b73e958a5efd684913",                  <-- This will be different because every segment has a unique id
  "name": "Male Mobile Visitors Interested in Socks",
  "description": "Male Mobile Visitors Interested in Socks",
  "rsid": "geo1metrixxprod",
  "owner": {
    "id": 706903                                                <-- This will be different because it will match the id of your own user
  },
  "modified": "2018-03-21T20:38:15Z"
}
```
7. Select the **`GET /segments`** API method
8. Specify **`shared`** in the includType field
9. Click the "Try it out!" button to submit the request
10. You should see your newly created segment in the list!


Section 3 Complete
-----
Congratulations! You've completed Section 3 and should understand how to work with segments in the new Analytics V2 API. You can **Continue to [Section 4](../s4_trended_data) »** now, or if you have some extra time you can try the optional Challenge below for some "extra credit", as well as explore the documentation further.

Extra Credit Challenge (optional)
-----
This step is optional, for those who have extra time and like a challenge. Using the Report API methods and techniques you've already learned, as well as the documentation, see if you can answer the following questions:

1. What is the most popular mobile device for users in the segment of "Browser contains Chrome"?
2. What is the least popular search engine for users in the segment of "Browser contains Chrome"?

Further Reading (optional)
-----
* [Segments](https://adobe-experience-cloud.github.io/analytics-io-lab/analytics-api-reference-guide.html#_segments_resource)

**Go back to [Section 2](../s2_breakdown_search) | Continue to [Section 4](../s4_trended_data) »**
