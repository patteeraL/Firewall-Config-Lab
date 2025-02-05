# 🔥 Firewall-Config-Lab

## 📌 Overview

This repository contains Lab 1 of the **Network and System Security (DV2636)** course at **Blekinge Institute of Technology, Sweden**. The lab focuses on configuring **networking and firewall settings** in a virtualized environment using **Parallels VMs** on an **Apple Silicon (M3) Mac**.

## 🛠 Environment Setup

Since I use an **Apple Silicon (M3) Mac**, I installed **Parallels VMs** to import the provided **Parallels Ubuntu VMs**. The imported VMs are prepared as four Parallels packages:

📂 **Virtual Machines**:
- **🖥️ ClientA.pvmp**
- **🖥️ ServerA.pvmp**
- **🖥️ ClientB.pvmp**
- **🖥️ ServerB.pvmp**

I configured the **network preferences** for each VM according to the provided **network diagrams**. The **MAC address** of each VM is listed in the network preferences.

<img src="Lab environment.png" alt="Environment Setup" width="1000">

## **🌐 Network Interface Configuration**

To list the network interfaces available in **Server A guest OS**, run:

```
sudo ip link
```

To view the manual pages for the ```ip``` command:
```
man ip
```
## **🌍 Finding the Default Gateway**

On the host OS, to identify the interface that can reach the default gateway, use:
```
netstat -rn -f inet
```
On the guest OS, use:
```
ip -4 route
```
## **🔎 Verifying Network Connectivity**

To test connectivity, we use Wireshark and ping:

Open Wireshark on both the host and guest OS. In the guest OS terminal, run:
```
ping 192.168.60.2
```
This pings the host-only interface in the host OS. After 4-5 seconds, stop the ping and the Wireshark capture.

The ICMP traffic in Wireshark confirms that network connectivity is functioning properly.

## **🔑 Establishing SSH Connection**

To enable SSH from the host OS to the guest OS (Server A):

🛠** Parallels Network Settings**:

Click the ⚙️ **cog symbol** > **Hardware** > **NAT adapter network** > **Advanced...** > **Network Preferences...**

Add a port forwarding rule for SSH (**port 22**).

**📌 Configuring HTTP and HTTPS Forwarding**:

Allow access to HTTP (**port 80**) and HTTPS (**port 443**) in Server A.

## **🔥 Viewing Firewall Rules in the VM**

To inspect the default iptables firewall rules:
```
sudo iptables -L   # Filter table
sudo iptables -t mangle -L  # Mangle table
sudo iptables -t nat -L  # NAT table
```
📌 The default policy for all chains (INPUT, FORWARD, OUTPUT) is ACCEPT.

## **🚫 Blocking HTTP Browsing in the Guest OS**

To block HTTP traffic in the guest OS:
```
sudo iptables -A OUTPUT -p tcp --dport 80 -j DROP
```
To block the host OS from accessing HTTP content from Apache2 in the guest OS:
```
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```
To unblock HTTP access, flush all OUTPUT chain rules:
```
sudo iptables -F OUTPUT
```
Then, test by browsing to http://www.httpvshttps.com.

## **🛡 Configuring Firewall Rules in ```firewall.sh```**

Modify the ```firewall.sh``` script to block HTTP traffic:
```
$IPT -A INPUT -p tcp --dport 80 -j DROP
```
Execute the script:
```
sudo ./firewall.sh
```
Verify the iptables rules:
```
sudo iptables -L
```
For additional firewall configurations, check the firewall.sh file, where comments explain each rule.

## Conclusion

This lab provides hands-on experience in network configuration and firewall management using iptables in a virtualized Linux environment. 

📩 Feel free to contribute or raise an issue if you have any questions! 🤝

