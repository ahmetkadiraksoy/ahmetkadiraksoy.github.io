---
title: "Zeek: Network Security Monitoring Tool"
description: "How Zeek helps in large-scale network visibility."
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

Test the installation:

```jsx
/opt/zeek/bin/zeek --version
```

> You should get an output similar to:<br>
> /opt/zeek/bin/zeek version 7.2.1

<br>

# Configuration:

> Zeek, by default, saves the logs in tab-separated values (TSV) format.

To record logs in JSON format, edit the 'local.zeek' file by running:

```jsx
sudo nano /opt/zeek/share/zeek/site/local.zeek
```

Add the following entry at the end of the file and save changes:

```jsx
@load policy/tuning/json-logs
```

> Saving Zeek output in JSON format enables efficient, structured, and automated forensic analysis by making the data easily parsable, searchable, and integrable with modern security tools.

<br>

# Execution:

Run Zeek on your network interface (e.g., eth0):

```jsx
sudo /opt/zeek/bin/zeek -i eth0
```

> With this command, Zeek will run in standalone mode and by default will save logs in the current working directory. If you want to specify a different log directory, you can use the -l flag.

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

To initiate and start the deamon for the first time:

```jsx
sudo /opt/zeek/bin/zeekctl deploy
```

> You may also use the 'deploy' parameter for safe and proper redeployment, especially after config or script changes.

In future executions, run the following to start Zeek as a daemon:

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

> zeekctl organizes logs into daily timestamped directories under the specified logs folder. Each day’s logs are stored separately, making it easy to review historical data. Additionally, zeekctl automatically compresses older logs (usually with gzip) to save disk space.

To ensure auto-start upon reboot, create a custom service file for Zeek:

```jsx
sudo nano /etc/systemd/system/zeekctl.service
```

Paste the following content into the file:

```jsx
[Unit]
Description=Zeek Network Security Monitor
After=network.target

[Service]
Type=forking
ExecStart=/opt/zeek/bin/zeekctl start
ExecStop=/opt/zeek/bin/zeekctl stop
ExecReload=/opt/zeek/bin/zeekctl deploy
User=root
WorkingDirectory=/opt/zeek
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Reload systemd to recognize the new service:

```jsx
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
```

Enable the service to run on boot:

```jsx
sudo systemctl enable zeekctl.service
```

<br>

# jq Command:

The [jq](https://jqlang.org) command is a powerful command-line JSON processor that is ideal for filtering and extracting data from Zeek logs saved in JSON format. To use jq, you simply pipe the JSON log file into the command and apply filters to extract the fields or records of interest. jq’s query syntax allows for chaining operations, handling nested fields, and formatting output, making it an essential tool for efficient and precise analysis of Zeek’s JSON logs.

<br>

# Installation:

jq is installed by default in many Linux distributions. However, you may install it if it missing with the following command:

```jsx
sudo apt install jq
```

<br>

# Usage:

Here is a sample content of conn.log file generated by Zeek from [Zeek page.](https://docs.zeek.org/en/master/logs/conn.html)
```jsx
{
  "ts": 1591367999.305988,
  "uid": "CMdzit1AMNsmfAIiQc",
  "id.orig_h": "192.168.4.76",
  "id.orig_p": 36844,
  "id.resp_h": "192.168.4.1",
  "id.resp_p": 53,
  "proto": "udp",
  "service": "dns",
  "duration": 0.06685185432434082,
  "orig_bytes": 62,
  "resp_bytes": 141,
  "conn_state": "SF",
  "missed_bytes": 0,
  "history": "Dd",
  "orig_pkts": 2,
  "orig_ip_bytes": 118,
  "resp_pkts": 2,
  "resp_ip_bytes": 197,
  "ip_proto": 17
}
{
  "ts": 1591367999.430166,
  "uid": "C5bLoe2Mvxqhawzqqd",
  "id.orig_h": "192.168.4.76",
  "id.orig_p": 46378,
  "id.resp_h": "31.3.245.133",
  "id.resp_p": 80,
  "proto": "tcp",
  "service": "http",
  "duration": 0.25411510467529297,
  "orig_bytes": 77,
  "resp_bytes": 295,
  "conn_state": "SF",
  "missed_bytes": 0,
  "history": "ShADadFf",
  "orig_pkts": 6,
  "orig_ip_bytes": 397,
  "resp_pkts": 4,
  "resp_ip_bytes": 511,
  "ip_proto": 6
}
```

To print a specific field (e.g. 'ts' field) from each record:

```jsx
cat conn.log | jq '.ts'
```

The above command would yield the following output:

```jsx
1591367999.305988
1591367999.430166
```

If the field name contains a period '.', make sure to enclose the field name with double quotations:

```jsx
cat conn.log | jq '."id.orig_h"'
```

## Select/filter objects:

The jq tool makes it easy to filter and extract specific pieces of JSON data from a file or stream. Here’s a breakdown of three example commands that demonstrate how to use jq to select only the data you care about.

The following command filters the JSON input and returns only the objects where the proto field is exactly "udp".

```jsx
cat conn.log | jq 'select(.proto == "udp")'
```

If conn.log contains a JSON array, the following command iterates over each element (.[]) and selects those where the age field is greater than 30.

```jsx
cat conn.log | jq '.[] | select(.age > 30)'
```

The following command returns objects where the name is either "Alice" or "Bob".

```jsx
cat conn.log | jq 'select(.name == "Alice" or .name == "Bob")'
```

## Working with arrays:

If conn.log contains a JSON array, the following command will output each element of the array as a standalone JSON object.

```jsx
cat conn.log | jq '.[]'
```

The following command assumes the JSON has a top-level key called items that holds an array. It loops through each item and prints the value of the name field.

```jsx
cat conn.log | jq '.items[] | .name'
```

To use 'any' keyword with multiple fields:

```jsx
cat conn.log | jq 'select([.src_ip, .dst_ip] | any(. == "10.0.0.1" or . == "192.168.1.5"))'
```

Get unique values:

```jsx
cat conn.log | jq '.[].category' | sort | uniq
```

Pretty print JSON:

```jsx
cat conn.log | jq '.'
```

Minify JSON:

```jsx
cat conn.log | jq -c '.'
```
