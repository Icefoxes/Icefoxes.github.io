---
layout:       post
title:        "Opening Ports on the Windows Firewall"
subtitle:     "Opening Ports on the Windows Firewall With GUI/CMD/PowerShell"
date:         2018-02-22 12:00:00
author:       "Kuo"
header-img:   "img/post-bg-ubuntu.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Windows Server
---

#### Opening Ports on the Windows Firewall Using GUI
To open a port in the firewall using the GUI in Windows Server 2008/2012 R2 and Windows Server 2016, follow the below steps:
1. Login using an administrator account.
2. Click Start > Administrative Tools > Windows Firewall with Advanced Security

![Open Port](/img/WindowsServerPorts/1.png)

3. Click on `Inbound Rules`, and then on `New Rule`

![New Rule](/img/WindowsServerPorts/2.png)                                                                         
The wizard to open a port and accept incoming connections has five steps: `Rule Type`, `Protocol` and `Ports`, `Action`, `Profile`, and `Name`. For this example, we will open TCP port 20002 on servers that are running the Parallels RAS Publishing Agents role:

- In the `Rule Type` section, select `Port` and click Next:
![Rule Type](/img/WindowsServerPorts/3.png)      


- In the `Protocol` and `Ports` section, select TCP as the type of protocol and type 20002 in the Specific local ports input field:
![Protocol](/img/WindowsServerPorts/4.png)  

- In the `Action` section, select `Allow the Connection` and click Next.
![Action](/img/WindowsServerPorts/5.png)  

- In the `Profile` section, select all three options and click Next. If you wish to limit the connection to a particular profile, you can do so by selecting only the profiles you think are appropriate to your setup. For this example, we will open the port on all profiles.
![Profile](/img/WindowsServerPorts/6.png)  

- In the `Name` section, enter a descriptive name for this rule. It is recommended to list the port number in the name, so the rule is easily recognizable. For example, name the new rule Pub_Agent_20002_IN. Click Finish when ready.
![Name](/img/WindowsServerPorts/7.png) 

To open additional ports, repeat the above procedure for each additional port and/or protocol you’d like to open in each server.

#### Opening Ports on the Windows Firewall Using Command Line (netsh)
To open a port on the Windows Firewall using the netsh command line, follow the below procedure:
1. Login to the server using an administrator account.
2. Run the Command Prompt as Administrator.
3. Execute the following command to open the TCP port 20002 on the servers running the Publishing Agents role:
```cmd
netsh advfirewall firewall add rule name="name" dir=in|out protocol=tcp|udp localport=20002 action=allow
```
To open additional ports, repeat the above procedure for each additional port and/or protocol you’d like to open in each server.

#### Opening Ports on the Windows Firewall Using PowerShell
To open a port in the Windows Firewall using PowerShell commands, follow the below procedure (applies only for 2012 R2 and 2016 Windows Server OS):
1. Logon using an administrator account.
2. Run the Windows PowerShell as Administrator.
3. Execute the following command to open the TCP port 20,002 on the servers running the Publishing Agents role:
```powershell
New-NetFirewallRule -DisplayName "Name" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 2002
```