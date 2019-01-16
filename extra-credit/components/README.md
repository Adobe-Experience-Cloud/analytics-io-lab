Components
====

Objectives
----
* Learn how to share, tag and favorite Analytics Components

Understanding Analytics Components
-----
An Analytics Component is an abstraction that allows sharing, tagging and favorites to be done generically across several Analytics assets. A few examples of components we have used during this lab are Segments and CalculatedMetrics.

Sharing API
-----
In order to understand how to share a component let's take a look at the required attributes to create a share object.
```javascript
{
  "shareToType": "user",
  "shareToId": 1234568111,
  "componentType": "calculatedMetric",
  "componentId": "cm6638_5a9986e0dc0e82002168db6e"
}
```

### shareToType
This is the type of share you are creating. The allowed values for shareToType are:
```
user
group
all
```

### shareToId
The id of the entity to which the component is being shared. In the case of user this is the numeric loginId of the user. For groups it is the numeric groupId. When sharing with `all` this attribute is not required.

### componentType
The type of the component that is being shared. Examples:
```
segment
calculatedMetric
...
```

### componentId
The id of the component that is being shared. Example: `cm6638_5a9986e0dc0e82002168db6e` is the id for the calculated metric above.

Sharing a calculated metric with another user
----
1. Get the numeric loginId of the user to which you would like to share the calculated metric.
2. Expand the `users` section in swagger.
3. Expand the `GET /users` API endpoint. The default will limit the result to 10 users. You can change the limit using the `limit' query parameter if you would like.
4. Click the `Try it out!` button and then the `Execute` button.
5. Look at the results.
```javascript
[
    {
      "companyid": 6638,
      "loginId": 1234573865,
      "login": "analyticstest1",
      "admin": false,
      "changePassword": null,
      "createDate": "2017-01-06T09:59:23",
      "disabled": false,
      "email": "user@example.com",
      "firstName": "analytics",
      "fullName": "analytics test1",
      "imsUserId": null,
      "lastName": "test1",
      "lastLogin": null,
      "phoneNumber": null,
      "tempLoginEnd": null,
      "title": "test"
    },
    ...
]
```
6. Find the user from the result list you would like and get id from the `loginId` attribute.
7. If you would like to create a new calculated metric follow the steps from the [Previous Section](../calculated-metrics) or you can just use the id of the calculated metric you already created in the previous section.
8. Expand the `shares` section.
9. Expand the `POST /shares` API endpoint.
10. Click the "Try it out!" button
11. Paste the following json in the post body:
```javascript
{
  "shareToType": "user",
  "shareToId": INSERT_LOGIN_ID_HERE,
  "componentType": "calculatedMetric",
  "componentId": "INSERT_CALCULATED_METRIC_ID_HERE"
}
```
12. Replace the loginId and componentId values with the ids we obtained earlier.
13. Click the `Execute` button.
14. Check your result. You should have something that looks like the following:
```javascript
{
  "shareId": 149302,
  "shareToId": 1234568111,
  "shareToType": "user",
  "componentType": "calculatedMetric",
  "componentId": "cm6638_5a9986e0dc0e82002168db6e",
  "shareToDisplayName": "Shawn Austin",
  "shareToLogin": "shaustin"
}
```

Tagging API
-----
Tags are used in the Analytics UI to help make it easy to organize components and find them quickly. In this part of the tutorial we will walk through tagging the calculated metric shared above. Let's first take a look at the structure of a tag object:
```javascript
[
  {
    "name": "TagTest",
    "description", "Awesome description of this tag"
    "components": [
      {
        "componentType": "calculatedMetric",
        "componentId": "cm6638_5a9986e0dc0e82002168db6e"
      }
    ]
  }
]
```

### name
The name you would like to give this tag.

### description (optional)
The description of this tag.

### components
List of the components you would like to tag with the tag you are creating.

#### componentType
The type of the component that is being tagged. Examples:
```
segment
calculatedMetric
...
```
#### componentId
The id of the component that is being tagged.

Tagging a calculated metric with a new tag
----
1. Expand the `tags` section in swagger.
2. Expand the `POST /tags` API endpoint.
3. Click the "Try it out!" button
4. Paste the following json in the post body:
```javascript
[
  {
    "name": "TagTest",
    "components": [
      {
        "componentType": "calculatedMetric",
        "componentId": "INSERT_CALCULATED_METRIC_ID_HERE"
      }
    ]
  }
]
```
4. Replace the `INSERT_CALCULATED_METRIC_ID_HERE` with the id of the calculated metric you would like to tag.
5. Click the `Execute` button.
6. Check your result. You should have a response similar to this:
```javascript
[
  {
    "id": 28206,
    "name": "MikeTagTest",
    "components": [
      {
        "componentType": "calculatedMetric",
        "componentId": "cm6638_5a9986e0dc0e82002168db6e"
      }
    ],
    "status": {
      "success": true
    }
  }
]
```

Favorites API
-----
Favorites can be used to mark components as important and make them more easily accessible. In this part of the tutorial we will mark the calculated metric from earlier as a favorite. Let's take a look at the structure of a favorite object:
```javascript
[
  {
    "componentType": "calculatedMetric",
    "componentId": "cm6638_5a9986e0dc0e82002168db6e"
  }
]
```

### componentType
The type of the component that is being marked as a favorite. Examples:
```
segment
calculatedMetric
...
```
### componentId
The id of the component that is being marked as a favorite.

Marking a calculated metric as a favorite
----
1. Expand the `favorites` section in swagger.
2. Expand the `POST /favorites` API endpoint.
3. Click the "Try it out!" button
4. Paste the following json in the post body:
```javascript
[
  {
    "componentType": "calculatedMetric",
    "componentId": "INSERT_CALCULATED_METRIC_ID_HERE"
  }
]
```
5. Replace the `INSERT_CALCULATED_METRIC_ID_HERE` with the id of the calculated metric you would like to tag.
6. Click the `Try it out!` button.
7. Check your result. You should have a response similar to this:
```javascript
[
  {
    "favoriteId": 47572,
    "componentType": "calculatedMetric"
    "status": {
      "success": true
    }
  }
]
```

Congratulations! You have finished the lab. Go enjoy the rest of summit!
