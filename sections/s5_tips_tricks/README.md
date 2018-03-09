Section 5 – Tips and Tricks
====

Objectives
----
*    Learn how to run report requests from the Analysis Workspace UI in the API

The new Analytics V2 API powers the Analysis Workspace project. Analysis Workspace includes a debugger that will let you view the different API requests it makes. This debugger is very helpful in learning how to craft report requests and exploring the full power of the new Analytics API.

Running a report from Analysis Workspace
-----
1. Log into Analysis Workspace as described in [Section 0](../s0_getting_started)
![s1_landing_page](../../images/s1_landing_page.png?raw=true)

2. Click on the **Create New Project** button to create a new project
![s1_create_new_project](../../images/s1_create_new_project.png?raw=true)

3. Scroll down and select the **Products** template
![s1_retail_template](../../images/s1_retail_template.png?raw=true)

4. Open the browser's developer tools 
![s1_open_dev_tools](../../images/s1_open_dev_tools.png?raw=true)

5. Enter `adobe.tools.debug.includeOberonXml = true` into the console and press Enter.
![s1_debug_text](../../images/s1_debug_text.png?raw=true)

6. Refresh the page

7. Notice that you now have a debugger icon on every panel!

8. Locate the **Product Performance Grid** panel in the Analysis Workspace project and click on the debugger icon, then click on **Freeform Table**

9. Notice that there are possibly multiple requests per panel
![s1_debug_link](../../images/s1_debug_link.png?raw=true)

10. Click on one of the requests

11. Copy the text from the **JSON REQUEST** box by either manually selecting the text or using the handy **Copy to Clipboard** button
![s1_copy_json](../../images/s1_copy_json.png?raw=true)

12. Paste the JSON into the /reports/ranked endpoint in the Swagger interface

13. Click the run button 

You can now see the data results that were used to create that panel. There is also a curl command listed in the debugger that you can examine to see which API method the Analysis Workspace UI called to render a given panel. When the new API is officially released, you will have access to most of the API methods used by Analysis Workspace, but not all. Some methods such as session keep-alive methods and other methods only make sense in the context of the Analysis Workspace product itself.


