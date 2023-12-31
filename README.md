<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.I throughly explain how to set up a domain controller with active directory and connecting a seperate virtual machine to the domain controller by using the same Private DNS IP Address. I also use the contents of a script given to me by my instructor in Powershell ISE as an administrator.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell ISE

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Set up the domain controller(DC-1) and Client-1(Virtual Machine) in Azure.
- Step 2: Set up Active Directory on DC-1.
- Step 3: Join Client-1 to DC-1 by changing DNS server.
- Step 4: How to use a powershell script in order to create other employee usernames that are connected to Client-1 and DC-1.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/d2DX4bX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Use your Azure account and make a resource group. Next create a virtual machine with windows servers 2022 datacenter: Azure Edition, make sure it has 2vcpus. Name the virtual machine DC-1(Domain Controller 1). After it has been created change the NIC private IP Address to static by going to the network,click the NIC name, Click IP-Configurations,lastly click IP Address and change to static.Lastly create another virtual machine as a regular operating system and call it client 1. When creating Client 1 put it in the same resource group as DC-1 and use Windows 10 as the operating system, the size of the operating system should be 2 vcpus. Next, put Client-1 in the same virtual network as DC-1. Login to Client-1 and DC-1 on separate Remote Desktop Connection programs using their unique public IP Addresses. Now you're logged in to both DC-1 and Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/pq5G1ow.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Client-1 activate cmd in the search bar and use command prompt ping -t with the private DC-1 IP Address. It will perpetually say request time out because the DC-1 windows firewall is blocking icmp traffic. Next go into the DC-1 system and activate server manager, after it is activated type in search bar, windows defender firewall with advanced security, while in the program click inbound rules and enable rule for, core network diagnostics-ICMP and enable the second one with the same name. Then go back to Client 1 command prompt, the ping will work, stop the ping with ctrl-C. There is now a guaranteed connectivity between DC-1 and Client-1. Lastly we want to set up active directory on DC-1 by activating service manager and clicking add roles and features under service manager, click next on the first three sections, on the server roles section click active directory domain services and click add features, click next for the remaining sections then click install.Once installed click on flag icon called notifications under task click promote this server to a domain controller. In the deployment configuration section click add a new forest,make a root domain name then click next,under domain controller options, make a password and click next. Click next for the remaining sections and finally click install. DC-1 will restart and you will have to log back in with the same public IP Address and the username will be domainname\username and the same password. 
</p>
<br />

<p>
<img src="https://i.imgur.com/f5U7lJD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back into DC-1 after the system has restarted. Click on server manager,and type in search bar active directory users and computers and click on the program. Click on your domain name, right click on domain name, click new, click organization unit(OU) name it employee,repeat the same step but name the OU Admins. Click on Admins OU, right click, click new, click user, put whatever name you want(ex Jane Doe), and login name should be Jane_Admin, create a password and click next, click finish. The employee is now created in Admins OU. Right click the employee name and click properties. Click members of, click add, type domain and click check names, click domain admins group,click ok,click apply, then click ok.Log out of DC-1 and log back in with the same public IP address, username would be domainname\Jane_admin, use the same password. You have now logged in as an employee. Go back to your main operating system, go on Azure resource group, copy DC-1 NIC private IP Address. Click on Client-1, click on network under settings, click network interface client number, click DNS servers, click custom, paste DC-1 private IP Address,Click save, and wait for Client-1 to update. Restart Client-1 from Azure. Log into Client-1 as the original local admin. Use the Client-1 public IP Address, and the same username and password to log into the remote desktop. Click on search bar type cmd,type ipconfig /all in command prompt, find DNS servers, make sure its the same as the domain controller DC-1. Right click the start menu, click rename this PC, in systems property click change, click domain, type in your domain name, click ok. Your now in windows security. Username is domainname\Jane_Admin and DC-1 password. Let Client-1 update and click restart now. Log back into Client-1 using the public IP Address, username domainname\Jane_Admin, and the same Client-1 password.
</p>
<img src="https://i.imgur.com/qcb6ggo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


</p>
<p>
When logged into Client-1, right click the start menu,click systems, click remote desktop,click select users that can remotely access this PC. In remote desktop users click add,type domain users,click check names,click ok,click ok. All domain users are now allowed to log in to Client-1.Return to DC-1 and type in the search bar active directory users and computers,click domainname, click users folder, click domain users,click members. You can now log into Client-1 as a normal, non-administrative user. Log into DC-1, type into search bar powershell_ISE and right click on it and click as administrator. Take content from the script, the script is given by the instructor in the form of a link. Copy the whole script, create a new file in powereshell_ISE and paste the content of the script in it.Run script and observe accounts being created.Go to the active directory users and computers, right click on employees OU,click refresh.Copy any employees usersname. Log out of Client-1, then log back in with the same public IP Address and password, the username will be domainname\employee name, click enter.Return to DC-1, go on to active directory users and computers, click on employees OU, right click on any username, you can enable accounts, reset passwords,and check properties for each individual user. 
