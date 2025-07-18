---
title: "tcpdump: Packet Analyzer"
description: "Command-line packet analyzer for capturing and displaying network traffic."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["tcpdump", "Network Monitoring",]
---

Basic traffic capture:

```jsx
sudo tcpdump -i eth0
```

Write packet capture to a pcap file:

```jsx
sudo tcpdump -i eth0 -w capture.pcap
```

Read from file and print contents on the screen:

```jsx
tcpdump -r capture.pcap
```

Limit number of packets captured to 100:

```jsx
sudo tcpdump -i eth0 -c 100
```

Print packets in ASCII — useful for seeing HTTP requests and simple text payloads:

```jsx
sudo tcpdump -i eth0 -A
```

Print packets in hex and ASCII side-by-side:

```jsx
sudo tcpdump -i eth0 -X
```