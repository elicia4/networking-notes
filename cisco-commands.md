# Cisco Commands

- Shortcuts:
    - To go to the previous command, use the `Up` arrow or `CTRL+P`.
    - To go to the next command, use the `Down` arrow or `CTRL+N`.
    - To go to the beginning of the command line, use `CTRL+A`.
    - To go the end of the command line, use `CTRL+E`.
    - To delete a word: `CTRL+W`.
    - To erase characters from the cursor to beginning of the line: `CTRL+U`
       or `CTRL+X`.
    - To exit from GCM: `CTRL+C`.
    - Cancel a command: `CTRL+Shift+6`

---

- Show help:

  ```
  ?
  ```

- Enter privileged EXEC mode[^1]:

  ```
  enable
  ```

- Enter global configuration mode[^2]:

  ```
  configure terminal
  ```

- Enter interface configuration mode:

  ```
  interface <interface> <ID>
  # i.e. interface gigabitethernet 0/0
  interface <interfaceID> # you dont need a space, i.e.
                          # interface gigabitethernet0/0
  # just "g" instead of "gigabitethernet" also works:
  # interface g0/0
  ```

- To change the hostname:

  ```
  hostname <name>
  ```

- Enable password (GCM):

  ```
  enable password <password>
  ```

- Enable an MD5 encrypted password (GCM):

  ```
  enable secret <password>
  ```

- Enable password encryption (GCM):

  ```
  service password-encryption
  ```

- Ping a host:

  ```
  ping <host>
  ```

- Show the ARP table:

  ```
  arp -a # Win/Mac/Linux
  show arp # Cisco IOS
  ```

- Show MAC address table:

  ```
  show mac address-table
  ```

- Clear dynamic entries in the MAC address table:

  ```
  clear mac address-table dynamic # all
  clear mac address-table dynamic interface <interface> # for an interface
  clear mac address-table dynamic address <address> # for an address
  ```
  
- Show configs:

  ```
  show running-config
  show startup-config
  ```

- Run a command from GCM in PEM:

  ```
  do <command>
  ```

- Remove a command from configuration:

  ```
  no <command>
  ```

- Write running configuration to memory, all three of these are
   interchangeable:

  ```
  write memory
  write
  copy running-config startup-config
  ```
  
- Show interface statuses + IP addresses:

  ```
  show ip interface brief
  ```

- To set an IP address:

  ```
  ip address <IP-Address> <subnet mask>
  # i.e. ip address 10.255.255.254 255.0.0.0
  ```

- To enable an interface, should apply this to router interfaces since they are
  shutdown by default:

  ```
  no shutdown
  ```

- To show L1, L2 and error information about an interface:

  ```
  show interfaces <interface>
  ```

- Show to show L1 and L2 statuses of interfaces (and description):

  ```
  show interfaces description
  ```

- To add a description to an interface:

  ```
  # int <interface>
  description <string>
  # i.e. description ## to SW2 ##
  ```

- Useful command to check switch interfaces (duplex, speed, type, vlan etc.),
  this command DOES NOT work on routers, only switches:

  ```
  show interfaces status
  ```

- To configure an interface's speed and duplex:

  ```
  # conf t
  # int <interface>
  speed <speed>
  duplex <auto/full/half>
  # description ## to <device> ##
  # sh int st
  ```

- To configure multiple interfaces at once:

  ```
  # interface range f0/5 - 12
  interface range <int# - #>
  # description ## not in use ##
  # shutdown
  ```
  
  The interface numbers don't have to be in order, for example:

  ```
  int range f0/5 - 6, f0/9 - 12
  # no shut
  ```

- To show the routing table:

  ```
  show ip route
  ```

- To configure a static route with next hop IP address:

  ```
  ip route <ip-address> <netmask> <next-hop>
  # i.e. ip route 192.168.4.0 255.255.255.0 192.168.13.3
  # routes traffic to the 192.168.4.0/24 subnet via 192.168.13.3
  ```
  
  You can also configure a route with an exit interface:

  ```
  ip route <ip-address> <netmask> <exit-interface>
  # i.e. ip route 192.168.4.0 255.255.255.0 g0/0
  # routes traffic to the 192.168.4.0/24 subnet via the g0/0 interface
  ```

  You can specify both as well:

  ```
  ip route <ip-address> <netmask> <exit-interface> <next-hop>
  ```

- To configure a default route on a Cisco router:

  ```
  ip route 0.0.0.0 0.0.0.0 <next-hop>
  ```

- To output lines that include a particular string, use `| include`, for
  example:

  ```
  # show static routes configurations
  show running-config | include ip route
  ```

  You can easily remove these configs with `no <configuration>`.

- To configure a custom MAC address:

  ```
  mac-address <mac address>
  # mac-address 1234.1234.1234
  ```

- To show the default gateway, MAC and IP addresses on a Windows machine:

  ```
  ipconfig /all
  ```

- To view the entire file starting from `LINE`:

  ```
  sh run | begin <LINE>
  ```

- To view lines that do not include a `LINE`:

  ```
  sh run | exclude <LINE>
  ```

- To view the sections containing a `LINE`:

  ```
  sh run | section <LINE>
  ```

- You can back up your configurations, to copy locally:

  ```
  # copy run flash:my-config
  copy run flash:<file-name>
  ```

- To erase your startup-config:

  ```
  wr erase
  ```

- To show the contents of a file, use `more`, for example:

  ```
  more flash:my-config
  ```

- To reboot a device:

  ```
  reload
  ```

- To go back to UEM from PEM:

  ```
  disable
  ```

- To go to UEM from any level:

  ```
  end
  ```

- To show VLAN information:

  ```
  show vlan brief
  ```

- To configure VLAN access ports:

  ```
  # interface range g0/1 - 3
  switchport mode access
  switchport access vlan <number>
  ```

- To configure a trunk port:

  ```
  # interface g0/0
  switchport trunk encapsulation dot1q # ISL/dot1q issue
  switchport mode trunk
  ```

- To show trunk ports information:

  ```
  show interfaces trunk
  ```

- To modify allowed VLANs on a trunk:

  ```
  switchport trunk allowed vlan 10,30 # reset list first, set list
  switchport trunk allowed vlan add 20 # add to the existing list
  switchport trunk allowed vlan remove 20 # remove from the existing list
  switchport trunk allowed vlan all # allow all VLANs
  switchport trunk allowed vlan none # allow no VLANs
  switchport trunk allowed vlan except 1-5,10 # allow all VLANs except 1-5, 10
  ```

- To change the default VLAN (for better security, change it to an unused
  VLAN):

  ```
  switchport trunk native vlan 1001
  ```

- To create/select a VLAN (created automatically when assigned to interfaces):

  ```
  vlan <number>
  ```

- To name a VLAN:

  ```
  name <NAME>
  # name Engineering
  ``` 

- To show the IP address and the subnet mask on Linux:

  ```
  ifconfig
  ```

  To show the default gateway:

  ```
  ip route
  ```

[^1]: PEM from here on 
[^2]: GCM from here on 