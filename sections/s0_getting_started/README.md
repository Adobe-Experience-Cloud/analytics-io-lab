Section 0 - Getting Started
====

Objectives
----
* Learn how we will authenticate API calls in the lab

Experience Cloud Accounts
----
The new Analytics APIs require users and organizations to be configured for login via the Experience Cloud at https://marketing.adobe.com rather than the traditional Analytics login page at my.omniture.com.

Throughout 2018, all Analytics users and login companies will be migrated to Experience Cloud authentication. If you don’t know whether your Analytics company is configured for Experience Cloud authentication or have questions about migrating, please contact your Account Manager, Technical Account Manager, Customer Success Manager, or Customer Care representative.

In this lab you will be using an Experience Cloud organization and Analytics login company that is already configured for Experience Cloud Authentication. 

**Let's sign in!**
Go to https://marketing.adobe.com and select **Sign In with an Adobe ID**
![s0_exp_cloud_login](../../images/s0_exp_cloud_login.png?raw=true)

Look on the table near your computer for the username and password.  The username will be of the following format:

`apilab18+<User ID>@adobetest.com`

where the `<User ID >` section is replaced by your lab computer number.  For example, apilab18+37@adobetest.com

Enter your assigned username and the password provided to you and click **Sign In**
![s0_exp_cloud_login2](../../images/s0_exp_cloud_login2.png?raw=true)

You should be taken directly to the Analysis Workspace landing page in the **Analytics API Summit 2018** login company:
![s0_workspace_landing_page](../../images/s0_workspace_landing_page.png?raw=true)

Authentication
----
Upon initial release, the Analytics V2 APIs will support the Adobe I/O JSON Web Token (JWT) authentication method. OAuth supportwill be added in a future release. You can learn about the different authentication methods and how to configure JWT authentication via the Adobe I/O Authentication Overview:

https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html 

JWT libraries are available for many different languages such as Java, JavaScript (Node.js), Ruby, PHP, and .NET; however, in this lab we’re going to skip the programming steps and simply provide a valid access token for you to use in making API requests. 

### Getting the Lab Access Token

Make sure you're logged into Analytics with an `apilab18+<User Id>@adobetest.com` account, then select the **“Access Token”** bookmark from the browser’s bookmark bar:
![s0_access_token_bookmark](../../images/s0_access_token_bookmark.png?raw=true)

This will automatically copy an access token to your clipboard.  The access token is a long string like in the following example:

**Example Access Token (don't try using this one)**
>eyJ4NXUiOiJpbXNfbmExLWtleS0xLmNlciIsImFsZyI6IlJTMjU2In0.eyJpZCI6IjE1MTkxNjgyNzM3NzlfZjI1ZjQzNjMtOTIwMC00NDE3LWEzYzQtNjc4NGZkN2IzM2I0X3VlMSIsImNsaWVudF9pZCI6IlNpdGVDYXRhbHlzdDIiLCJ1c2VyX2lkIjoiRUJDQTJBOUM1QTY2NEJBQTBBNDk1Qzk0QEFkb2JlSUQiLCJzdGF0ZSI6IntcInNlc3Npb25cIjpcImh0dHBzOi8vaW1zLW5hMS5hZG9iZWxvZ2luLmNvbS9pbXMvc2Vzc2lvbi92MS9ZVGN5T0RBd1pqRXROVGxqTVMwMFlUWmtMV0UyTUdFdE9HWXdNekJqTlRBME9XSTRMUzFGUWtOQk1rRTVRelZCTmpZMFFrRkJNRUUwT1RWRE9UUkFRV1J2WW1WSlJBXCJ9IiwidHlwZSI6ImFjY2Vzc190b2tlbiIsImFzIjoiaW1zLW5hMSIsImZnIjoiU0dMUTdTNkdYUFA3TzdRQUFBQUFBQUFBVFE9PT09PT0iLCJydGlkIjoiMTUxOTE2ODI3Mzc4MV8wMDg5NDYyYS1hNTQ4LTQ3MTItYjdhZC1kOWU4ZjIwNDg5YWNfdWUxIiwib2MiOiJyZW5nYSpuYTFyKjE2MWI1N2MzYTI5KkJXV1BYWFJSVk4zMjlDTlpXWDEyQlg0Vko4IiwicnRlYSI6IjE1MjAzNzc4NzM3ODEiLCJtb2kiOiI2NWJjMGFkZiIsImMiOiJvdFJHT2lZaFN6STdzVlFwQ3N5OTBnPT0iLCJleHBpcmVzX2luIjoiODY0MDAwMDAiLCJzY29wZSI6Im9wZW5pZCxBZG9iZUlELHJlYWRfb3JnYW5pemF0aW9ucyxhZGRpdGlvbmFsX2luZm8ucHJvamVjdGVkUHJvZHVjdENvbnRleHQsYWRkaXRpb25hbF9pbmZvLmpvYl9mdW5jdGlvbixzZXNzaW9uIiwiY3JlYXRlZF9hdCI6IjE1MTkxNjgyNzM3NzkifQ.NiXWEImAS0dFJHBlYcI4TbFdBjVCty69ZanZZldjP3UCIUWrim4VFYDI9zkYre5QpLXdzOth-ORRB1C0nlgZZuFetbjE0Ig8-MzMDJe7S1G_wb4MFUxKF8kEyHFJmWrbrAud4B9NgQWz_TaUtCOjBBAQ0vvAxNuIlQdBxPPVP797BfT-osayfqVwDh-zluo2MrH9oQkWdwlKrd4FBh0wnwzI7-711CuGGpss5bYZdkeZo8_E4pO4EmN8PfzQI9GsGrmodrqJYJ48hMKt3Bcs7b8BxNXcYuaM7wpN3GluPjpyB-naJ8wVe0qY4D6WInCIw5gtQ2iSO3JLWmDKZbmLMQ

Whenever you need an access token during this lab, make sure you’re on a browser tab logged into Analytics and click the Access Token bookmark and one will be generated for you and automatically copied to your clipboard. If the access token you are using expires, simply generate a new one using the Access Token bookmark. 

Accessing the Swagger Interface
----
We will be using a Swagger interface to generate API requests in this lab. To access Swagger, open your web browser and navigate to [https://adobe-experience-cloud.github.io/adobe-analytics-lab-api-docs/](https://adobe-experience-cloud.github.io/adobe-analytics-lab-api-docs/?input_companyName=Analytics%20API%20Summit%202018&input_clientKey=aa_test_key#!/users)
![s0_swagger_interface](../../images/s0_swagger_interface.png?raw=true)

To make an API request in Swagger, you will need to make sure you have provided an access token on each request

Validate API Connectivity
----
Let's make sure everything works correctly. Click on the **Users** section in the Swagger interface and then select **GET /users/me**, then click **Try it out!**:
![s0_swagger_users_me](../../images/s0_swagger_users_me.png?raw=true)

If you entered the Access Token, Analytics Company, and Client Key correctly you should get a JSON response from the /users/me endpoint with information about your current user:

/users/me response
```javascript
{
  "companyid": 6638,
  "loginId": 82365,
  "login": "tester",
  "changePassword": null,
  "createDate": null,
  "disabled": false,
  "email": "user@example.com",
  "firstName": "Tester",
  "fullName": "Tester Tester",
  "imsUserId": null,
  "lastName": "Tester",
  "lastLogin": "2017-07-18T00:00:00Z",
  "phoneNumber": "",
  "tempLoginEnd": null,
  "title": "Tester"
}
```

For rest of this lab you will use Swagger and make the API requests in the same way with the *Try it out!* button for each API method.

**Continue to [Section 1](../s1_api_intro) »**


