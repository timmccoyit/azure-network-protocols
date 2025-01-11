<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create A resource group where we house two VM's and A virtual network in Microsoft Azure 
- Use remote desktop to connect to the windows Virtual Machine and install Wireshark
- Use wireshark to filter for ICMP traffic specifically and observe it
- Configure a firewall (Network Security Group) to block inbound ICMP traffic
- Log into Windows VM and SSH into your Ubuntu VM then filter for SSH traffic in wireshark
- Filter for DHCP Traffic then assign yourself a new ip address utilizing Powershell commands as an Administrator
- Use CLI to nslookup some websites and Observe DNS traffic
- Observe RDP traffic on port 3389 and decide if the data is a continous stream or if its sent in increments. Why is it sent that way? Figure it out
- Completion of lab clean up resources and virtual environment

<h2>Wireshark Install and Basic ICMP Traffic Observation</h2>


![image](https://github.com/user-attachments/assets/76563425-dac2-432e-b457-0b6ad2929162)
![image](https://github.com/user-attachments/assets/143c8187-4fd0-4c7f-8ec3-23f912406b11)

<p>Internet Control Message Protocol (ICMP) is a tool computers on a network use to send error messages and important information about network communication. This could also be referred to as a "troubleshooting tool." In the first image we install Wireshark, which is a popular network protocol analyzer used to capture and analyze data traveling through a network in real time. After installing Wireshark, we filter the packet capture traffic to specifically view ICMP traffic. Wireshark is open source as well so give it a try if you havent!  
</p>
<br />

<h2>Firewall Configuration</h2>

![image](https://github.com/user-attachments/assets/20aa9f18-73a7-4ceb-b573-da9dc6a2c889)
![image](https://github.com/user-attachments/assets/f74ca359-469c-489d-83c0-b683f5d88445)

<p>
In the above screenshots, we are configuring the inbound security rules for the linux machine's firewall in the network security settings. We configure the firewall to block inbound ICMP traffic, then we obsever the pings to the linux machine timing out/failing. After confirming this, we get delete this security rule and make sure the ping traffic is coming through again. 
</p>
<br />

<h2>SSH Traffic Observation</h2>

![image](https://github.com/user-attachments/assets/fe280467-3f53-4baf-96aa-e3f31f30756c)

<p>
We SSH into our linux machine in powershell and filter the packet capture to only view SSH interactions. Notice every interaction including simple keystrokes is being shown in real time in ICMP. Logout of SSH and prepare for more observation.
</p>
<br />

<h2>DHCP Traffic Observation</h2>

![image](https://github.com/user-attachments/assets/5adbc3b4-45c8-412c-9f1c-0e46617cf350)

<p>
Now we're filtering for traffic on UDP ports 67 & 68 also known for being the DHCP ports. First we observe that there is nothing there yet then, we use the CLI commands "ipconfig /release and ipconfig /renew" to give up our current IP address then recieve a new one from the DHCP server. Its important to note, that we created a simple batch script to automatically run the both these commands at the same time. The reason for doing this, is because releasing the IP address while on the virtual machine will essentially shut it down due to it no longer having network connectivity. 
</p>
<br />

<h2>DNS Traffic Observation</h2>

![image](https://github.com/user-attachments/assets/d47f74f7-8b71-4b2b-b853-d3756b208f0f)
![image](https://github.com/user-attachments/assets/a4cae94e-a2b8-48dd-9b9d-cca18e6514f5)

<p>
The Domain Name System(DNS) portion is very simple and quick to accomplish. First we filter for DNS traffic which can be done by either typing DNS in the filter or typing the actual ports that DNS uses which is TCP port 53 and UDP port 53. We then, observed the packets in ICMP and used nslookup to find the ip address of a commonly known website and watch the traffic again in wireshark. The website does not come up but you can clearly see that this ip address is associated with domain name that came up in CLI. 
</p>
<br />

<h2>RDP Traffic Observation</h2>

![image](https://github.com/user-attachments/assets/382d56f2-464d-4a9d-9c14-c847a0303b3e)

<p>
Lastly, we filter for TCP/UDP port 3389 also known as the Remote Desktop Protocol(RDP) default port, because this is the default port number, there can be security concerns associated with this but for the purpose of demonstration in this lab this port is what will be used. Once we filter for this port we observe the traffic in Wireshark which it will be continous. This is simply because there are large amounts of data being continously streamed. In conclusion, we clean up our Virtual environment in Microsoft Azure by deleting the resources we used.
</p>
<br />
