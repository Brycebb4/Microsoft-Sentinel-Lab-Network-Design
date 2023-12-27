# Microsoft-Sentinel-Lab-Network-Design
Microsoft Sentinel Lab Network Desgin
![image](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/f891f06a-9a88-430d-86b9-bc0b369d54bb)
IN THIS LAB WE WILL GO OVER THE FOLLOWING

Configure and Deploy Azure Resources such as Log Analytics Workspace, Virtual Machines, and Azure Sentinel.
Implement Network and Virtual Machine Security Best Practices.
Utilize Data Connectors to bring data into Sentinel for Analysis.
Understand Windows Security Event logs.
Configure Windows Security Policy Settings.
Utilize KQL to query Logs.
Write Custom Analytic Rules to detect Microsoft Security Events.
Utilizing MITRE ATT&CK to map adversary tactics, techniques, detection, and mitigation procedures

STEP 1 : Hence you have a Azure account. Create your Resource Group and Virtual Machines. Once the Virtual Machine is created hit "Start" to run VM.
![Screen Shot 2023-12-27 at 1 33 53 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/ecaed663-28d6-454b-9969-f032c8075360)
![Screen Shot 2023-12-27 at 1 36 09 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/0e514ceb-0a3f-462c-a58b-dc56d2ad7744)

When you deploy a virtual machine in Azure, that virtual machine is placed on a Virtual Network (vnet). Your Virtual Machine is assigned an IP address on that network as well as a network interface. Another Azure security feature that is implemented with the default settings we used are Network Security Groups (NSG). A NSG is used to filter network traffic to and from Azure resources. Similar to a firewall, filtering is based on rules that dictate source des andtination ports as well as the network protocols that are allowed or denied.

If we go to back to our resource group we created earlier we can see the Virtual Network and NSG listed as resources.
![Screen Shot 2023-12-27 at 1 39 43 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/a96d5fcc-d3f4-4395-821e-f7d1fdb0ee73)

Looking into our NSG we can see the default rules. Here you will enable "Allow selected ports" This will help protect the VM since anyone who obtains our public IP (which can be possibly be obtained via network scan) can potentially connect to the VM as this is public-facing. This presents a security risk as it makes the VM vulnerable to possibly a brute force or password spray attack. We see that the source tab has been identified to only my IP address.


Search "Microsoft Defender for Cloud" in the search bar at the top of the Azure Portal and select the service. You will see a page similar to this. On the left pane select “environment settings”

![Screen Shot 2023-12-27 at 1 49 42 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/586bc91d-d8ba-443c-8612-228b569b2b2a)


Select your Azure Subscription from the list provided. Upon selection, the following services will be seen on the screen. By default, Enhanced security is off but you will want to select the Enable All Microsoft Defender for Cloud Plans. You will be given a 30 free trial so be sure to disable when finished with the lab to avoid any cost. You can select then “Enable all option” and hit save.

select subscription for Defender for cloud

Enable All Microsoft Defender for Cloud Plans

After enabling the plan, navigate back to the homepage for Defender for Cloud and select 'Workload Protections' on the left pane. That will then present the following screen
![Screen Shot 2023-12-27 at 1 49 03 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/a974dd82-2738-42f9-bd56-e02b4dcf329b)

Now go back to VM and Connect the configuration page and Enable Just-in-time button which will allow only your IP to access the VM 
![Screen Shot 2023-12-27 at 1 44 59 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/2c8f1861-d803-432d-bb15-c74b54b68490)

Also you can see the RDP traffic is alllowed for a certain amount of time only from the IP of my computer. Anyone else trying to establish and RDP connection will be blocked due to the rule. 
![Screen Shot 2023-12-27 at 1 53 08 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/0af193c0-ab94-41a9-9f28-b201c4839644)

CREATING MICROSOFT SENTINEL 

Search "Microsoft Sentinel" and hit create
![Screen Shot 2023-12-27 at 1 57 07 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/d43cac25-9505-4dbe-b9a0-d6e4a0d1a9e8)

Go to the Data connectors Tab. This is where we can select the type of data that we want to bring into our SIEM.

 Data Connector
![Screen Shot 2023-12-27 at 1 58 20 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/90537654-b619-411e-a8a4-61006a75e447)

In the Search bar type in “Windows” and you will see Windows Security Events via AMA. Select that option and click “Open Connector Page”.

 Open Connector Page

Click “Create data collection rule.”

Create data collection rule. This here is my "Data Collection Rule" Refresh this page and you will see that the "Connected" status will show. 
![Screen Shot 2023-12-27 at 2 07 01 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/ac7e61ec-a472-4e8f-ad5f-75ca5e290947)

GENERATING SECURITY EVENTS
Click “Start” at the top page to turn on the VM if its not running already . Enable Just in time Access if necessary.

