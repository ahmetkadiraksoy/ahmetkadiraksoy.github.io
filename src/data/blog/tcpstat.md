---
title: "tcpstat: Network Interface Statistics Monitoring Utility"
description: "Command-line utility that reports network statistics."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["tcpstat", "Network Monitoring",]
---

## **How is tcpstat typically used?**

- It is **usually used for short-term, on-demand monitoring**, such as:
    - Quickly diagnosing spikes in traffic.
    - Checking throughput or packet rate on an interface.
    - Troubleshooting suspected attacks or performance issues in real time.
- You can save its output to a file and run it in the background with tools like nohup or screen, but that is not the default or primary design.

## When to use `tcpstat`

- You want a quick snapshot of TCP traffic rates without detailed packet inspection.
- You want lightweight monitoring (for example, on a server or embedded device).
- You need to analyze historical captures quickly.

# Setup:

sudo apt -y install tcpstat

# Commands:

## Basic capture on interface eth0 with default 1-second interval

```jsx
sudo tcpstat -i eth0
```

## Set a custom reporting interval (e.g., 5 seconds)

```jsx
sudo tcpstat -i eth0 -l 5
```

## Display a specific format: throughput, packet rate, average packet size

```jsx
sudo tcpstat -i eth0 -l 1 -o "Throughput: %b bytes/s, Pkts: %p pkts/s, Avg pkt size: %a bytes\n"
```

## Monitor only TCP traffic on port 443 (HTTPS)

```jsx
sudo tcpstat -i eth0 -l 1 "tcp port 443"
```

## Monitor UDP traffic only

```jsx
sudo tcpstat -i eth0 -l 1 "udp"
```

## Use a pcap file instead of live capture

```jsx
tcpstat -f capture_file.pcap -l 1
```

##