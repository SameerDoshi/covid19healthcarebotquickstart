#COVID-19 Healthcare bot
#About
This script uses the [Microsoft healthcarebot framework](https://github.com/microsoft/HealthBot-WebChat) to quickly deploy a COVID19 triage bot.  This bto can be later customized.

Sample bot: https://healthbotcontainersamplee67f.azurewebsites.net

1. Go to portal and click + and create (subscribe) "Microsoft Healthcare Bot"
2. You'll get a link to a config portal via email (From "Microsoft Marketplace")- click on the link in your email
3. You'll need to name your bot. This name will display to users. Ex: Vivi
4. Chose the location for the bot. (ex: USA)
Elapsed time: 5 mins
4. Find covid-19 response template (there are other tempaltes here)
5. Click on that template and give it a  "Specify main scenario name" ex: SameerCOVID-19
6.  You'll see the visual designer. Click save. Read more below.
This view allows you to edit the scenario using either the visual interface or a javascript code editor.  You can get back to this at anypoint by coming back to the config portal and clicking manage.

7. If you're in the editor click the Exit button (on top right)
8. On the side menu pane click the button with two arrows collapsing labele integration.  Then click secrets
9.  Grab web chat secret and app secret.
10. Go to: https://github.com/microsoft/HealthBotContainerSample/tree/webchat_v4
11. Click Deploy to Azure
12. Fill in the form (chose sub, RG, location)
13. Once deployed go back to Azure Portal
14. Find and click on the app service it created.
15. Click App Configuration and under app settings you'll add two new settings.
16. Add "APP_SECRET" with value from above
17. Add "WEBCHAT_SECRET" with value from above
18. Click save to save changes.
19. Go to app service editor
20. Edit wwwroot\public\index.js
21. Replace contents of liens 55 and 72 with a space
22. You're almost done!
23. Change value on line 87 to "covid19" (or whatever the id of the scenario template)
24. Remove args lines
It should look like: 
~~~~
    botConnection.postActivity({
        type: "invoke",
        value: {
            trigger: "covid19",
        },
        from: user,
        name: "TriggerScenario"
    }).subscribe(function(id) {});
~~~~ 

25. Find website url on overview page of app service and Visit website.  (no restart required)
25a.  Notice the text entry box at the bottom of your app?  If you don't like that then please add the following lines to index.js on the next line after the code you edited on above.
~~~~
   var shellInput = document.querySelector(".wc-console.has-upload-button");
   shellInput.parentNode.removeChild(shellInput);
~~~~
Final block will ook like:
~~~
store.dispatch({
     type: 'DIRECT_LINE/POST_ACTIVITY',
     meta: {method: 'keyboard'},
     payload: {
        activity: {
           type: "invoke",
           name: "TriggerScenario",
           value: {
              trigger: "covid19",
            }
        }
     }
  });
  var shellInput = document.querySelector(".wc-console.has-upload-button");
  shellInput.parentNode.removeChild(shellInput);
~~~

26. Add the bot to your webstite using the steps [here](https://github.com/microsoft/HealthBot-WebChat)
27. Now customize the template

## Production Checklist
Before you go prod , some suggestions: 
1 Add resource locks to prod RGs
2 Crete dev environment and transition RBAC
3 Crete dev bot and transition editors to dev bot
4 Setup auto-scale rules
5 Setup monitoring and alerting rules
6 Enable backup
7 (Ideal) Setup TM profile infront of app service or scale set
8 Obtain SSL cert for TM CNAME

NOTES THIS CODE-SAMPLE IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A PARTICULAR PURPOSE. This sample is not supported under any Microsoft standard support program or service. The script is provided AS IS without warranty of any kind. Microsoft further disclaims all implied warranties including, without limitation, any implied warranties of merchantability or of fitness for a particular purpose. The entire risk arising out of the use or performance of the sample and documentation remains with you. In no event shall Microsoft, its authors, or anyone else involved in the creation, production, or delivery of the script be liable for any damages whatsoever (including, without limitation, damages for loss of business profits, business interruption, loss of business information, or other pecuniary loss) arising out of the use of or inability to use the sample or documentation, even if Microsoft has been advised of the possibility of such damages, rising out of the use of or inability to use the sample script, even if Microsoft has been advised of the possibility of such damages.

