---
title: "editcap: Capture File Editing Utility"
description: "Command-line utility to manipulate pcap files."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["editcap", "Network Monitoring",]
---

## What is editcap?

- editcap is a command-line tool (part of Wireshark) used to edit and manipulate pcap or pcapng capture files.

## Why useful for cybersecurity?

- Work with large incident response captures more easily.
- Focus on suspicious time windows or connections only.
- Remove irrelevant or noisy packets.
- Share smaller, sanitized pcaps for reports or collaboration.

## Split a large pcap into smaller files

Splits into multiple files with 1,000 packets each:

```jsx
editcap -c 1000 bigfile.pcap smaller.pcap
```

## Remove (drop) specific packets

Removes packets 5 to 10 from the original capture:

```jsx
editcap -r original.pcap cleaned.pcap 5-10
```

## Extract a range of packets

Saves only packets 1 to 100 into new.pcap:

```jsx
editcap -r original.pcap new.pcap 1-100
```

## Cut packets by time window

Keeps only packets captured between specified start (-A) and end (-B) times:

```jsx
editcap -A "2024-06-28 10:00:00" -B "2024-06-28 10:05:00" bigfile.pcap shortfile.pcap
```

## Convert capture file format

Converts from pcapng to classic pcap format:

```jsx
editcap -F pcap original.pcapng converted.pcap
```

## Truncate packets to a maximum length

Truncates packets to 100 bytes, useful to remove payload content:

```jsx
editcap --snaplen 100 capture.pcap truncated.pcap
```