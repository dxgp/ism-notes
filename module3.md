## Module 3

### How to Troubleshoot Firewall Problems

**Port**: A port is a virtual point where network connections start and end. Ports are software-based and managed by a computer's operating system. Each port is associated with a specific process or service. Ports allow computers to easily differentiate between different kinds of traffic: emails go to a different port than webpages, for instance, even though both reach a computer over the same Internet connection.

**ICMP Ping**: IP based signal sent from one device to another. If the target device receives the ping, it will respond to confirm that it is active and connected to the network. It's used to determine if a device is online or not.
1. Ping a PC near the device as an initial test.
2. Ping the device itself. If the pings sent in step 1 go through but the pings to the device fail, the problem is with SNMP receiver device.
3. Try telnet to browse the device. If that succeeds, then the receiver SNMP device has a firewall issue.
4. Reckeck if the SNMP device's firewall settings are blocking standard ports and using non-standard ones. (for e.g. port 80 is commonly used for HTTP. However, the firewall settings may block port 80 and instead use port 56349).
5. Check if the firewall is configured to block specific IP address ranges.
6. Use `traceroute` to check where in the connection the problem is stemming from. It may be the case that the pings are not reaching the SNMP device due to an interruption in the line.

### How to Troubleshoot CISCO IOS Firewall Configurations
