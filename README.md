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

- Create a Resource Group
- Create a Virtual Machine
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

 ![Screen Shot 2024-12-30 at 6 15 05 PM](https://github.com/user-attachments/assets/9d03e2a9-a8a7-4d88-968b-f2b0c247aa04)

First, we'll create our Resource Group (RG) inside of Azure.
</p>
<br />

![Screen Shot 2024-12-30 at 8 43 03 PM](https://github.com/user-attachments/assets/b5c6d2b7-3bdb-4140-9ae9-8d5b4cc9a66f)

![Screen Shot 2024-12-30 at 8 43 33 PM](https://github.com/user-attachments/assets/757a0526-c13d-4a36-a0a2-9f7f4b4cd277)

Now create your Windows virtual machine. I typically create the VM in (US) East US.

While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make note of the username and password you decide to use.
</p>
<br />

![Screen Shot 2024-12-30 at 8 53 06 PM](https://github.com/user-attachments/assets/4ecc6aff-dbb6-4ad9-a73b-d3d9a49b4f3c)

![Screen Shot 2024-12-30 at 8 53 17 PM](https://github.com/user-attachments/assets/3f92e48c-21a6-47ab-9b78-f386a412d91b)

Create an Linux (Ubuntu) virtual machine.

While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the Administrator Account section.
</p>
<br />

![Screen Shot 2025-01-01 at 6 23 50 PM](https://github.com/user-attachments/assets/ce11c1cd-3f04-4e88-9f4c-c90ab465d3e2)

We'll observe some ICMP Traffic. Remote into your Windows 10 Virtual Machine, install Wireshark, open it and filter for ICMP traffic only.
</p>
<br />

![Screen Shot 2025-01-01 at 6 33 25 PM](https://github.com/user-attachments/assets/2b22e680-5d4c-4fa3-9c2d-d644fe759ea7)

![Screen Shot 2025-01-01 at 6 39 47 PM](https://github.com/user-attachments/assets/ffb6599e-e38f-4ab6-9a5a-9dfca2e6d4ae)

![Screen Shot 2025-01-01 at 6 41 08 PM](https://github.com/user-attachments/assets/8208efac-5312-4705-bdfc-ca840ecfa540)

Retrieve the private IP address of the Linux VM and attempt to ping it from within the Windows 10 VM. Observe ping requests and replies within WireShark.
</p>
<br />

![Screen Shot 2025-01-01 at 6 56 57 PM](https://github.com/user-attachments/assets/3e499bc0-c003-4485-831c-c43a382749f5)

![Screen Shot 2025-01-01 at 6 57 26 PM](https://github.com/user-attachments/assets/ab41861d-b2d1-4ad9-a2a0-686c212ff47e)

Attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark.
</p>
<br />

![Screen Shot 2025-01-01 at 7 13 59 PM](https://github.com/user-attachments/assets/ab6d3c1f-74b7-4850-979a-b16ac4c76766)

![Screen Shot 2025-01-01 at 7 14 25 PM](https://github.com/user-attachments/assets/2d763293-2230-462c-a399-efd279dc54b1)

Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM.
</p>
<br />

![Screen Shot 2025-01-01 at 7 18 31 PM](https://github.com/user-attachments/assets/d859727f-0104-4426-ab58-3ced66cb798b)

![Screen Shot 2025-01-01 at 7 20 22 PM](https://github.com/user-attachments/assets/9593ac26-1318-4fed-92f1-8e5ff0b8629b)

Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic, while back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity.
</p>
<br />

![Screen Shot 2025-01-01 at 7 22 30 PM](https://github.com/user-attachments/assets/c6a91c2c-e405-42f8-948d-77ed1b0a29ad)

Re-enable ICMP traffic for the Network Security Group in your Ubuntu VM and back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity (should start working again). Finally, stop the ping activity.
</p>
<br />

![Screen Shot 2025-01-01 at 7 48 43 PM](https://github.com/user-attachments/assets/609a76b1-a161-4cef-a75e-cf210c63a981)

Now, we'll observe SSH Traffic.
  
Back in Wireshark, filter for SSH traffic only and from your Windows 10 VM, “SSH into” your Ubuntu virtual machine (via its private IP address). Type commands (ls, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark.

Exit the SSH connection by typing ‘exit’ and pressing [return]:
</p>
<br />

![Screen Shot 2025-01-01 at 7 53 16 PM](https://github.com/user-attachments/assets/54e05f86-f248-4ce9-8a9a-711d4e437bb6)

Next, observe DHCP Traffic. Back in Wireshark, filter for DHCP traffic only. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)

Observe the DHCP traffic appearing in WireShark:
</p>
<br />

![Screen Shot 2025-01-01 at 8 01 51 PM](https://github.com/user-attachments/assets/08e2a872-24b0-4b0f-a18d-07826dbe8e97)

Back in Wireshark, filter for DNS traffic only. From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are and observe the DNS traffic being shown in WireShark.
</p>
<br />

![Screen Shot 2025-01-01 at 8 06 12 PM](https://github.com/user-attachments/assets/9adc3336-d8d4-4f61-af25-e9e503297014)

Finally, we'll observe RDP Traffic. Back in Wireshark, filter for RDP traffic only using "tcp.port==3389". You'll be obseving a non-stop stream of traffic. Do you know why there is constant traffic in our tcp.port==3389?

The answer is because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefore, traffic is always being transmitted.
</p>
<br />

Now that we're finished observing the network, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT! This will prevent you from incurring additional charges.

Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion. You'll typically be notified or can click unde the bell notification just to make sure.
