---
title: "Intro to Zeek: Passive Network Monitoring"
description: How Zeek helps in large-scale network visibility
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: [zeek, network-forensics, tools]
---

[Zeek](https://zeek.org) is a powerful and flexible open-source network security monitoring platform. It passively analyzes network traffic to detect suspicious activity, policy violations, and other security-relevant events in real time. Unlike traditional intrusion detection systems, Zeek focuses on deep, protocol-level inspection and generates rich, structured logs that are ideal for threat hunting and incident response.

<br>

# Installation:

First, let's start by installing the necessary dependencies:

```jsx
sudo apt install -y curl gnupg
```

Add Zeek repository to your system:

```jsx
echo 'deb http://download.opensuse.org/repositories/security:/zeek/Debian_11/ /' | sudo tee /etc/apt/sources.list.d/zeek.list
curl -fsSL https://download.opensuse.org/repositories/security:zeek/Debian_11/Release.key | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/security_zeek.gpg
```

> This repository is intended for **Debian 11**. For other operating systems, please refer to the full list by clicking [here.](https://software.opensuse.org/download.html?project=security%3Azeek&package=zeek)

Update repository packages:

```jsx
sudo apt update
```

Install Zeek:

```jsx
sudo apt install zeek
```

Test Zeek:

```jsx
/opt/zeek/bin/zeek --version
```

> You should get an output similar to:<br>
> /opt/zeek/bin/zeek version 7.2.1

Run Zeek on your network interface:

```jsx
sudo /opt/zeek/bin/zeek -i eth0
```

> With this command, Zeek will run in standalone mode and by default will save logs in the current working directory. If you want to specify a different log directory, you can use the -l flag.

<br>

# Configuration:

To record logs in JSON format, edit the 'local.zeek' file by running:

```jsx
sudo nano /opt/zeek/share/zeek/site/local.zeek
```

Add the following entry at the end of the file and save changes:

```jsx
@load policy/tuning/json-logs
```

<br>

# zeekctl:

zeekctl (Zeek Control) is a command-line management tool for administering and managing Zeek deployments—especially in production environments.

You can use Zeek as a daemon—i.e., running in the background continuously to monitor your network traffic—you’ll want to configure and manage it via zeekctl. This setup is ideal if you’re integrating Zeek as part of a real-time network monitoring system.

Before we start, if you'd like to change log directory, edit the following line:

```jsx
sudo nano /opt/zeek/etc/zeekctl.cfg
```

Look for the following line, change accordingly, and save:

```jsx
LogDir = /opt/zeek/logs
```

To initiate and start the deamon:

```jsx
sudo /opt/zeek/bin/zeekctl deploy
```

To start Zeek as a daemon:

```jsx
sudo /opt/zeek/bin/zeekctl start
```

To check the status:

```jsx
sudo /opt/zeek/bin/zeekctl status
```

To stop the deamon:

```jsx
sudo /opt/zeek/bin/zeekctl stop
```

<br>

# jq Command

## Basic field access

```jsx
jq '.field'
jq '.parent.child'
jq '."field.with.dots"'
```

## Select/filter objects

```jsx
jq 'select(.status == "active")'
jq '.[] | select(.age > 30)'
jq 'select(.name == "Alice" or .name == "Bob")'
```

## Working with arrays

```jsx
jq '.[]'
jq '.items[] | .name'
```

## Multiple fields output

```jsx
jq '{name: .name, age: .age}'
jq '[.[] | {name: .name, age: .age}]'
```

## Check if a field is in a list (like IN)

```jsx
jq 'select(.color | IN("red", "blue", "green"))'
```

## Use any with multiple fields

```jsx
jq 'select([.src_ip, .dst_ip] | any(. == "10.0.0.1" or . == "192.168.1.5"))'
```

## Get unique values

```jsx
jq '.[].category' | sort | uniq
```

## Pretty print JSON

```jsx
jq '.'
```

## Minify JSON

```jsx
jq -c '.'
```
