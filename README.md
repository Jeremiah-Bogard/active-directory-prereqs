# Active Directory and File Sharing
Learn how to setup a Domain Controller with Windows Server and install Active Directory then create a users with certain permissions and create files to share with specific groups.

## Requirments

1. Microsoft Azure subscription
2. Create two Virtual Machines
    1. Windows Server
    2. Windows 10
4. How to create virtual machines in Microsoft Azure
5. How to connect to VM using remote desktop
    1. Microsoft Remote Desktop
    2. Windows Remote Desktop

## Active Directory Prerequisites

In this section we will install and setup Active Directory in which we will create two different users and Organizational Unit (OU). You will need to have already created a Windows Server VM and a Windows 10 VM.

### Windows Server VM

First thing we will need to do is allow communication between both Virtual Machines. To do this we need to open the "Windows Defense Firewall and Advanced Security" in the Windows Server VM (Domain Controller). After you open, go to "Inbound Rules" then enable both rules highlighted below.

![image](/firewall.png)

> ### How to enable rules
> Right click on the rule and click enable. You can also just click on it and press the enable rule button on the right side of the window.

To ensure you did this right, remote desktop into the Windows 10 VM (client) and run this command in the Command Prompt: ``` ping 10.0.0.4 ``` where "10.0.0.4" is the private IP Address of the Domain Controller.

Now we need to add Active Directory to the Domain Controller. We can do this by opening the Server Manager and select "Add Roles and Features."

> ### How to open Server Manager
> If the Server Manager is not already opened then you can click the start button and type "Server Manager."

![image](/server-manager.png)

You will need to add the "Active Directory Domain Services" feature. After you add that feature, just hit next over and over agian until you can install it. While that installs click the flag in the navigation bar on the right, then select "Promote as a Domain Server." You will need to setup a forest. The domain can be anything you want like "mydomain.com." After completing this you will need to restart the VM. When it comes back, open the Server Manager, select Tools, and click Active Directory Users and Computers.

![image](/AD-user-computers.png)

As you can see, if you click on the mydomain.com (or whatever you set the domain to) there are several folders. One such folder is called Users, which of course is a list of users connected to the Domain Server. But before we create a new user, we need to make severall Organization Units (OU). We can acomplish this by right clicking on the mydomain.com, select new, then select Organizational Unit. Create three OU's \_ADMINS, \_EMPLOYEES, and \_CLIENTS. We will use the clients folder later.

![image](/AD-org-unit.png)

Now we will create a user! Right click on the \_EMPLOYEES folder and create a new user just like you created a OU. Just simply fill out the form then right click on the user and select properties.

![image](/create-john-doe.png)

Go to the "Member of" tab and type "domain admins" then click "Check Names" it should underline the domain admins. Then click 'Ok' and don't forget to apply the changes.

![image](/assign-group.png)

### Windows 10 VM

![image](/system-button.png)

![image](/connect-mydomain.png)

![image](/client-1-clients.png)

# File Sharing with Permissions

![image](/file-sharing.png)

![image](/group-access-settings.png)
