---
title: "nmap: Network Scanning"
description: "Network Scanning tool used to discover hosts, services, and vulnerabilities on a computer network."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["nmap", "Network Monitoring",]
---

## Basic Host Discovery:

Useful for just discovering live hosts on a network without being intrusive.

```jsx
sudo nmap -sn 172.20.50.0/24
```
- -sn: Ping scan only (skip port scan)

## Full TCP Connect Scan (Stealth detection):

Less stealthy, uses full TCP handshake. Good for forensics if you want to confirm open ports without needing raw socket privileges.

```jsx
sudo nmap -sT 172.20.50.30
```
- -sT: TCP connect scan

## Stealth SYN Scan:

Default scan, more stealthy, half TCP handshake. Popular for detection evasion and to identify listening services without completing handshake.

```jsx
sudo nmap -sS 172.20.50.30
```
- -sS: SYN scan (half-open)

## Service and Version Detection:

Helps analysts identify vulnerable or suspicious versions.

```jsx
sudo nmap -sV 172.20.50.30
```
- -sV: Probe open ports to determine service/version info

## Aggressive Scan:

OS detection, version, scripts, traceroute.

```jsx
sudo nmap -A 172.20.50.30
```
- -A: Enables OS detection, version detection, script scanning, and traceroute

## Operating System Detection:

```jsx
sudo nmap -O 172.20.50.30
```
- -O: Enable OS detection

## Scan Specific Ports Only:

```jsx
sudo nmap -p 22,80,443 172.20.50.30
```
- -p: Specify a list of ports or ranges (e.g., 1-1000)