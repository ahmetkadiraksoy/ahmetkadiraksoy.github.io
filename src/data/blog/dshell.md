---
title: "Dshell: Network Forensic Analysis Framework"
description: "An extensible network forensic analysis framework."
pubDatetime: 2025-07-17T00:00:00Z
featured: true
tags: ["dshell", "Network Monitoring",]
---

## Install build tools and dependencies

```jsx
sudo apt install git build-essential libpcap-dev libffi-dev libgeoip-dev -y
```

## Clone Dshell

```jsx
git clone https://github.com/USArmyResearchLab/Dshell.git
cd Dshell
```

## Install Python dependencies

```jsx
pip3 install netifaces geoip2
```

## Install Dshell

```jsx
sudo python3 [setup.py](http://setup.py/) install
```

## Verify installation

```jsx
dshell --help
```
