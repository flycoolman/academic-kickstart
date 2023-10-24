---
title: 'Linux Bonding'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: It describes how to create bonding on Ubuntu.
authors:
- admin
tags:
- Linux
- System
categories:
- Linux
- System
date: "2023-08-04T00:00:00Z"
lastmod: "2023-10-24T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  caption: ''
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


## Install modules
    sudo modprobe bonding
    lsmod | grep bond
    sudo apt-get install ifenslave


## Temporary testing
To create a bond interface composed of the first two physical NCs in your system, issue the below command. However this method of creating bond interface is ephemeral and **DOES NOT** survive system reboot.  

    sudo ip link add bond0 type bond mode 802.3ad
    sudo ip link set eth0 master bond0
    sudo ip link set eth1 master bond0

## Permanant configuration
To create a permanent bond interface, use the method to manually edit interfaces configuration file. Details about the bond interface can be obtained by displaying the content of the below kernel file using cat command as shown.

    nano /etc/netplan/01-netcfg.yaml

    cat /etc/netplan/01-netcfg.yaml

### Access port
    root@R107-U04:/home/ba# cat /etc/netplan/01-netcfg.yaml
    # This file describes the network interfaces available on your system
    # For more information, see netplan(5).
    network:
      bonds:
        bond0:
          dhcp4: yes
          interfaces:
          - eno2
          - eno3
          parameters:
            lacp-rate: fast
            mii-monitor-interval: 100
            min-links: 1
            mode: 802.3ad
            transmit-hash-policy: layer3+4
      version: 2
      renderer: networkd
      ethernets:
        eno1:
          dhcp4: yes
          dhcp-identifier: mac
        eno2:
          dhcp4: false
        eno3:
          dhcp4: false

### Access port with route

    root@R107-U04:/home/ba# cat /etc/netplan/01-netcfg.yaml 
    # This file describes the network interfaces available on your system
    # For more information, see netplan(5).
    network:
      bonds:
        bond0:
          dhcp4: false
          interfaces:
          - eno2
          - eno3
          addresses: [10.30.15.168/26]
          routes:
          - to: 10.50.101.0/24
            via: 10.30.15.129
          parameters:
            lacp-rate: fast
            mii-monitor-interval: 100
            min-links: 1
            mode: 802.3ad
            transmit-hash-policy: layer3+4    
      version: 2
      renderer: networkd
      ethernets:
        eno1:
          dhcp4: yes
          dhcp-identifier: mac
        eno2:
          dhcp4: false
        eno3:
          dhcp4: false

### Trunk port

    network:
      bonds:
        bond0:
          dhcp4: false
          interfaces:
          - eno2
          - eno3
          parameters:
            lacp-rate: fast
            mii-monitor-interval: 100
            min-links: 1
            mode: 802.3ad
            transmit-hash-policy: layer3+4
        
      version: 2
      renderer: networkd
      ethernets:
        eno1:
          dhcp4: yes
          dhcp-identifier: mac
        eno2:
          dhcp4: false
        eno3:
          dhcp4: false
        
      vlans:
        bond0.1021:
          addresses: [192.168.100.168/24]
          dhcp4: false
          id: 1021
          link: bond0
          routes:
          - scope: link
            table: 11
            to: 10.30.15.128/26
          - table: 11
            to: 10.50.101.0/24
            via: 10.30.15.129
          routing-policy:
          - from: 10.30.15.168
            priority: 1
            table: 11
        bond0.1022:
          critical: false
          dhcp-identifier: mac
          dhcp4: true
          dhcp4-overrides:
            send-hostname: false
            use-hostname: false
          dhcp6: false
          id: 1022
          link: bond0

## Change Transmit Hash
If the Transmit Hash Policy is changed, to reboot the Ubuntu for the new policy taking effect.

