Section 5 – Tips and Tricks
====

**Go Back to [Section 4](../s4_trended_data)**

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

On a Mac:

![s5_open_dev_tools](../../images/s5_open_dev_tools.png?raw=true)

On a PC:

![s5_open_dev_tools_pc](../../images/s5_open_dev_tools_pc.png?raw=true)

5. Select the **Console** tab and enter `adobe.tools.debug.includeOberonXml = true` into the console and press Enter.

![s1_debug_text](../../images/s1_debug_text.png?raw=true)

6. Refresh the page

7. Notice that you now have a debugger icon on every panel!

8. Locate the **Product Performance Grid** panel in the Analysis Workspace project and click on the debugger icon, then click on **Freeform Table**

9. Notice that there are possibly multiple requests per panel
![s1_debug_link](../../images/s1_debug_link.png?raw=true)

10. Click on one of the requests

11. Copy the text from the **JSON REQUEST** box by either manually selecting the text or using the handy **Copy to Clipboard** button
![s1_copy_json](../../images/s1_copy_json.png?raw=true)

12. Click the **Try it out!** button

12. Paste the JSON into the **`/reports`** endpoint in the Swagger interface

14. Click the **Execute**

15. Do your results match Analysis Workspace?

16. Open the browser's developer tools again

On a Mac:

![s5_open_dev_tools](../../images/s5_open_dev_tools.png?raw=true)

On a PC:

![s5_open_dev_tools_pc](../../images/s5_open_dev_tools_pc.png?raw=true)

17. Select the **Console** tab and enter `adobe.tools.debug.includeOberonXml = false` into the console and press Enter.

18. Refresh the page. The debug icon should be gone.

Section 5 Complete
-----
Congratulations! You have completed Section 5 and should know how to use the Analysis Workspace debugger to help you format report requests.

Lab Complete
-----
You have completed the lab! 

If you have additional time, you can try some of the Extra Credit Challenges in the previous sections.

**Go Back to [Section 4](../s4_trended_data)**

