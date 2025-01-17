# 16-01-2025

# Documentation about Microsoft Azure Virtual Machine creation:


Steps to Create a Virtual Machine
Log into Azure Portal.Navigate to portal.azure.com and log in with our Azure credentials.
Create a New Resource
On the homepage, click Create a resource.
Select Virtual Machine.
Configure the Virtual Machine (fill in all info like subscriptions, resource group, VM name, region, etc...)
Marketplace Image: Choose Ubuntu Server 24.04 LTS Gen 2 published by Canonical.
Name the VM: Use a logical naming convention.
VM Size: Select Standard_B2ls_v2.
Public IP Configuration: Specify the public IP and configure it to allow SSH traffic.
-click next Disk(OS disk size, OS disk Type,key manegement.) 
-click next Networking (VM Network, basic, Allow selected port: HTTP(s)80,443,SSH 22. select delete public IP when VM is deleted, enable accelerated networking, none, )
-Management (select: Enable basic plan free, Auto-shutdown, Time zone, image default )
-click next monitoring (Enable with manage storageaccount) 
-click next Advanced (None )
-click next Tags 
-click next Review + create ( view all info , add phone number, and then create)
-Generate New key pair(Download private key and create resource)
-Give the DNS Name.


# Connection to Local Machine
-Native SSH (go to the downloaded KEY PAIR file and press SHIFT + RIGHT CLICK to copy the file path and paste it in the SSH commad)
-Open windows powershell and then chose the command prompt then copy the SSH to VM specidied private key and paste it in the command prompt and run it.