### Before rebooting

    root@R107-U04:/home/ba# cat /proc/net/bonding/bond0
    Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)
        
    Bonding Mode: IEEE 802.3ad Dynamic link aggregation
    Transmit Hash Policy: layer2 (0)
    MII Status: up
    MII Polling Interval (ms): 100
    Up Delay (ms): 200
    Down Delay (ms): 200
    Peer Notification Delay (ms): 0
        
    802.3ad info
    LACP rate: slow
    Min links: 0
    Aggregator selection policy (ad_select): stable
    System priority: 65535
    System MAC address: 78:ac:44:9e:52:f2
    Active Aggregator Info:
            Aggregator ID: 1
            Number of ports: 2
            Actor Key: 15
            Partner Key: 3
            Partner Mac Address: 00:01:02:03:04:05
        
    Slave Interface: eno3
    MII Status: up
    Speed: 10000 Mbps
    Duplex: full
    Link Failure Count: 9
    Permanent HW addr: 78:ac:44:9e:52:f2
    Slave queue ID: 0
    Aggregator ID: 1
    Actor Churn State: none
    Partner Churn State: none
    Actor Churned Count: 0
    Partner Churned Count: 0
    details actor lacp pdu:
        system priority: 65535
        system mac address: 78:ac:44:9e:52:f2
        port key: 15
        port priority: 255
        port number: 1
        port state: 61
    details partner lacp pdu:
        system priority: 127
        system mac address: 00:01:02:03:04:05
        oper key: 3
        port priority: 127
        port number: 3
        port state: 63
        
    Slave Interface: eno2
    MII Status: up
    Speed: 10000 Mbps
    Duplex: full
    Link Failure Count: 12
    Permanent HW addr: 78:ac:44:9e:52:f1
    Slave queue ID: 0
    Aggregator ID: 1
    Actor Churn State: churned
    Partner Churn State: churned
    Actor Churned Count: 1
    Partner Churned Count: 1
    details actor lacp pdu:
        system priority: 65535
        system mac address: 78:ac:44:9e:52:f2
        port key: 15
        port priority: 255
        port number: 2
        port state: 61
    details partner lacp pdu:
        system priority: 127
        system mac address: 00:01:02:03:04:05
        oper key: 3
        port priority: 127
        port number: 32769
        port state: 63


### Rebooting

    root@R107-U04:/# shutdown -r now

### After rebooting

    ba@R107-U04:~$ cat /proc/net/bonding/bond0
    Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)
      
    Bonding Mode: IEEE 802.3ad Dynamic link aggregation
    Transmit Hash Policy: layer3+4 (1)
    MII Status: up
    MII Polling Interval (ms): 100
    Up Delay (ms): 0
    Down Delay (ms): 0
    Peer Notification Delay (ms): 0
      
    802.3ad info
    LACP rate: fast
    Min links: 1
    Aggregator selection policy (ad_select): stable
      
    Slave Interface: eno3
    MII Status: up
    Speed: 10000 Mbps
    Duplex: full
    Link Failure Count: 0
    Permanent HW addr: 78:ac:44:9e:52:f2
    Slave queue ID: 0
    Aggregator ID: 1
    Actor Churn State: none
    Partner Churn State: none
    Actor Churned Count: 0
    Partner Churned Count: 0
      
    Slave Interface: eno2
    MII Status: up
    Speed: 10000 Mbps
    Duplex: full
    Link Failure Count: 0
    Permanent HW addr: 78:ac:44:9e:52:f1
    Slave queue ID: 0
    Aggregator ID: 1
    Actor Churn State: none
    Partner Churn State: none
    Actor Churned Count: 0
    Partner Churned Count: 0

## To check the packets

    root@R107-U04:/# tcpdump -i eno2 icmp
    root@R107-U04:/# tcpdump -i eno3 icmp 



## Links

[How to Configure Network Bonding or Teaming in Ubuntu](https://www.tecmint.com/configure-network-bonding-teaming-in-ubuntu/)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌
