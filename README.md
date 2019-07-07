# ServiceNow-WebexTeams Workshop


### **Overview**

ServiceNow will provide the user interface to gather the necessary information for your solution to accomplish the desired task(s).  Webex Teams will be the conduit to extract the information from ServiceNow and deliver it to your solution. 

A complete solution will need two Webex Teams Bots.  This workshop will  show you how to create the first Bot that sends the information from ServiceNow to Webex Teams.  The second Bot will take the information from Webex Teams and use it to interact with your solution.  The second Bot is not part of this workshop.

---

### **Table of Contents**

- [ServiceNow account and Developer instance](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#servicenow-account-and-developer-instance)
- [Add ServiceNow app from repo to your Dev instance](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#add-servicenow-app-from-repo-to-your-dev-instance)
- [Create Webex Teams Bot](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#create-webex-teams-bot)
- [Add Bot to a Webex Teams space](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#add-bot-to-a-webex-teams-space)
- [Get Webex Teams roomId](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#get-webex-teams-roomid)
- [Add Bot Access Token and roomId to ServiceNow app](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#add-bot-access-token-and-roomid-to-servicenow-app)
- [Validate](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#validate)
- [Modify ServiceNow Form to meet your requirements](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#modify-servicenow-form-to-meet-your-requirements)
- [Update Business Rule to match Form modifications](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#update-business-rule-to-match-form-modifications)
- [ServiceNow trouble shooting tips](https://github.com/pselker2/SNOW-WebexTeams-Infrastructure/#servicenow-trouble-shooting-tips)

---

### **ServiceNow account and Developer instance**

1. Create an account on [developer.servicenow.com](https://developer.servicenow.com)  
2. Log in and navigate to Manage > Instance
3. Request Instance
    - There are several versions (Choose Kingston if no preference)
4. Start Dev Instance by clicking on URL
    URL looks like:  https://dev12345.service-now.com/
4. Optional: Take the following courses: 
    (developer.servicenow.com menu: Learn > Training):
    - ServiceNow Basics
    - ServiceNow Studio
    - Build My First Application
    - Build the NeedIt Application
    - Scripting in ServiceNow
    
---

### **Add ServiceNow app from repo to your Dev instance**

1. git clone https://github.com/pselker2/SNOW-WebexTeams-Infrastructure.git to a public repo like GitHub to enable the ServiceNow Dev instance to access it
2. ServiceNow Navigator > Studio
    - A new browser tab opens with the Load Application window
    - Select Import From Source Control
    - Use the information from the **new** repo you cloned in step 1
    - In the left Studio pane you should see files associated with the Cisco-Infrastructure app
    - Studio > Source Control > Create Branch
        - The Branch will capture all your changes
        - Source Control > Commit Changes  to save changes to the new repo 

---

### **Create Webex Teams Bot**

1. Log in on [developer.webex.com](https://https://developer.webex.com) (Sign up if you need an account)
2. Click on the "Start Building Apps" button
3. Click on the "Create a New App" button
4. Click on "Create a Bot" button
5. Fill in the information and click the "Add Bot" button at the bottom of the page
6. Save the "Bot's Access Token" in a safe place

---

### **Add Bot to a Webex Teams space**

1. Create a space for testing the Bot you just created
2. Add your new Bot to the space
    - Click the circle with six dots in the upper right corner of the space
    - Click the People circle
    - Click the Add people circle
    - Type the Bot Username (example@webex.bot)
    
---

### **Get Webex Teams roomId**

1. Log in on [developer.webex.com](https://https://developer.webex.com)
2. Click on the "Documentation" 
3. Click on "API Reference" on the left side
4. Click on "Teams" on the left side
5. Click on the Get Method url with the Description List Teams
6. Click the yellow "Run" button on the right side
    - All the Teams you are in will be listed in the response section
    - Copy the "id" of the Team that contains the space with your Bot
7. Click on "Rooms" on the left side
8. Under "Method" click on the Get url to "List Rooms"
    - On the right under "Query Parameters"
        - Paste the "id" of the Team from step 6 into the "teamId" parameter
        - Click the yellow "Run" button on the right side
        - Copy the "id" of the Room that contains your Bot

---

### **Add Bot Access Token and roomId to ServiceNow app**

1. ServiceNow Studio browser tab
    - Click on "SN Bot for Webex Teams" in the left pane under "Business Rules"
    - Right pane click "Advanced" tab
    - Replace the roomId in line 22 of the script with your roomId you found in the previous section. (roomId in the cloned script looks like 'Y2lzY29...NDA4')
    - Click the "Update" button in upper right
2. ServiceNow Studio browser tab
    - Click on "Cisco Webex Integration" in the left pane under "REST Messages"
    - Click on the "HTTP Request" tab in the right pane
    - Click the "Authorization" link in the right pane
    - In the "value" box replace only the Bot access token that looks like OWQ0OD...e10f with the access token of the Bot you created in the "Create Webex Teams Bot" section
    - Click the "Update" button in the upper right 
    
---

### **Validate**

1. In the Navigator browser tab (not Studio) type "Cisco-Infrastructure"
    - You should see Cisco-Infrastructure, Create New, All and Open in the left pane
    - Click "Create New"
    - Fill in the needed information
    - Click "Submit" button in upper right
    - You should see that a record was created in the right pane
    - You should also have a message in your Webex Teams space from the BOT in ServiceNow that provides the information you just entered in the ServiceNow form
    
---

### **Modify ServiceNow form to meet your requirements**

1. In the Studio browser tab click on "Cisco-Infrastructure Table[Default view]" under "Forms" in the left pane
2. Modify the form in right pane to accomodate your solution
    - If you need help modifying the form, see the "Working with Fields" exercise in the "Building the NeedIt Application" training module.  The module can be found by starting on the Developer main menu > Learn > Training.
    
---

### **Update Business Rule to match Form modifications**

1. If you added, changed or deleted fields from the form in the previous section you will need to modify the script in the "Business Rule" to match the changes
    - For example:  If Phone number is deleted and replace with VLAN.
        - the "Name" will change from "u_dn_phone_number" to "u_vlan".
2. In Studio browser tab click on "SN Bot for Webex Teams" in the left pane.
    - Click on the "Advanced" tab in the right pane
    - Update line 13 in the script
        - From:  
        ```javascript
        dn = current.u_dn_phone_number;
        ```
        - To:
        ```javascript
        vlan = current.u_vlan;
        ```
    - Update line 15 
        - From:  
        ```javascript
        var phoneinfo = "<@all> update-phone userid: " + userid + " phone type: " + phonetype + " phone location: " + phonelocation + " directory number: " + dn;
        ```
        - To:
        ```javascript
        var phoneinfo = "<@all> update-phone userid: " + userid + " phone type: " + phonetype + " phone location: " + phonelocation + " vlan: " + vlan;
        ```
        - Click the "Update" button in the upper right.
3. Test the changes by creating a new record

---

### **ServiceNow trouble shooting tips**

1. In the ServiceNow Navigator
    - Find/Search window 
        - Type System Log
            - View Script Log Statements
            - View Application Logs
