---
title: "tcpflow: Network Traffic Analyzer and Debugger"
description: "Powerful command-line tool for analyzing network traffic."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["tcpflow", "Network Monitoring",]
---

## Capture TCP flows on eth0, save reconstructed streams to files (saves in current folder)

```jsx
sudo tcpflow -i eth0
```

## Capture only HTTP traffic (port 80) (saves in current folder)

```jsx
sudo tcpflow -i eth0 port 80
```

## Read from an existing pcap file (saves in current folder)

```jsx
tcpflow -r capture.pcap
```

## Specify output directory for flow files

```jsx
tcpflow -i eth0 -o /tmp/flows
```