Start Virtual Machine
![Screen Shot 2023-12-27 at 2 00 32 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/faa02ec5-4c07-4f89-a823-a8c251bbf731)

Under Networking, you are given a public IP. Use an RDP on your PC Client such as Remote Desktop Connection to access your VM by entering in the public IP address.

Note: You might need to refresh after starting the virtual machine to have the public IP show up.

As you can see there are several types of logs Windows Collects:
Application logs, Security Logs, Setup, System, and Forwarded Events.

Event Viewer Search
![Screen Shot 2023-12-27 at 12 46 53 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/33bfe326-fff4-4c25-ab39-1f4ce8db43f9)

Event Viewer

Our focus in this lab will be on Windows Security events.

Click “Security” and observe the events.
![Screen Shot 2023-12-27 at 2 13 27 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/bb9766bf-38e8-4f63-829e-efaadd5b35c1)

As you can see there are several security events in event viewer. Let’s drill into one of these events.

Use the find option and search for 4624

Find Event 4624
![Screen Shot 2023-12-27 at 2 13 27 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/16d01ca5-abca-490e-ae54-c769aecf4cd1)

When we select event 4624 we see that 4624 ID is indicative of a successful logon. We can also examine more detailed information about the logon if need be.

4624 Log Details

Part 4: Kusto Query Language

The purpose of a SIEM such as Azure Sentinel would be to bring data like this into a centralized location. In an enterprise, we would want data coming from all our endpoints and virtual machines to make it easier for an analyst to get the information that is needed quickly.
Let’s go back to Azure Sentinel and pull this event from our Sentinel Logs.

In the Sentinel, Main Page click “Logs”

Select Logs from Left Pane

Logs should bring up this page.

Log page

In the section where it says “Type your query here or click one of the queries to start” we are going to use the following KQL (Kusto Query Language) Logic:

SecurityEvent
| where EventID == 4624
| project TimeGenerated, Computer AccountName
Every SIEM has a search language that makes it simple to extract data from Logs. In Sentinel, that language is called KQL or Kusto Query Language. While there are many different syntax rules and ways to construct queries in KQL we will be using a few basic KQL commands to extract the data and write our analytics rule later in the lab.
![Screen Shot 2023-12-27 at 2 20 31 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/740115ee-6ae6-4eff-995f-37bc0500138b)

Let’s break down the meaning of this query

Security Event refers to the event table we are pulling the data from. All the events we observed in the event viewer are stored there.

Where command filters on a specific category. In this case, we only want events that correspond to successful logins.

Project command will specify what data to display when the query is run so, in this specific scenario, we want to only see the time the logon event occurred, what computer it came from and what account on this computer-generated the event.

When the query is run we get this result.

After running KQL request

As you can see, we have a list of all the times we have had a successful login on our VM. However, as you can see the Account Name field is empty and sentinel is not automatically putting that data into that field. We will go over how to populate that field later in the lab when we create our analytic rule.

Part 5: Writing Analytic Rule and Generating Scheduled Task

We can have the option to be alerted to certain events by setting up analytic rules. The Analytic rule will check our VM for the activity that matches the rule logic and generate an alert any time that activity is observed. There will be so some details provided in the alert that can help an analyst start their investigation into determining whether the event in the alert is a false positive or true positive.
Aanlytics

These are a list of alerts that we can enable our SIEM to monitor that come out of the box. If you wish, you can expand some and see what they are comprised of by clicking create a rule and following the onscreen tasks to see the rule logic and as well as enabling the rule. However, the rules will only fire if the logic is met by a security event on your VM.

Scheduled Task and Persistence Techniques
The final part of this lab is to create our own custom rule to detect potentially malicious activity on our VM. In Windows Task scheduler you have the option to create a scheduled task. A scheduled task is essentially a way to automate certain activities on your machine .

For instance, you could set up a scheduled task that opens google chrome at a certain time every day. While many times scheduled task can be a harmless event this can also be used as a persistence technique for malicious actors.

According to the MITRE Attack Framework, “Adversaries may abuse task scheduling functionality to facilitate initial or recurring execution of malicious code. Utilities exist within all major operating systems to schedule programs or scripts to be executed at a specified date and time”.

In this lab, our scheduled task will not be associated with any malicious activity as we will set up a scheduled task that opens Internet Explorer at a certain time but we will create an analytic rule to monitor for that specific action so we can be alerted to the to the activity in our SIEM in order to simulate the scenario.

The Windows Security Event ID that corresponds to scheduled task creation is 4698. However, these events are not logged by default in the Windows event viewer. To enable logging for this event we need to make some changes to the Windows Security policy in our VM.

Search for “Local Security Policy” in Windows 10 VM and expand “Advanced Audit Policy Configuration”

Advanced Policy Configuration

Expand “System Audit Policies” and select "Object Access”. Then select the “Audit Other Object Access Event”

