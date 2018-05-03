Section 0 - Getting Started
====

Objectives
----
* Learn how to authenticate API calls in the lab

Experience Cloud Accounts
----
The new Analytics APIs require users and organizations to be configured for login via the Experience Cloud at https://marketing.adobe.com rather than the traditional Analytics login page at my.omniture.com.

In this lab you will be using an Experience Cloud organization and Analytics login company that is already configured for Experience Cloud Authentication. 

**Let's sign in!**
Go to https://marketing.adobe.com and select **Sign In with an Adobe ID**
![s0_exp_cloud_login](../../images/s0_exp_cloud_login.png?raw=true)

The username will be of the following format:

`apilab18+<User ID>@adobetest.com`

where the `<User ID >` section is replaced by your lab computer number.  For example, apilab18+37@adobetest.com

Enter your assigned username and the password provided to you and click **Sign In**
![s0_exp_cloud_login2](../../images/s0_exp_cloud_login2.png?raw=true)

You should be taken directly to the Analysis Workspace landing page in the **Analytics API Summit 2018** login company:
![s0_workspace_landing_page](../../images/s0_workspace_landing_page.png?raw=true)

Authentication
----
### Getting the Lab Access Token

Make sure you're logged into Analytics, then select the **“Access Token”** bookmark from the browser’s bookmark bar:
![s0_access_token_bookmark](../../images/s0_access_token_bookmark.png?raw=true)

This will automatically copy an access token to your clipboard.

<a href='javascript:var text = "";if(typeof(OM) != "undefined"){ text = OM.Config.appConfig.appService.token;} else { text = adobe.analytics.appConfig.appService.token;} var textArea = document.createElement("textarea"); textArea.style.position = 'fixed'; textArea.style.top = 0; textArea.style.left = 0; textArea.style.width = '2em'; textArea.style.height = '2em'; textArea.style.padding = 0; textArea.style.border = 'none'; textArea.style.outline = 'none'; textArea.style.boxShadow = 'none'; textArea.style.background = 'transparent'; textArea.value = text; document.body.appendChild(textArea); textArea.select(); try { var successful = document.execCommand('copy'); var msg = successful ? 'successful' : 'unsuccessful'; console.log('Copying text command was ' + msg); } catch (err) { console.log('Oops, unable to copy'); } document.body.removeChild(textArea);'>Access Token</a>

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

For rest of this lab you will use Swagger and make the API requests in the same way with the **Try it out!** button for each API method.

**Continue to [Section 1](../s1_api_intro) »**


