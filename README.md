# Active Directory and File Sharing
Learn how to set up a Domain Controller with Windows Server and install Active Directory then create a user with certain permissions and create files to share with specific groups.

## Requirements

1. Microsoft Azure subscription
2. Create two Virtual Machines
    1. Windows Server
    2. Windows 10
4. How to create virtual machines in Microsoft Azure
5. How to connect to VM using remote desktop
    1. Microsoft Remote Desktop
    2. Windows Remote Desktop

## Active Directory Prerequisites

In this section we will install and set up Active Directory in which we will create two different users and an Organizational Unit (OU). You will need to have already created a Windows Server VM and a Windows 10 VM.

### Windows Server VM

First thing we will need to do is allow communication between both Virtual Machines. To do this we need to open the "Windows Defense Firewall and Advanced Security" in the Windows Server VM (Domain Controller). After you open, go to "Inbound Rules" then enable both rules highlighted below.

![Firewall](/firewall.png)

> ### How to enable rules
> Right click on the rule and click enable. You can also just click on it and press the enable rule button on the right side of the window.

To ensure you did this right, remote desktop into the Windows 10 VM (client) and run this command in the Command Prompt: ``` ping 10.0.0.4 ``` where "10.0.0.4" is the private IP Address of the Domain Controller.

Now we need to add Active Directory to the Domain Controller. We can do this by opening the Server Manager and selecting "Add Roles and Features."

> ### How to open Server Manager
> If the Server Manager is not already opened then you can click the start button and type "Server Manager."

![Server Manager](/server-manager.png)

You will need to add the "Active Directory Domain Services'' feature. After you add that feature, just hit next over and over again until you can install it. While that installs click the flag in the navigation bar on the right, then select "Promote as a Domain Server." You will need to set up a forest. The domain can be anything you want like "mydomain.com." After completing this you will need to restart the VM. When it comes back, open the Server Manager, select Tools, and click Active Directory Users and Computers.

![Active Directory Users and Computers](/AD-user-computers.png)

As you can see, if you click on the mydomain.com (or whatever you set the domain to) there are several folders. One such folder is called Users, which of course is a list of users connected to the Domain Server. But before we create a new user, we need to make several Organization Units (OU). We can accomplish this by right clicking on mydomain.com, select new, then select Organizational Unit. Create three OU's \_ADMINS, \_EMPLOYEES, and \_CLIENTS. We will use the clients folder later.

![Organization Unit](/AD-org-unit.png)

Now we will create a user! Right click on the \_EMPLOYEES folder and create a new user just like you created an OU. Just simply fill out the form then right click on the user and select properties.

![Creating John Doe Account](/create-john-doe.png)

Go to the "Member of" tab and type "domain admins" then click "Check Names" it should underline the domain admins. Then click 'Ok' and don't forget to apply the changes.

![Assign Group](/assign-group.png)

### Windows 10 VM

Now login to the Windows 10 VM. To connect the client (our Windows 10 VM) to the Domain Controller, we need to join it to our domain. First we need to change the DNS Server of the VM set to the private IP Address of the Domain Controller.

> ### How to Change the DNS Server
> In order to update the DNS Server of the Windows VM in Microsoft Azure, you will need to click on the VM. Then go to the Networking tab, click on the NIC (Network Interface Card):
> ![Network Interface Card Location](/nic.png)
> Now click on the DNS Servers and add a custom server that is set to the private IP Address of the Domain Controller. You may need to restart the VM from Microsoft Azure.

After changing the DNS Server of our client, we need to right click the start button on the VM and select the system button.

![System Button Location](/system-button.png)

When the system settings loads, select the "Rename this PC (advanced)." It should open a pop-up, on it click the "Change" button to change the domain. Now select domain and change it to mydomain or whatever your Domain Controller's domain is.

![Connect To My Domain](/connect-mydomain.png)

# File Sharing with Permissions

To share files with your computers connected to your Domain Controller, you will need to create some files of the C: drive of the Domain Controller. You can get to the C: drive by clicking start and typing C: then select the option that pops-up. Within this drive create three folders: "read-access", "write-access", and "admins-only". The naming does not matter, this is just so we know which files we can mess with. Now if you go back to our client VM click the start button and type: ``` \\dc-1 ``` where "dc-1" is the name of your Domain Controller. We can see the folders we just created. However, they do not have restrictions. If I login with a normal domain user then I can still do whatever I want with the folders we created. To fix this, go back to the domain controller and right click on one of the folders. Select the properties button then navigate to the sharing tab and click "Share". Now type "domain users'' and click add. Now in the dropdown select Read, Read/Write, or Remove (in case you messed up).

![File Sharing](/file-sharing.png)

![Group Access Settings](/group-access-settings.png)

Set the folders to the following conditions:

| Folder | Group | Access |
|--------|-------|--------|
| read-access | Domain Users | Read |
| write-access | Domain Users | Read/Write |
| admins-only | Domain Admins | Read/Write |

Upon completion return to the client VM as a regular user (not an admin). Now see if you can modify the read-access folder or even look at the admins-only folder. You can't, but if you login as an admin user, then you can see and modify the admins-only folder and do all of what the normal user could do.
