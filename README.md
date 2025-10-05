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



</p>
<br />


<h2> << Ticket Assignment and Communication | Working Tickets |	Resolution of Tickets >> </h2>

<p>
<img width="600" height="494" alt="Step 2" src="https://github.com/user-attachments/assets/5ae610c2-ed55-4ef3-ac65-d6ed8f2678fe" />


</p>
<p>
  
- Go to the osTicket login page: http://localhost/osTicket/scp/login.php .
	- Log in as the admin user we created in the previous project.
	- One of mine was Jane.


</p>
<br />
<p>
<img width="962" height="462" alt="Step 2a" src="https://github.com/user-attachments/assets/766068ee-fba6-48c4-959c-8ef26ec036e4" />


<p>
  
- Once logged in, you will see the 2 tickets that were submitted by the users Karen and Ken. Select the “Online Banking” one from Karen.
   
</p>
<br />
<p>
<img width="770" height="999" alt="Step 2a1" src="https://github.com/user-attachments/assets/bb8d20f5-e701-4eab-97af-50949772acb9" />


<p>
  
- As a Help Desk Agent (Jane) will observe the ticket’s properties.
- The Priority, Department, SLA and who the ticket is assigned to.
- Set the Ticket’s properties by selecting the SLA Plan “Default SLA”.

   
</p>
<br />
<p>
<img width="650" height="255" alt="Step 2a2" src="https://github.com/user-attachments/assets/9ac077ff-1a0d-44a6-a643-2fbbc5aa6df7" />


<p>
  
- Change it to “SEV-A” and write down a reason. Then select “Update”.
   
</p>
<br />

<p>
<img width="650" height="255" alt="Step 2a3" src="https://github.com/user-attachments/assets/29292c16-94e2-45da-8a78-eb594626a093" />


<p>
  
- Then select the Help Topic “Report a Problem”.
- Change it to “Report a Problem / Business Critical Outage” and write down the reason. Then select “Update”.

   
</p>
<br />

<p>
<img width="650" height="255" alt="Step 2a4" src="https://github.com/user-attachments/assets/fc90d09e-9485-4a8a-80d3-0af96de7d328" />


<p>
  
- Then Select the Assigned To “Unassigned”.
- Change it to “Online Banking” and write down a reason. Then Select “Assign”.

   
</p>
<br />

<p>
<img width="889" height="997" alt="Step 2a5" src="https://github.com/user-attachments/assets/b1ec1c08-6142-4a93-9d6b-31a2276e007b" />


<p>
  
- So, I assigned the ticket to the Online Banking Team and made it so that Jane took over the ticket. I acknowledged the issue to Karen the user and responded with an update of what the problem could be related to after investigating. This assures Karen that I am working on the problem. Then I updated Karen on what was done to solve the problem, assuring her that everything is back up and running.
- The Ticket Thread section is there to give the agent access to communicate to the customer, to keep them updated on the ticket’s progress of being resolved. It’s also very important through good communication skills to remain polite and empathetic with the customers.

   
</p>
<br />

<p>
<img width="770" height="998" alt="Step 2a6" src="https://github.com/user-attachments/assets/b3030823-313f-49b3-9124-724f4ab4e321" />


<p>
  
- Now that everything is resolved, you can now close the ticket. Select the Status “Open” and select “Resolved”. Then select “Close”.
   
</p>
<br />

<p>
<img width="650" height="255" alt="Step 2b1" src="https://github.com/user-attachments/assets/496fa44d-37df-4676-ab7e-1d25bb4f6727" />


<p>
  
- Next, we are going to assign the other ticket to John since John has a support role.
- Jane as the Admin Agent will give John the assignment. Select the other ticket that was submitted by Ken from the previous steps above.
- This ticket submitted by Ken is a Password Reset issue and we are going to let John from the Support Team take over.
  
   
</p>
<br />

<p>
<img width="605" height="546" alt="Step 2b2" src="https://github.com/user-attachments/assets/7e7a37c2-2ff9-4653-abc0-a420b4445884" />


<p>
  
- Next, log out and go log in as John.
   
</p>
<br />

<p>
<img width="960" height="464" alt="Step 2b2a" src="https://github.com/user-attachments/assets/2ec5c67a-cd35-45fc-9ffa-d1bbc79f4bdc" />


<p>
  
- Then select the ticket assigned which in this case is the Password Reset ticket.

   
</p>
<br />

<p>
<img width="897" height="996" alt="Step 2b3" src="https://github.com/user-attachments/assets/4b4e08f8-b454-419e-b461-bcc1a6881d66" />


<p>
  
- The issue has been resolved as we can see by reading the Ticket Thread log.

   
</p>
<br />

<p>
<img width="822" height="994" alt="Step 2b4" src="https://github.com/user-attachments/assets/ed4d3e5a-6b7b-4c63-8af3-51a2155c40f6" />


<p>
  
- You can now close the ticket to status resolved. 

   
</p>
<br />

<p>
<img width="960" height="441" alt="Step 2c" src="https://github.com/user-attachments/assets/92992a73-73f1-435c-8ccf-381720964d29" />


<p>
  
- All tickets have been resolved.

   
</p>
<br />

<h2> << Conclusion >> </h2>

<p>
  
- Close the Remote Desktop connection.
- Go Back to your Azure resource group page.
- Make sure your VMs are on “Stop” status if you are not going to use them right away. This way you will not be charged while they are not in use.
- Also, make sure your VMs are on “Stop” status if you are not going to use them right away. This way you will not be charged while they are not in use.
- To conclude, we were able to create tickets from an end user’s perspective. Then we were able to go and solve those tickets through the Helpdesk Agents perspective. Both from an admin role and a support role. 


</p>
<br />

