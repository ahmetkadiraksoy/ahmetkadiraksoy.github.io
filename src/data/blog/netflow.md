---
title: "NetFlow: Network Traffic Analyzer"
description: "NetFlow for effective network traffic monitoring and analysis."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["NetFlow", "Network Monitoring",]
---

# Installation:

Install nfdump and softflowd:

```jsx
sudo apt -y install nfdump softflowd
```

<br>

# Setup:

Create a directory to store NetFlow data:

```jsx
sudo mkdir -p /var/netflow
```

Start nfcapd to collect NetFlow data on port 9995 and run as a daemon:

```jsx
sudo nfcapd -l /var/netflow -p 9995 -D
```

- -D deamon mode
- -R Can be used to relay NetFlow packets to a different collector rather than locally.

Start softflowd on eth0, export to localhost:9995 using NetFlow v5:

```jsx
sudo softflowd -i eth0 -n 127.0.0.1:9995 -v 5 -t maxlife=60
```
- -n collector address and port
- -v 5 NetFlow version
- -t maxlife=60 specifies timeout and flow lifetime options (flow record exported at most every 60 seconds, even if active).

Verify that port 9995 is open and receiving UDP traffic:

```jsx
sudo netstat -anu | grep 9995
```

To capture all packets on the network, enable promiscuous mode on eth0:

```jsx
sudo ifconfig eth0 promisc
```

<br>

# nfdump Commands:

Show statistics for flows containing 172.20.50.30:

```jsx
nfdump -R /var/netflow/ "host 172.20.50.30" -s ip/bytes
```

List all available NetFlow files:

```jsx
nfdump -R /var/netflow/
```

Show top 10 source IP addresses by bytes:

```jsx
nfdump -R /var/netflow/ -s srcip/bytes -n 10
```

Show top 10 destination IP addresses by bytes:

```jsx
nfdump -R /var/netflow/ -s dstip/bytes -n 10
```

Show top 10 ports used (source ports):

```jsx
nfdump -R /var/netflow/ -s srcport/bytes -n 10
```

Show top 10 ports used (destination ports):

```jsx
nfdump -R /var/netflow/ -s dstport/bytes -n 10
```

Filter flows to/from a specific subnet:

```jsx
nfdump -R /var/netflow/ "net 192.168.1.0/24"
```

Filter flows within a specific time window:

```jsx
nfdump -R /var/netflow/ -t 2024/06/01.00:00-2024/06/01.23:59
```

Display only TCP flows:

```jsx
nfdump -R /var/netflow/ "proto tcp"
```

Display only UDP flows:

```jsx
nfdump -R /var/netflow/ "proto udp"
```

Display long, detailed output for each flow:

```jsx
nfdump -R /var/netflow/ -o long
```

Sort by packets instead of bytes:

```jsx
nfdump -R /var/netflow/ -s ip/packets
```
