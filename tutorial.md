#Configure SQL Firewall Rules Automatically#


-------------------------------------------------------

### Overview ###

This tutorial is mainly about how to configure firewall rules automatically for your SQL Azure database. It aims at introducing how to retrieve, add and delete firewall rules via using Windows Azure Cmdlets and also showing the interactive relationship between both Windows Azure Cmdlets and SQL Azure portal.
### Goals ###

This tutorial will help to achieve four goals as below:

1. To create firewall rules via SQL Azure Portal.

1. To retrieve firewall rules by using Windows Azure Cmdlets. 

1. To add firewall rules by using Windows Azure Cmdlets.

1. To delete firewall though Cmdlets.

### Key Technologies ###

* [SQL Azure Firewall](http://msdn.microsoft.com/en-us/library/ee621782.aspx)

### Setup and Configuration ###

SQL Azure database is a must for this tutorial and please refer to the Introduction of SQL Azure lab to set it up.

> **Note:** Learn more about SQL Azure Firewall

> **SQL Azure Firewall:** 

> [http://msdn.microsoft.com/en-us/library/ee621782.aspx](http://msdn.microsoft.com/en-us/library/ee621782.aspx)

> **How to configure the SQL Azure Firewall**:

> [http://msdn.microsoft.com/en-us/library/ee621783.aspx](http://msdn.microsoft.com/en-us/library/ee621783.aspx) 


### Tutorial ###

The tutorial includes following four sections:

1. [Create Firewall Rules via SQL Azure Portal](#segment1).

1. [Retrieve Firewall Rules by Using Windows Azure Cmdlets](#segment2).

1. [Add Firewall Rules by Using Windows Azure Cmdlets](#segment3).

1. [Delete Firewall Rules through Cmdlets](#segment4).

<a name="segment1" />
###Create Firewall Rules via SQL Azure Portal###
Log on SQL Azure portal [https://windows.azure.com](https://windows.azure.com/) and enter your Windows Live account.

![Log on Azure Services Portal](./images/Log-on-Azure-Services-Portal.png?raw=true "Log on Azure Services Portal")

_Log on Azure Services Portal_

Select the **Database** option on Windows Azure Management portal UI. And then, **SQL Azure Servers** will appear in the left pane. Please select the SQL Azure Server which you want to work with. At last, the **Server Information** will be presented in the center pane.

![Present SQL Azure Server on SQL Azure portal](./images/Present-SQL-Azure-Server-on-SQL-Azure-portal.png?raw=true "Present SQL Azure Server on SQL Azure portal")

_Present SQL Azure Server on SQL Azure portal_

Click **Firewall Rules** button and add an exception to the firewall.

![Add an exception to the firewall](./images/Add-an-exception-to-the-firewall.png?raw=true "Add an exception to the firewall")

_Add an exception to the firewall_

On the **Add Firewall Rule** dialog window, input "**Allowed Host 1**" in the blank space which is at the back of **Rule name**. After that, copy your current IP address into above two blank spaces after**IP Range start** and**IP Range end** respectively.

![Add an exception to the firewall](./images/Add-an-exception-to-the-firewall.png?raw=true "Add an exception to the firewall")

_Add an exception to the firewall_

> **Note:** You can specify an IP range or just a single IP address by entering the same value in both fields. By specifying your IP address, it allows you to connect programmatically to this SQL Azure project later in the lab.

Add the following values repeatedly.

| **Name** | **Start IP Address** | **End IP Address** |
| --- | --- | --- |
| Allowed Range 1 | 192.168.0.0 | 192.168.0.255 |
| Allowed Range 2 | 192.168.1.0 | 192.168.1.255 |

> **Note:** These IP addresses are just for demonstration purposes. You would retrieve these firewall rules programmatically later. They don't have real effect on your SQL Azure server as they are private IP addresses.


After configuring the rules, the firewall settings would be displayed in **Server Information**.

![Firewall settings](./images/Figure-5.png?raw=true "Figure 5")

 _Firewall settings_

<a name="segment2" />
###Retrieve Firewall Rules by Using Windows Azure Cmdlets###
SQL Azure allows you to retrieve firewall rules by using_[Windows Azure PowerShell Cmdlets](http://wappowershell.codeplex.com/)._ Open a **Windows PowerShell** command prompt. On **Start** menu, open **All Programs | Accessories | Windows PowerShell**.

In order to obtain the **Firewall Rules**, you need to add support for **Azure services Management Cmdlets** in the **Powershell** console. To do this, enter the command '**Add-PSSnapin WAPPSCmdlets**' in the console.

Now you will get the current **Firewall Rules** what your SQL Azure Server has. To do this, in the **PowerShell** console, enter the command '**Get-SqlAzureFirewallRules [YOUR-SERVER-NAME] -SubscriptionId [YOUR-SUBSCRIPTION-ID] -Certificate (Get-Item cert:\\LocalMachine\My\[YOUR-CERTIFICATE-THUMBPRINT]) | Format-Table'** in the console. Remember to update the placeholders with your account information.

> **Note:** With the **Get-SqlAzurefirewallRules** command, you can list all of current Firewall Rules a specific SQL Azure Server has. There are some parameters we used:

> **Server Name:** the SQL Azure Server name (without trailing database.windows.net).

> **Subscription Id:** Windows Azure subscription Id.

> **Certificate Thumbprint:** the thumbprint of Management Certificate uploaded to Windows Azure. For more information about this Cmdlet, you could run this line in the PowerShell Console:

> **Get-Help Get-SqlAzureFirewallRules -full**


In **PowerShell** window, the following table will present the **Firewall Rules** created by you.

![Obtain Firewall Rules](./images/Obtain-Firewall-Rules.png?raw=true "Obtain Firewall Rules")

_Obtain Firewall Rules_

<a name="segment3">
###Add Firewall Rules by Using Windows Azure Cmdlets###
Previously, you have defined several firewall rules via the SQL Azure portal. You can also create an additional firewall rule by using Windows Azure Cmdlets.

Create a new **Firewall Rule** in your **SQL Azure Server** for the IP address range "**10.0.0.0**" to "**10.0.0.255**". To do this, in the **PowerShell** console, enter the command '**New-SqlAzureFirewallRule -ServerName [YOUR-SERVERNAME] -RuleName "IP Example 1" -StartIpAddress "10.0.0.0" -EndIpAddress "10.0.0.255" -SubscriptionId [YOUR-SUBSCRIPTION-ID] -Certificate (Get-Item cert:\\LocalMachine\My\[YOUR-CERT-THUMBPRINT]) | Format-Table**' in the console. Please remember to update the placeholders with your account information.

Please refer to the below message:

![Create a new Firewall Rules](./images/Create-a-new-Firewall-Rules.png?raw=true "Create a new Firewall Rules")

_Create a new Firewall Rules_
In order to verify that if the new firewall rule has already been created,**please retrieve Firewall Rules by using Windows Azure Cmdlets**.
![Verify the new Firewall Rules](./images/Verify-the-new-Firewall-Rules.png?raw=true "Verify the new Firewall Rules")

_Verify the new Firewall Rules_

You could also confirm that the firewall rule has already been added successfully via **SQL Azure** portal. If the rule is not displayed, please do refresh.
![Updated firewall settings is presented on SQL Azure portal UI](./images/Updated-firewall-settings-is-presented-on-SQL-Azure-portal-UI.png?raw=true "Updated firewall settings is presented on SQL Azure portal UI")

_Updated firewall settings is presented on SQL Azure portal UI_

<a name="segment4" />
###Delete Firewall Rules through Cmdlets###
SQL Azure server firewall rules can be deleted via SQL Azure Portal.
Delete an existing firewall rule in your **SQL Azure Server**. To do this, in the **PowerShell** console, enter the command '**Remove-SqlAzureFirewallRule -ServerName [YOUR-SERVERNAME] -RuleName "IP Example 1" -SubscriptionId [YOUR-SUBSCRIPTION-ID] -Certificate (Get-Item cert:\\LocalMachine\My\[YOUR-CERT-THUMBPRINT]) | Format-Table'** in the console. 
Refer to the **PowerShell** window, firewall rule has been removed.
![Delete Firewall Rules](./images/Delete-Firewall-Rules.png?raw=true "Delete Firewall Rules")

_Delete Firewall Rules_

**Retrieve Firewall Rules by using Windows Azure Cmdlets**, in order to verify that the "**IP Example 1**" rule you created in the previous task is no longer in the list of Firewall Rules.
![Retrieve Firewall Rules](./images/Retrieve-Firewall-Rules.png?raw=true "Retrieve Firewall Rules")

_Retrieve Firewall Rules_

You can also confirm that the firewall rule was deleted correctly via the SQL Azure portal. If the rule is displayed, please do refresh.

![Delete IP Example 1 rule on SQL Azure Portal ](./images/Delete-IP-Example-1-rule-on-SQL-Azure-Portal-.png?raw=true "Delete IP Example 1 rule on SQL Azure Portal ")

_Delete IP Example 1 rule on SQL Azure Portal_ 

### Summary ###

This tutorial describes how to configure SQL Firewall rules automatically. Above firewall rules help users to specify an allowed list of IP addresses which could make you access SQL Azure Server. Users could retrieve, add and delete firewall rules by using SQL Azure Portal and also make it by inputting right commands in Powershell console.
There is an interactive relationship between Windows Azure Cmdlets and SQL Azure Portal. If you add or delete any firewall rules by using Windows Azure Cmdlets, the relevant firewall rules will present the updates on SQL Azure Portal.


