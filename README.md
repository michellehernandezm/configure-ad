<p align="center">
<img src="https://imgur.com/ocgeUoz.png" alt="osTicket logo"/>
</p>

<h1>Microsoft Azure- Configuring Active Directory Within Microsoft VMs </h1>
In this tutorial, we will be activating Active Directory within a virtual machine. We will also create another client desktop within another virtual machine and have the users that are configured on the Active Directory attempt to login to the client desktop.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Command Line/Windows Powershell
- Microsoft Active Directory
- Control Pannel

<h2>Operating Systems Used </h2>

- Windows 10</b> (version 22H2)

<h2>List of Prerequisites</h2>

- Windwows Powershell
- Control Pannel
- Remote Desktop

<h2>Installation Steps</h2>


<p>  
<img src = "https://imgur.com/xOn8Ufa.png" " height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>

<p>Overview</p>

<p>
<img src = "https://imgur.com/LYWGrDr.png" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src = "https://imgur.com/fwpNOew.png" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 <p>
1. Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that are created (create a resource group if one isn't automatically created).

</p>                                                                                                    
                                                                                                     


<p>
   
 <p>
<img src= "https://imgur.com/SxlNlD0.png" " height="80%" width="80%" alt="Disk Sanitization Steps" />
</p>
                                                                                                 
                                                                                                 
2. Set Domain Controller’s NIC Private IP address to be static. 
</p>
<br />

<p>
<img src="https://i.imgur.com/dDY9AQi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>




<p>
<img src="https://imgur.com/mhivNHG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>



<p>
3. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1. (the one used for "DC-1"). Note that it is important that you choose Windows Server 2022 as your VM's operating system.
</p>
<br />

<p>
<img src="https://imgur.com/gFyMk77.png" width="80%" alt="Disk Sanitization Steps"/>
</p>


<p>
4. Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher).
</p>
<br />

<p>
<img src="https://imgur.com/ecn1aBB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
5. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). The ping is expected to fail or "timeout".
</p>
<br />

<p>
<img src="https://imgur.com/jFBSzWt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
6. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.

</p>
<br />

<p>
<img src="https://imgur.com/E5HmHDi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
7. Check back at Client-1 to see the ping succeed

</p>
<br />

<p>
<img src="https://imgur.com/2s2furr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
8. Login to DC-1 and install Active Directory Domain Services

</p>
<br />

<p>
<img src="https://imgur.com/Nlg31jf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
9. Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
</p>
<br />


<p>
10. Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://imgur.com/JNPV4Q6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
11. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
</p>
<br />

<p> 12. Create a new OU named “_ADMINS” </p>

<p>
<img src="https://imgur.com/kHDTEbh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 13. Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” to the _ADMINS OU. </p>

<p>
<img src="https://imgur.com/K05g1Ni.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>



<p>14. Add jane_admin to the “Domain Admins” Security Group </p>

<p>15. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
</p>

<p>
<img src="https://imgur.com/Nu5aPaH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 16. User jane_admin as your admin account from now on  </p>



<p>
<img src= "https://imgur.com/YNb8oQc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


<p> 17. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address</p>



<p> 18. From the Azure Portal, restart Client-1</p>


<p> 19. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)</p>

<p>
<img src= "https://imgur.com/wMekVWq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


<p>
<img src= "https://imgur.com/KOUakfR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 20. Login to the Domain Controller (Remote Desktop) and verify that client-1 shows up in ADUC</p>


<p>
<img src= "https://imgur.com/KhR7rDB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 21. Create a new OU named “_CLIENTS” and drag Client-1 into there</p>

<p>
<img src= "https://imgur.com/AP2QvUl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 22.Log into Client-1 as mydomain.com\jane_admin and open system properties. Click “Remote Desktop” and allow “domain users” access to remote desktop.</p>

<p> 23.You can now log into Client-1 as a normal, non-administrative user. Login to DC-1 as jane_admin and
open PowerShell_ise as an administrator</p>


<img src= "https://imgur.com/eOOi5LN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p> 24.Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).</p>

<img src= "https://imgur.com/ngYiYVI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p> 25.Run the script and observe the accounts being created. Open ADUC and observe the accounts in the appropriate OU. Note: Since this Github lab was completed on multiple days, I changed the domain name from www.mydomain.com to mydomain.com</p>

<img src= "https://imgur.com/DhzSers.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p> 26. Attempt to log into Client-1 with one of the accounts (take note of the password in the script).</p>







