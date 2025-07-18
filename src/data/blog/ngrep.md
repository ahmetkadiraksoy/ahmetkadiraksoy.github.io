---
title: "ngrep: Packet Analyzer"
description: "Network packet analyzer with the functionality of grep with network packet sniffing."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["ngrep", "Network Monitoring",]
---

## Capture all HTTP GET requests

Useful to see which resources clients are requesting in cleartext HTTP:

```jsx
sudo ngrep -d eth0 "GET" tcp port 80
```

## Monitor HTTP Host headers

Helps identify target domains or virtual hosts accessed by clients:

```jsx
sudo ngrep -d eth0 -W byline "Host:" tcp port 80
```

## Watch for basic authentication credentials

Useful for catching base64-encoded credentials in HTTP headers:

```jsx
sudo ngrep -d eth0 "Authorization: Basic" tcp port 80
```

## Monitor DNS queries

Show DNS requests to detect suspicious or unusual domains:

```jsx
sudo ngrep -d eth0 -q '.*' udp port 53
```

## Capture FTP login commands

Check for plaintext FTP credentials:

```jsx
sudo ngrep -d eth0 "USER|PASS" tcp port 21
```

## Find Telnet login attempts

Identify login attempts on insecure Telnet connections:

```jsx
sudo ngrep -d eth0 "login" tcp port 23
```

## Watch all outgoing POST requests (possible data exfiltration)

Useful to track data uploads to web servers:

```jsx
sudo ngrep -d eth0 "POST" tcp port 80
```

## Filter traffic to or from a specific host

Monitor all payloads to and from a single IP address:

```jsx
sudo ngrep -d eth0 -W byline "." host 192.168.1.50
```

## Save ngrep output to a file for later review

Logs suspicious matches to a text file:

```jsx
sudo ngrep -d eth0 "password" tcp port 80 > suspicious_traffic.txt
```