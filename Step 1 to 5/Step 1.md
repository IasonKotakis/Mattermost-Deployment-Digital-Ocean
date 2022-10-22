# Step 1 - Setting up Digital Ocean

<hr>

<h2> Description </h2>

<p>Working with ticketing systems will take up a sizable deal of your time as a system administrator. You can prioritise tasks and gain a better understanding of how your firm uses IT as a whole by using a ticketing system effectively.
You will need to provision a virtual machine for this project, then set up the OTRS ticketing system on it.</p>



<h2> VM Setup </h2>

<p>You must use your DigitalOcean account to construct a virtual machine (VM) before you can begin this project. Log in to the dashboard and follow these steps to continue:</p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/create%20droplet.png" alt="create droplet " width="700px" height="500px">

<ol>
    <li>At the top right of the dashboard, click ‘Create’ button, and then select ‘droplets’</li>
    <li>Under the ‘Choose an Image’ section, click ‘Snapshots’.</li>
    <li>Select Ubuntu destribution.</li>
    <li>Under the ‘Choose Plan’ section, click ‘Basic’ and select CPU options ‘Regular with SSD’.</li>
    <li>Click the arrow to scroll to the left of the list of Droplet types, scroll to the far left until you can choose ‘$6/month’. </li>
    <li>Switch the ‘Authentication’ button to Password. Choose a memorable one and be sure to record it somewhere safe.</li>
    <li>At the very bottom under ‘Choose a hostname’, you can name it to your desires.</li>
    <li>Click ‘Create’</li>
    <li>You will now be back to at the dashboard. Once your VM is finished provisioning, look for it in the list and copy the IP address somewhere.</li>
</ol>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/droplet%20dashboard.png" alt="droplet dashboard" width="700px" height="500px">

<ol>
    <li>In the menu on the very left, select ‘Manage’ and navigate to ‘Networking’ section.</li>
    <li>In the HOSTNAME field add your hostname from point 7 above.</li>
    <li>In the ‘WILL REDIRECT TO’ ﬁeld, input the IP address you recorded, or
select your VM from the list.</li>
    <li>Click ‘Create Record’ and ZABAM, now you can use either PuTTy or any SSH client to connect to your VM.</li>
</ol>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/clapping-hands-emoji-clipart-md.png" alt="clapping hands" width="200px" height="200px">

<hr>
 Wonderful!!! You have completed Step 1, how about we delve deeper and set OTRS on 
 (https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/documentation/Digital%20Ocean/Digital%20Ocean%20documentation)
 [Digital Ocean](https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/documentation/Digital%20Ocean/Digital%20Ocean%20documentation)
