Section 0 - Getting Started
====

Objectives
----
* Log in and get to know analysis workspace
* Learn how to authenticate API calls in the lab

Experience Cloud Accounts
----
The new Analytics APIs require users and organizations to be configured for login via the Experience Cloud at https://experiencecloud.adobe.com rather than the traditional Analytics login page at my.omniture.com.

In this lab you will be using an Experience Cloud organization and Analytics login company that is already configured for Experience Cloud Authentication. 

**Let's sign in!**
Go to https://experiencecloud.adobe.com and select **Sign In with an Adobe ID**
![s0_exp_cloud_login](../../images/s0_exp_cloud_login.png?raw=true)

The username will be of the following format:

`apilab18+<User ID>@adobetest.com`

where the `<User ID >` section is replaced by your lab computer number.  For example, apilab18+37@adobetest.com

Enter your assigned username and the password provided to you and click **Sign In**
![s0_exp_cloud_login2](../../images/s0_exp_cloud_login2.png?raw=true)

You should be taken directly to the Analysis Workspace landing page in the **Analytics API Summit 2018** login company:
![s0_workspace_landing_page](../../images/s0_workspace_landing_page.png?raw=true)

You Are now Ready to work in analysis workspace


API Authentication
----


Accessing the Swagger Interface
----
We will be using a Swagger interface to generate API requests in this lab. To access Swagger, in a new tab or window in your web browser navigate to [Analytics Swagger](https://adobedocs.github.io/analytics-2.0-apis/)

To make an API request in Swagger, you will need to make sure you are authenticated to provide an access token on each request

### Authenticating through Swagger

**Let's sign in!**
To authenticate through swagger click on the login button at the top of the page. 
![s0_swagger_ui](../../images/s0_swagger_ui.png?raw=true)

Go to https://experiencecloud.adobe.com and select **Sign In with an Adobe ID**
![s0_exp_cloud_login](../../images/s0_exp_cloud_login.png?raw=true)

The username will be of the following format:

`apilab18+<User ID>@adobetest.com`

where the `<User ID >` section is replaced by your lab computer number.  For example, apilab18+37@adobetest.com

Enter your assigned username and the password provided to you and click **Sign In**
![s0_exp_cloud_login2](../../images/s0_exp_cloud_login2.png?raw=true)


Validate API Connectivity
----
Let's make sure everything works correctly. Click on the **Users** section in the Swagger interface and then select **GET /users/me**, then click **Try it out!**:

![s0_swagger_users_me](../../images/s0_swagger_users_me_1.png?raw=true)

Then click **Execute**

![s0_swagger_users_me2](../../images/s0_swagger_users_me_2.png?raw=true)


If you have authenticated correctly you should get a JSON response from the /users/me endpoint with information about your current user:

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

For rest of this lab you will use Swagger and make the API requests in the same way with the **Try it out!** and **Execute** buttons for each API method.

**Continue to [Section 1](../s1_api_intro) »**


