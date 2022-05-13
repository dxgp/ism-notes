# Module 3

## How to Troubleshoot Firewall Problems

**Port**: A port is a virtual point where network connections start and end. Ports are software-based and managed by a computer's operating system. Each port is associated with a specific process or service. Ports allow computers to easily differentiate between different kinds of traffic: emails go to a different port than webpages, for instance, even though both reach a computer over the same Internet connection.

**ICMP Ping**: IP based signal sent from one device to another. If the target device receives the ping, it will respond to confirm that it is active and connected to the network. It's used to determine if a device is online or not.
1. Ping a PC near the device as an initial test.
2. Ping the device itself. If the pings sent in step 1 go through but the pings to the device fail, the problem is with SNMP receiver device.
3. Try telnet to browse the device. If that succeeds, then the receiver SNMP device has a firewall issue.
4. Reckeck if the SNMP device's firewall settings are blocking standard ports and using non-standard ones. (for e.g. port 80 is commonly used for HTTP. However, the firewall settings may block port 80 and instead use port 56349).
5. Check if the firewall is configured to block specific IP address ranges.
6. Use `traceroute` to check where in the connection the problem is stemming from. It may be the case that the pings are not reaching the SNMP device due to an interruption in the line.

## CISCO IOS Firewall

### Configuring Cisco IOS Firewall IDS
This task consists of four subtasks:
1. Initializing Cisco IOS Firewall IDS
```
Router(config)# ip audit smtp spam recipients
Router(config)# ip audit po max-events number_events
Router(config)# exit
```
2. Initializing the post office
Sends event notifications (alarms) to either a Cisco Secure IDS Director, a syslog server, or both.
```
Router(config)# ip audit notify nr-director
```
Sets the Post Office parameters for both the router (using the ip audit po local command) and the Cisco Secure IDS Director (using the ip audit po remote command).
```
router(config)# ip audit po local hostid host-id orgid org-id
```
Sets the Post Office parameters for both the Cisco Secure IDS Director (using the ip audit po remote command).
- host-id is a unique number between 1 and 65535 that identifies the Director.
- org-id is a unique number between 1 and 65535 that identifies the organization to which the router and Director both belong.
- rmtaddress ip-address is the Director’s IP address.
- localaddress ip-address is the router’s interface IP
address.
- port-number identifies the UDP port on which the Director is listening for alarms (45000 is the default).
- preference-number is the relative priority of the route to the Director (1 is the default)—if more than one route is used to reach the same Director, then one must be a primary route (preference 1) and the other a secondary route (preference 2).
- seconds is the number of seconds the Post Office waits before it determines that a connection has timed out
(5 is the default).
- application-type is either director or logger.
```
Router(config)# ip audit po remote hostid host-id orgid org-id rmtaddress ip-address localaddress ip-address port port-number preference preference-number timeout seconds application application-type
```
```
Router(config)# logging console info
Router(config)# exit
Router# write memory
Router# reload
```
3. Configuring and applying audit rules
First, we need to set the default actions for info and attack signatures:
```
Router(config)# ip audit info {action [alarm] [drop] [reset]}
Router(config)# ip audit attack {action [alarm] [drop] [reset]}
```
Then, we create audit rules. Here, audit-name is a user defined name for an audit rule.
```
Router(config)# ip audit name audit-name {info | attack} [list standard-acl] [action [alarm] [drop] [reset]]
```
Then, we enter the interface configuration mode, followed by appying audit rule at an interface.
```
Router(config-if)# interface interface-number
Router(config-if)# ip audit audit-name {in | out}
Router(config-if)# exit
```
Then, we configure which network must be protected by the router:
```
Router(config)# ip audit po protected ip-addr [to ip-addr]
Router(config)# exit
```


4. Verifying the configuration.
Finally, we can verify the configuration using the:

```
ids2611# show ip audit configuration
```

Here is a complete example:
```

ip audit smtp spam 25
ip audit notify nr-director
ip audit notify log
ip audit po local hostid 55 orgid 123
ip audit po remote hostid 14 orgid 123 rmtaddress 10.1.1.99 localaddress 10.1.1.1 ip audit po remote hostid 15 orgid 123 rmtaddress 10.1.2.99 localaddress 10.1.1.1
ip audit name AUDIT.1 info action alarm
ip audit name AUDIT.1 attack action alarm drop reset
interface e0
 ip address 10.1.1.1 255.0.0.0
 ip audit AUDIT.1 in
interface e1
 ip address 172.16.57.1 255.255.255.0
 ip audit AUDIT.1 in
```


