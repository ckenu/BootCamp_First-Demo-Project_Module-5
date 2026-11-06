Purpose
The purpose of module 5 is to understand the basics of Cloud and Infrastructure as a Service (IaaS).
Topics covered will be:

Create a virtual server on a cloud provider using DigitalOcean.
Deploy an application on the server and configure access.
Review common concepts and best practices.

Create a Droplet Server on DigitalOcean

Create a DigitalOcean account.
Select Create Droplet.
Choose Region (closest to you).
Choose an image (operating system) Ubuntu.
Choose Size / Droplet Type (choose Basic Plan)
Choose CPU Options. Depending on your needs, plans can range in options and price. Choose Regular Disk and $4/mo as this is enough for this for module.

Select Regular Disk Type: SSD
Select $4/mo 512 MD/1CPU, 10 GB SSD Disk, and 500 GB/Transfer


Choose Authentication Method

Password is selected by default, however SSH Key is considered best practice. Select SSH Key.
If SSH Key doesn't already exist in DigitalOcean, the first one will need to be created. Once created can be used for other Droplets.
Select Add SSH Key.

From the local CLI in the home directory type cat ~/.ssh/id_rsa.pub
Copy your public ssh id and paste it in the SSH Key Contect section of DigitalOcean.
Name your SSH Key (example my-droplet-key)
Click Add SSH Key




Leave remaining settings as is and Click Create Droplet.

Securing DigitalOcean Droplet
When creating a server on DigitalOcean by default the sever is not protected by a firewall and is accessible by all ports. This is bad security practice.
Configuring the firewall will allow access on specific ports. Best practice suggests closing all port access by default then only allowing access to required ports.

Select your newly created Droplet.
Select Networking.
Scroll down to Firewalls and Select Edit.
Select Create Firewall.
Name the firewall, example my-droplet-firewall (Once the rules are configured they the can be applied to other Droplets).
By default the Inbound Rules are set to following, however for this module Source should be updated with your own computers IP address.
Type: SSH
Protocol: TCP
Port Range: 22
Source: All IPv4 All IPv6

To confirm your own IP address you can visit link

Copy your IP address and add it to the Source section of the Inbound Rules.
Outbound Rules will not need to be changed at this time.


Click Create Firewall.
Once the firewall is created, you'll return to the Firewalls section under Networking within DigitalOcean.
Select your firewall rule.

Select Droplets tab.
Click Add Droplets.
Start typing the name of your Droplet Server in order to apply the firewall rules to the server.
Click Add Droplet.
These steps allow for the Droplet server to be now accessed via ssh on port 22 from your computer's IP address.



Accessing Droplet Server
To access your Droplet via SSH
- Click copy next to your Droplet's ipv4 address.
- Return to your computers CLI.
- Type ssh root@xxx.xx.xxx.xxx (paste the copied ipv4 address) and press Enter.
- An authentication message will appear; you can type Yes.
You are now logged in the the Droplet Ubuntu server as the root user. The state of the server is completely clean and there are no applications installed.
Install Java on Droplet
From the CLI of the Ubuntu Server, before installing java, type 'apt update' in order to update the sever with the latest packages.
When you type java the following is returned; "Command 'java' not found, but can be installed with:" listing different version of java.
The version used for this module is openjdk-17-jre-headless because it will be needed for future applications.

Type 'apt install openjdk-17-jre-headless' to install java.
Type Y to confirm.
Now when you type java you'll see the latest version installed of the sever.

Deploy Application on Droplet
This part of the module will include copying and building a Jar File, copying it to the remote server then running the app on the remote server.

Clone the java-react project repository locally using the java-react-example project link: link
From your local CLI,
cd java-react-example
gradle build

If done correctly, "Build Successful" will display. To confirm, return to local java-react-example project and look for java-react-example.jar was created under the libs folder.
Now want the jar file to run on the Ubuntu server on DigitalOcean.

Return to local CLI and run scp (secure copy) to copy the jar file from local computer to server. Use the following command (replace xx's with Droplet IP address) scp build/libs/java-react-example.jar root@xxx.xx.xxx.xxx:/root


Switch back to Server CLI and type 'ls' to confirm java-react-example.jar file is listed and now up loaded to the server.
Run java -jar java-react-example.jar to start program. You will see that the server is listening on port 7071. In order access this program from a browser, the port will need to be opened on the sever.

Note: In order the continue using the CLI, run the app in the background by adding & to the end for the command; example java -jar java-react-example.jar &. Otherwise, the app will continue in the foreground and if the CLI is closed the app will stop.


Return to DigitalOcean.
Locate the firewall rules that you created earlier.
In the Inbound Rules, click 'New Rule'
Type: Custom
Protocol: TCP
Port Range: 7071
Source: All IPv4 All IPv6

Click Save
Copy Droplet's IP address and paste it into a new browser window. Should now see the program running within the Browser.
