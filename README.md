## HF Server Events Setup Script

The main idea of this project is to help companies that don’t want to expend too much money on Centralized Logs solutions. Most of this can be accomplished using default tools in Windows. 

In this project I’m using Windows Server and SQL Server only.

#### After you join a Windows Server to domain and Install SQL Server. Run the script and it will:

 - Configure and enable WinRM and Event Collector Service
 - Create the Event Forward Subscription - following the Microsoft's Best Practices (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor)
 - Configure all the Domain Controllers to forward the events to this server
 - Increase the maximum size of the Forwarded Events to 4 GB (to support a large number of events until the sync task put the events in the database)
 - Create a local group named: "HF Event Report Viewer"
 - Create the SQL Server database and tables
 - Configure the SQL Server's Full Text Search
 - Configure a Scheduled Task to Synchronize the Forwarded Events with the SQL Server Database (hourly)
 - Configure the Reporting Services 
 - Create and import the Reporting Services Reports
 - Configure the Reporting Services Permissions (to give permissions to more users just add them to the Windows "HF Event Report Viewer" local group)

#### This script may affect your environment!

Even there is no harm in the script itself. It will change a few things in your Domain Controller's environment. It will:

- Add a registry key in all your Domain Controllers (to configure the Centralized Event Server)
- Configure WinRM in all your Domain Controllers (this is a default pre-requisite to Event Forwarders to work)
- Configure Event Forward in all your Domain Controllers (this will enable and start the default “Windows Event Collector” service)
- Adds the account "NETWORK SERVICE" the Domain Group "Log Event Readers"

### How to run:

Just copy or download the HFEventServer.ps1 and run on any Windows Server computer that meets the requirements below.

### Requirements:

The script must be run with the following requirements:

 - The script was tested in Windows Server 2012 and Windows Server 2019
 - The SQL Server 2014 was the only version tested (you must also install “Reporting Services” and “FullText Search”. This is detailed  in the screenshots)
 - The Account running the script must have rights to connect remotelly and create registry keys on the Domain Controllers
 - Connectivity trough port TCP 5985 (Default Event Forwarder Port) with the Domain Controllers
 - Internet connection is not required*

## Screenshots:

The final result will be the Web Reports created in the Reporting Services:

![alt text](https://github.com/ClaudioMerola/HFServerEvents/raw/master/Docs/ReportWorking.png)

### Installation.

#### .NET Framework 3.5 Feature 

This is required by the SQL Server

![alt text](https://github.com/ClaudioMerola/HFServerEvents/raw/master/Docs/DOTNET.png)


#### Important Details in the SQL Server Installation.

There is not many configuration required by the SQL Server installation besides the ones listed bellow

### The Following are required components (Red):
 - Database Engine Services
 - Full-Text and Semantic Extractions for Search
 - Reporting Services - Native
 
### The Following are recommended components (Blue):
  - Management Tools - Complete

![alt text](https://github.com/ClaudioMerola/HFServerEvents/raw/master/Docs/DB.png)


### Reporting Services Installation and Configuration:

Just use the default "Install and configure"

![alt text](https://github.com/ClaudioMerola/HFServerEvents/raw/master/Docs/ReportingSetup.png)


### After the SQL Server Installation, restart the server.

### After the restart, just run the HFEventServer.ps1. 

if no errors occur, the following should have been configured automatically in the local server:



![alt text](https://github.com/ClaudioMerola/HFServerEvents/raw/master/Docs/ReportWorking.png)


