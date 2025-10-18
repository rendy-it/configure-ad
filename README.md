<p align="center">
<img width="2560" height="1440" alt="Active Directory Logo" src="https://github.com/user-attachments/assets/09373d78-71f6-4af6-8737-110159040f9c" />



</p>

<h1>On-premises Active Directory Deployed in the Azure Cloud Infrastructure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Windows Virtual Machine on Microsoft Azure
- Remote Desktop Connection
- Microsoft Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022 for a Domain Controller
- Windows 10 Pro </b> (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

-	Step 1: Create a Domain Controller Windows Server 2022 VM.
-	Step 2: Use the Windows-VM that we used from previous projects to be the Client.
-	Step 3: Set the Domain Controller’s VM’s private IP to static and disable the Windows Firewall.
-	Step 4: Set the Windows- VM’s DNS settings to DC-1’s Private IP address.
-	Step 5: Confirm network connectivity between the DC-1 VM and the Client VM.
-	Step 6: Install Active Directory in the DC-1 VM. Promote it as a Domain Controller and log back in using domain credentials.
-	Step 7: Create a Domain Admin user within the DC-1 VM domain.
-	Step 8: Join Client-1 Virtual Machine to the domain (mydomain.com).
-	Step 9: Setup RDP for non-administrative users on the Client-1 VM.
-	Step 10: Create additional Non-Administrative Users and attempt to login into Client-1’s VM with one of the new users. 

<h2>Deployment and Configuration Steps</h2>  

<h2> << Step 1: Create a Domain Controller Windows Server 2022 VM >> </h2>
<p>
<img width="372" height="364" alt="Step 1" src="https://github.com/user-attachments/assets/c52606d9-c394-409c-b7a7-fbb0edf07071" /> <img width="53" height="364" alt="Right Arrow for Step 1_1a" src="https://github.com/user-attachments/assets/02fec441-0c57-4cba-89ea-b284105715ba" /> <img width="371" height="364" alt="Step 1a" src="https://github.com/user-attachments/assets/e0427367-3448-4966-b035-795b79085809" />





</p>
<p>
  
- On the Azure home page, hover you mouse cursor on “Virtual Machines”.
- A small window will appear, then click on “Create”.
- Then select “Virtual Machine”. 


</p>
<br />
<p>
<img width="686" height="1007" alt="Step 1b" src="https://github.com/user-attachments/assets/5ef2f882-c6bb-4a32-b62d-4c3eaa6ae5d4" />


<p>
  
- On the next page, select your resource group.
  - Give the VM a name (DC-1).
  - Select a Zone.
  - Select “Windows Server 2022” for the OS image.
  - And select at least 2 vcpus, with 8 or 16 gig memory.


  
</p>
<br />
<p>
<img width="614" height="317" alt="Step 1c" src="https://github.com/user-attachments/assets/daa109ac-5421-4835-b8c9-53b71b6d74d1" />


</p>
<p>
  
- Next, create a Username and Password


</p>
<br />
<p>
<img width="777" height="283" alt="Step 1d" src="https://github.com/user-attachments/assets/624aba38-fc57-4df9-8340-488883b09528" />


</p>
<p>
  
- Next, go to the “Networking” tab.

</p>
<br />
<p>
<img width="777" height="283" alt="Step 1d" src="https://github.com/user-attachments/assets/624aba38-fc57-4df9-8340-488883b09528" />


</p>
<p>
  
- On the next page:
  - Make sure to select the same Virtual Network as the Windows VM that we used from previous projects.
  - Everything else is set by default.


</p>
<br />
<p>
<img width="739" height="331" alt="Step 1f" src="https://github.com/user-attachments/assets/a3b074f2-61f3-42da-8d3b-e8bcf233029b" />


</p>
<p>
  
- Go back to the “Basics” tab.
- Scroll to the bottom and select “Review + Create”.



</p>
<br />
<p>
<img width="431" height="1009" alt="Step 1g" src="https://github.com/user-attachments/assets/883fb488-e5fa-4087-89e0-0c06d81a3b81" />



</p>
<p>
  
- On the next Page:
  - Double Check to make sure everything is to your liking.
  - Then select “Create”.
  - Once completed you can click on “Go to resource”.
  - Then you will be able to see the VM that you just created with all the information.




</p>
<br />


<h2> << Step 2: Use the Windows-VM that we used from previous projects to be the Client >> </h2>

<p>
<img width="771" height="974" alt="Step 0" src="https://github.com/user-attachments/assets/1ed00524-e11e-4efc-9739-65cb5d858437" />


</p>
<p>
  
- For the client VM we are going to use the Windows VM that we created in a previous project referenced [here](https://github.com/rendy-it/azure-vm).
- If you have not created one before just follow the exact steps for the Windows 10 VM created from that project.
- Once you are done, head back to the VM page of your Azure Resource Group and your Windows 10 VM should be there alongside your DC-1 VM.


</p>
<br />

<h2> << Step 3: Set the Domain Controller’s VM’s private IP to static and disable the Windows Firewall >> </h2>


<p>
<img width="366" height="433" alt="Step 2" src="https://github.com/user-attachments/assets/9e0b69b2-6eb9-4e33-af3c-e386ea0d9fd5" /><img width="72" height="433" alt="Right Arrow for Step 2_2a" src="https://github.com/user-attachments/assets/9f868247-3e48-4bff-bba8-b8b7cf678aa9" />
<img width="366" height="433" alt="Step 2a" src="https://github.com/user-attachments/assets/424228bd-2fb3-432b-8d43-022bc99b0c5b" />



<p>
  
- After Creating the VMs, we need to set the DC VM’s NIC’s private ip address to be static.
- Go to the DC-1 VM’s page in Azure and select “Network Settings”.
- Then click on the “Network Interface / IP configuration”.

   
</p>
<br />
<p>
<img width="336" height="594" alt="Step 2a1" src="https://github.com/user-attachments/assets/1858cc82-6cd2-4913-8ce9-b93b0110f9be" /><img width="81" height="594" alt="Right Arrow for Step 2a1_2a1a" src="https://github.com/user-attachments/assets/1063d4fe-7064-4b8d-8f99-cd5470f4ff7b" /><img width="337" height="594" alt="Step 2a1a" src="https://github.com/user-attachments/assets/7c58bb4e-5d6c-4824-a5d6-1c0896529727" />



<p>

  
- Then, click on “ipconfig1” and change the Allocation from “Dynamic” to “Static” and click on “Save”.
- Once you do that, the private IP address of DC-1 should no longer change.
  
   
</p>
<br />
<p>
<img width="1186" height="440" alt="Step 2a2" src="https://github.com/user-attachments/assets/caef7ce6-352a-41ec-ae6d-081fea2c6b2e" />

<p>
  
- Next, log into the DC-1 VM with RDP using the VM’s public IP address.
   
</p>
<br />

<p>
<img width="886" height="761" alt="Step 2a3" src="https://github.com/user-attachments/assets/0a58343a-bd44-4afc-8ed4-3ab856015271" />


<p>
  
- Then once logged in, you want to run “wf.msc” for windows firewall.

   
</p>
<br />

<p>
<img width="836" height="626" alt="Step 2a4" src="https://github.com/user-attachments/assets/1f7d054a-00f5-4574-8cce-013780046f59" />


<p>
  
- Then, select “Windows Defender Firewall Properties”.

   
</p>
<br />

<p>
<img width="1400" height="454" alt="Step 2a5_2a6_2a7" src="https://github.com/user-attachments/assets/bd1f9db5-0e7f-4584-ae39-72c7850f7203" />



<p>
  
- Then for the “Domain” “Private” and “Public” profile tabs make sure that “Firewall State” is off. Then select “Apply”.
- Once that is done, the windows firewall for the DC-1 VM should be off.

   
</p>
<br />

<h2> << Step 4: Set the Windows- VM’s DNS settings to DC-1’s Private IP address >> </h2>


<p>
<img width="563" height="432" alt="Step 4" src="https://github.com/user-attachments/assets/20b4b043-42a2-42ce-902d-bc895e6ecef7" />


<p>
  
- Go to the DC-1 VM page. Make note and copy the private IP address.
   
</p>
<br />

<p>
<img width="893" height="506" alt="Step 4a" src="https://github.com/user-attachments/assets/bcab426f-ca02-4c3d-9634-730d911d9b07" />


<p>
  
- Go to the Windows-VM page and select “Network Settings”
- Then click on the “Network Interface / IP configuration”.

 
   
</p>
<br />

<p>
<img width="758" height="449" alt="Step 4a1" src="https://github.com/user-attachments/assets/8e5779c5-9261-47cb-b3b9-b3400325d389" />


<p>
  
- Then select the “DNS servers” tab on the left.
- Then, select “Custom” and paste the DC-1 VM private IP address number. Then select “Save”.

   
</p>
<br />

<p>
<img width="1458" height="403" alt="Step 4a3" src="https://github.com/user-attachments/assets/2efa9048-196d-4a07-8961-90010f6309db" />


<p>
  
- The DNS configuration is now complete. To make sure all changes have been applied. Go back to your Azure VMs page and restart all the VMs.
   
</p>
<br />

<h2> << Step 5: Confirm network connectivity between the DC-1 VM and the Client VM >> </h2>


<p>
<img width="643" height="460" alt="Step 5" src="https://github.com/user-attachments/assets/8fffb1e6-9414-4899-a1aa-0e6799651853" />


<p>
  
- We are going to log in to the Client Windows-VM and attempt to ping DC-1s private IP address.
- Make note of and copy your DC-1 VM’s private IP address.

   
</p>
<br />

<p>
<img width="887" height="726" alt="Step 5a" src="https://github.com/user-attachments/assets/cbb7c728-f04a-4257-8833-5a8a6bc2b760" />


<p>
  
- Log in to the Windows VM using RDP.
- Then you want to open “PowerShell”

   
</p>
<br />

<p>
<img width="761" height="534" alt="Step 5a1" src="https://github.com/user-attachments/assets/074dd991-92d4-4638-99a2-7f900d3eeba8" />


<p>
  
- Then type “ping” and paste the DC-1 private IP number.
- Example: “ping 10.1.0.5” then press enter and it should ping successfully.


   
</p>
<br />

<p>
<img width="699" height="867" alt="Step 5a2" src="https://github.com/user-attachments/assets/50dee3fc-0f73-4839-923d-e7c8fd1ff1d4" />


<p>
  
- Next, type “ipconfig /all” and it should give us results in which will show DC-1’s private IP address.
- The network connectivity attempt between both VMs is now complete.


   
</p>
<br />

<h2> << Step 6: Install Active Directory in the DC-1 VM. Promote it as a Domain Controller and log back in using domain credentials >> </h2>

<p>
<img width="649" height="685" alt="Step 6" src="https://github.com/user-attachments/assets/bda3dc76-ad94-4a77-9a09-76d1a472ad17" />


<p>
  
- Login to the DC-1 VM using RDP.
- When you are logged in you should have the Server Manager software opened. If it is not opened, click on Start and select “Server Manager”.


   
</p>
<br />

<p>
<img width="652" height="616" alt="Step 6a" src="https://github.com/user-attachments/assets/c7d86aac-789e-49cd-9c6d-c6a78d77a822" />


<p>
  
- Next on the Server Manager app, select “Add roles and features”.

   
</p>
<br />

<p>
<img width="1785" height="557" alt="Step 6a1_6a2" src="https://github.com/user-attachments/assets/88e1fb6e-eb5e-4da4-9268-a6dfdc81d8d7" />


<p>
  
- Select Next all the way until “Server Roles”, then select “Active Directory Domain Services”. Then Select “Add Features”.

   
</p>
<br />

<p>
<img width="1789" height="562" alt="Step 6a3_6a4" src="https://github.com/user-attachments/assets/47f08ca6-8a12-41bf-aade-4be9a1ff8f95" />



<p>
  
- Then, select “Next all the way and then click “Install”.
- Once Installed, select “Close”.


   
</p>
<br />
<p>
<img width="1435" height="772" alt="Step 6b" src="https://github.com/user-attachments/assets/f456d28f-dccb-431c-8a1f-76f5a20e7319" />



<p>
  
- Once Installed, promote the server as a Domain Controller.
- Go to the top right on the window, select the flag with the caution symbol.
- Then select “Promote this server to a domain controller”.


   
</p>
<br />

<p>
<img width="1765" height="564" alt="Step 6b1_b2" src="https://github.com/user-attachments/assets/33a20028-4065-4378-965c-ca402d8874a6" />



<p>
  
- Then, select “Add new forest” and make it “mydomain.com”. then hit “Next”.
- Make a simple password for yourself. Then hit “Next”.


   
</p>
<br />

<p>
<img width="1765" height="564" alt="Step 6b3" src="https://github.com/user-attachments/assets/1bf640f2-d12c-421b-9c25-9338f91a7d20" />


<p>
  
- Click “Next” all the way to “Prerequisites Check”, then click “Install”.

   
</p>
<br />

<p>
<img width="1511" height="685" alt="Step 6b4" src="https://github.com/user-attachments/assets/c4978936-622a-4d20-aba2-b67cae7db71c" />


<p>
  
- Once the VM restarts, you can now log in using your domain credentials with your DC-1 VM username (mydomain.com\labuser) to be in control of Active directory. 


   
</p>
<br />

<h2> << Step 7: Create a Domain Admin user within the DC-1 VM domain >> </h2>

<p>
<img width="1411" height="746" alt="Step 7" src="https://github.com/user-attachments/assets/2f2c1e0d-9407-42d6-878e-3763f1831185" />


<p>
  
- Once logged into the DC-1 VM domain.
- In the search bar look for “Active Directory Users and Computers” and open it.


   
</p>
<br />

<p>
<img width="1758" height="534" alt="Step 7a_7a1" src="https://github.com/user-attachments/assets/0f123447-e93c-4aa8-9757-20e9f6e55638" />



<p>
  
- Next, right click on “mydomain.com”, go to “New” then “Organization Unit” and give it the name “_EMPLOYEES”. Then click Ok.

   
</p>
<br />

<p>
<img width="1258" height="634" alt="Step 7a2" src="https://github.com/user-attachments/assets/a54f286d-cae1-4425-9b95-fb37471e8632" />

<p>
  
- Then, we are going to create another one and this time with the name “_ADMINS”. Then click Ok.

   
</p>
<br />

<p>
<img width="1258" height="632" alt="Step 7b" src="https://github.com/user-attachments/assets/61a6ef13-3286-464f-9ab9-4d36e20ff3e2" />


<p>
  
- Next, we are going to create a new employee account.
- Right click on “_ADMINS”, go to “New” then “User”.



   
</p>
<br />
<p>
<img width="1758" height="532" alt="Step 7b1_7b2" src="https://github.com/user-attachments/assets/b6738c63-2513-4a2a-840a-050b01cb8e1a" />



<p>
  
- Then, give her the name “Jane Doe”. Then “jane_admin” for the username.
- Click next and give her a password. Use the same password as the VM to make it easier just for this project. (Don’t do it in the real world).
- Uncheck “User must change password at next logon” just for this project. In the real world, this would be best to leave checked. Then click “Next” and then click “Finish”. 


   
</p>
<br />

<p>
<img width="1258" height="632" alt="Step 7c" src="https://github.com/user-attachments/assets/71eef92a-8ea5-45c8-80df-a2fe061ea5fe" />


<p>
  
- Next, we are going to add this account we just made to the “Domain Admins” Security Group.
- Select the “_ADMINS” folder on the left and right click on “Jane Doe” and select “Properties”.

   
</p>
<br />

<p>
<img width="1758" height="532" alt="Step 7c1_7c2" src="https://github.com/user-attachments/assets/a12eba9e-5fb6-4b88-93f6-47dc0b763d2c" />

<p>
  
- Then select the “Member Of” tab and then select “Add…”.
- Then, type “domain admins” and then click on “Check Names”. Then click on Ok. Then click on Apply and then click on Ok.
- Now this account is a Domain Admin which gives the ability to create users and other abilities.


   
</p>
<br />

<p>
<img width="1458" height="574" alt="Step 7d" src="https://github.com/user-attachments/assets/ded6142b-94a2-430b-8a72-dbd4fbf08fbb" />


<p>
  
- Next, log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”. User account “jane_admin is the Administrator account from now on.


   
</p>
<br />

<h2> << Step 8: Join Client-1 Virtual Machine to the domain (mydomain.com) >> </h2>

<p>
<img width="1622" height="643" alt="Step 8_8a" src="https://github.com/user-attachments/assets/dff2771b-e0cf-4efb-8c6f-f1b68e9ca7ae" />


<p>
  
- To join Client-1 to the domain, first log into the Client-1 VM.
- Then right click on the start menu, then click “System”.
- Then click on “Rename this PC (advanced)”.

   
</p>
<br />

<p>
<img width="935" height="471" alt="Step 8a1_8a2" src="https://github.com/user-attachments/assets/6916e7ac-85a5-4f1a-b5f9-6ede6dde6acb" />


<p>
  
-	Then on the “Computer Name” tab click on “Change….”.
-	Then change the domain to “mydomain.com”. Click Ok.


   
</p>
<br />

<p>
<img width="1010" height="304" alt="Step 8a3_8a4" src="https://github.com/user-attachments/assets/e6759bdb-90bc-475a-be8e-491d4ce92f98" />


<p>
  
- Then input the credentials of the “jane_admin” account “mydomain.com\jane_admin” as it is the Domain Admin that we created in the previous steps and it will allow Client-1 to join the domain. Then click Ok.
- Then you will see a welcome message telling you that the Client-1 VM has joined the domain (mydomain.com). Then click on Ok. And restart the Client-1 VM. When it restarts, it will now be part of the domain.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Once the Client-1 VM restarts and successfully joins the domain, log back into the DC-1 VM.


   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Open “Active Directory Users and Computers” in the search bar.


   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
-	Then right click on “mydomain.com” tab on the left, go to “New”, then “Organizational Unit” and give it the name “_CLIENTS”. Then click Ok.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
-	Then go to the “Computers” tab and select and drag “Client-1” into the “_CLIENTS” folder that we created. Then click on Yes.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
-	Then when you refresh the “mydomain.com” tab, you will now see all the Organizational Unit folders that we created in their order. 

   
</p>
<br />

<h2> << Step 9: Setup RDP for non-administrative users on the Client-1 VM >> </h2>

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/30b86ab0-6f2e-40af-945a-c88b881afd57" />


<p>
  
- First, log into the Client-1 VM as username “mydomain.com\jane_admin” and the password that you set for it in previous steps.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/db68a43c-5c06-4be2-9f33-9d5ccd98c811" />


<p>
  
- Then right click on the start menu, then click “System”.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/8baf9e70-55f3-488d-a5d0-c6bd08862454" />

<p>
  
- Then click on “Remote Desktop”.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
-	Then click “Select users that can remotely access this PC”.


   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Then click on “Add...”.


   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Then input “domain users” in the box and then click on “Check Names” then click on Ok.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Now all standard users are by default able to access this computer. Remotely. Click on Ok.

   
</p>
<br />

<h2> << Step 10: Create additional Non-Administrative Users and attempt to login into Client-1’s VM with one of the new users >> </h2>

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/30b86ab0-6f2e-40af-945a-c88b881afd57" />


<p>
  
- First, log into the DC-1 VM as username “mydomain.com\jane_admin” and the password that you set for it in previous steps.
   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/db68a43c-5c06-4be2-9f33-9d5ccd98c811" />


<p>
  
- Then open PowerShell ISE as an administrator.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/8baf9e70-55f3-488d-a5d0-c6bd08862454" />

<p>
  
- Then you are going to create a new file and paste the contents of a script that can be used to automatically generate multiple employee user accounts in the “_EMPLOYEES” Organizational Unit that we created in the previous steps.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
-	In the PowerShell ISE interface, select the “New Script” icon.


   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Then save the script as “create users” on your desktop.


   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Then paste into it the contents of the multiple employee user accounts script copied.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Next, run the scrip by clicking on the “Run Script” icon or hit “F5” on your keyboard. What this script will do is create thousands of users with random generated.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Next you want to open “Active Directory Users and Computers” to observe the accounts being created in the _EMPLOYEES” Organizational Unit.
- As this is just a project, you can stop the script by hitting on the stop icon as plenty of new users have been created. 

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Next, we are going to attempt to log into the Client-1 VM with one if the user accounts that was created with the script. Reminder that each user’s password by default in the script was set to “Password1”.
- Back on “Active Directory Users and Computers”, in the “_EMPLOYEES” folder, select a random user of your choosing and attempt to log into that account in the Client-1 VM.
- For example, I will pick “loco.nala” as the user. We are going to attempt to log into the Client-1 VM with that user account.

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Input “mydomain.com\loco.nala” as the username and “Password1” as the password as this is the default password for all new users created based on the script. 

   
</p>
<br />

<p>
<img width="367" height="75" alt="Insert Image Here" src="https://github.com/user-attachments/assets/a8f4a131-6a2b-4fbe-acd7-46ae8eb2b68f" />


<p>
  
- Once logged in. We can verify in multiple ways. Open the command prompt and it will display the name of the account. And also go to the Users folder in the C drive and there will be a folder for “loco.nala”. And to confirm that this account does not have admin permissions try to open the “jane_admin” folder and it will not open without administration credentials.

   
</p>
<br />



<h2> << Conclusion >> </h2>

<p>
  
- Close the Remote Desktop connection.
- Go Back to your Azure resource group page.
- Also, make sure your VMs are on “Stop” status if you are not going to use them right away. This way you will not be charged while they are not in use.
- To conclude, we have successfully implemented the on-premises of Active Directory within Virtual machines on the Azure Cloud infrastructure. 


</p>
<br />