Audit Other Object Access

Select to enable Configure the following event: “Success” and “Failure”

Enable Success & Failure
![Screen Shot 2023-12-27 at 2 21 35 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/455478b6-92e2-4818-9454-7d9d70757d5e)

Logging is now enabled for the scheduled task event.

Creating our scheduled task

To detect a scheduled task creation, we need to generate some activity in our VM.

Open Windows Task Scheduler and navigate to “Create Task” . Add a name and change the “Configure For” Operating system to Windows 10.

Create Task
![Screen Shot 2023-12-27 at 2 25 01 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/a07253fd-91ba-4861-b80f-9b4d5e9b52a1)


Writing Analytic Rule

Lastly, we need to write some KQL logic to alert us when a scheduled task is created.

Go to the sentinel Home Page and click “Analytics Rules” and click create at the top of the page and select the scheduled query option.
![Screen Shot 2023-12-27 at 2 28 44 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/27beaa64-cfab-4ce3-9188-acee4fad8480)

Sentinel Created Rules for Scheduled Query

We will add the tactic and techniques of "Persistence and sub-category of T1053 - Scheduled task/job". This will give you the 2 selected.

Sentinel Tactics and Techniques

Here we are simply providing some information about the alert to the analyst.

Next, we will come up with the alert logic that causes our alert to fire.

Most of the logic will be like the KQL Query that we created earlier for logon event.

 Security Event
| where EventID == 4698
This query will pull instances of scheduled task creation as shown here in our logs.

KQL Query 4698 ![Screen Shot 2023-12-27 at 2 16 10 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/4491d72a-1838-400f-90ac-ebd80abe0424)


Expand the log for the scheduled task you created in the previous step and look at the Event Data category.

You should see something like this:

Expand the Event Data

There is a lot of useful data in here such as the name of the scheduled task, the Task Name field, the ClientProcessID, the username of the account that created the scheduled task amongst other info.

However, if you use the project command to display these data fields as columns when you run the query, we need to add this to our logic.

SecurityEvent                             
| where EventID == 4698
| parse EventData with * '<Data Name="SubjectUserName">' User '</Data>' *
| parse EventData with * '<Data Name="TaskName">' NameofSceuduledTask '</Data>' *
| parse EventData with * '<Data Name="ClientProcessId">' ClientProcessID '</Data>' *
| project Computer, TimeGenerated, ClientProcessID, NameofSceuduledTask, User
The Parse command will allow us to extract data from the Event Data Field that we find important.

This extracted the SubjectUserName , TaskName, ClientProcessID (Computer automatically displays) .

The above logic allows me to assign those two new categories such as User, NameofScheduledTask, and ClientProcessID respectively. When we project our new fields, the output is the following:

NameofScheduledTask, and ClientProcessID

As you can see we’re able to generate Event Data and place it into its own category for readability.

Copy and Paste the above query into the editor under the “Set Rule Logic” tab

Set Rule Logic
![Screen Shot 2023-12-27 at 2 31 58 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/84328062-9763-4043-bc95-55b8ef586886)

The Alert Enrichment section is below this. Enriching an alert simply is the process of adding context to the alert to make it easier for the analyst to investigate.

As opposed to having to query this data as we did in the analytic rule Alert Enrichment allows us to put the necessary data into the alert details as Entities so the analyst can begin investigating those specific components.

Use the following settings for entity mapping.

Entity Mapping
![Screen Shot 2023-12-27 at 2 31 58 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/31d7706b-1b48-47f1-92c1-42cdb3bdc05f)

The only other setting that need to be changed for the purposes of this lab is how often the query is run and when to look up the data. Change defaults to the following:

Query scheduling
Run query every
5								Minutes 
Lookup data from the last
5 								Minutes
How often query is run

Incident Settings and Automated Response are not necessary to alter for this lab so you can go ahead to “review and create” to make the Analytic Rule.

Event Grouping for each event

Confirm the existing schedule rule 

Once you create your analytic rule the final step is to create another scheduled task in your Windows VM and wait for the alert to trigger in Sentinel
![Screen Shot 2023-12-27 at 2 35 27 PM](https://github.com/Brycebb4/Microsoft-Sentinel-Lab-Network-Design/assets/148110535/89625eb8-d2d2-4e88-8ab4-76e923490cf6)

Note: This might take up to 10 minutes. Refresh the Incidents page periodically until the Alert triggers. When the alert fires this is what you should see.

Second Scheduled Task

On the right pane we see all the necessary information we would need to begin investigating the alert such as the host machine, user account, process ID of the task, and the name of the scheduled task.

While in this case, the scheduled task is non-malicious, in the event it was, from here an analyst would investigate the entities such as the user account, the scheduled task, host, etc. with other tools such as an EDR solution and other security tools to decide if this is a false positive or true positive.