### Troubleshooting
1. Access lists can be removed using:
```
int <interface>
no ip access-group (access-group-no) in|out
```
2. If too much traffic is being blocked, the logic used in the access list might be a problem. Permit all traffic using:
```
access-list (access-list-no) permit tcp any any
access-list (access-list-no) permit udp any any
access-list (access-list-no) permit icmp any any
```
Then, apply the access list using:
```
int <interface>
ip access-group (access-group-no) in|out
```
3. Check which access lists are being used using the `show ip access-lists` command. 
4. If the router is not a heavy traffic one, inspect traffic using the `debug` command.

## Troubleshooting the Router
To troublshoot routers:
1. Check physical layer: Check for power issues, plugs and circuit breakers.
2. Check interfaces: `show ip interface brief` command can beused to re-check which interfaces are being used.
3. Ping: The `ping` and `trace` command can be used to check for connectivity issues.
4. Checking routing: The routing table can be checked using the `show ip route` command. Verify that the routing table is correctly configured.
5. Check firewall: Check if a firewall is configured. If it is configured, re-check the configuration.
6. Check VPN: Check if the VPN is up using `show crypto isakmp sa` and `show crypto ipsec client ezvpn` commands.

The following commands can be used on Cisco routers:
1. `show`: This is a powerful monitoring and troubleshooting tool that can be used to monitor router behavior during installation, monitor network operation, isolate problem interfaces, determine when a network is congested and determine the status of servers, clients or other neighbors.

<table>
<tr>
    <th>Command</th>
    <th>Use</th>
</tr>
<tr>
    <td><code>show interfaces [ethernet|fddi|atm|serial]</code></td>
    <td>Use the show interfaces command to display statistics for all interfaces (or the specified interfaces) configured on the router or access server.</td>
</tr>
<tr>
    <td><code>show controllers [mci|token|FDDI|LEX|ethernet|E1|MCI|cxbus|t1]</code></td>
    <td>The show controllers command displays information on all of the router's controllers. However, you can specify the type of controller so that only the information on that particular controller is displayed.</td>
</tr>
<tr>
    <td><code>show running-config</code></td>
    <td>The show running-config command shows the router, switch, or firewall’s current configuration. The running-configuration is the config that is in the router’s memory. You change this config when you make changes to the router. </td>
</tr>
<tr>
    <td><code>show startup-config</code></td>
    <td>To display the contents of NVRAM (if present and valid) or to show the configuration file pointed to by the CONFIG_FILE environment variable. </td>
</tr>
<tr>
    <td><code>show flash-filesystem</code></td>
    <td>To display the layout and contents of a Flash memory file system, use the show flash-filesystem command in EXEC mode.</td>
</tr>
<tr>
    <td><code>show buffers</code></td>
    <td>To display detailed information about the buffer pools on the network server when Cisco IOS, Cisco IOS Software Modularity, or Cisco IOS XE images are running, use the show buffers command in user EXEC or privileged EXEC mode.</td>
</tr>
<tr>
    <td><code>show processes memory</code></td>
    <td>Displays memory used by processes</td>
</tr>
<tr>
    <td><code>show stacks</code></td>
    <td>Monitors the stack usage of processes and interrupt routines.</td>
</tr>
<tr>
    <td><code>show version</code></td>
    <td>Displays the configuration of the system hardware, the software version, the names and sources of configuration files, and the boot images.</td>
</tr>
</table>

2. `debug`: This command provides various functions like traffic, error messages produced by nodes, protocol specific diagnostic packets and other troubleshooting data. To use the debug commands, enter into privelaged mode and show the possible commands using:

```
Router>enable
Router# debug ?
```
It must be noted that debug commands are processor intensive.

3. `ping`: Used to check host reachability and network connectivity. It also requires privilaged mode. The command sends ICMP echo messgaes.

4. `trace`: The `trace user` command discovers the routes that a router's packets follow when travelling to their destinations.