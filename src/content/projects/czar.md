---
title: 'Czar'
description: 'Automated active reconnaissance and bug hunting tool for continuous security scanning'
pubDate: 'Sep 01 2020'
github: 'https://github.com/alpharaoh/czar'
---

A fully automated active reconnaissance and bug hunting tool designed to perform continuous security scanning on target domains. Combines established security tools with custom modules to identify vulnerabilities.

## System Overview

```
+------------------------------------------------------------------+
|                           CZAR                                    |
+------------------------------------------------------------------+
|                                                                   |
|   +-------------+     +-------------+     +-------------+         |
|   |   config    | --> |    main     | --> |   scanner   |         |
|   |   .py       |     |    .py      |     |   modules   |         |
|   +-------------+     +-------------+     +-------------+         |
|                              |                   |                |
|                              v                   v                |
|                       +-------------+     +-------------+         |
|                       |   scheduler |     |   results   |         |
|                       |  (24h loop) |     |   parser    |         |
|                       +-------------+     +-------------+         |
|                                                  |                |
|                                                  v                |
|                                           +-------------+         |
|                                           |    Slack    |         |
|                                           |   alerts    |         |
|                                           +-------------+         |
+------------------------------------------------------------------+
```

## Scan Pipeline

```
+----------------+
|  Target Domain |
+-------+--------+
        |
        v
+-------+--------+     +------------------+
| Subdomain      | --> | DNS Resolution   |
| Enumeration    |     | & Validation     |
+----------------+     +--------+---------+
                                |
        +-----------------------+-----------------------+
        |                       |                       |
        v                       v                       v
+-------+--------+     +--------+-------+     +--------+-------+
| Subdomain      |     |  Host Header   |     |   CVE Check    |
| Takeover Check |     |  Injection     |     |   (Known vulns)|
+-------+--------+     +--------+-------+     +--------+-------+
        |                       |                       |
        +-----------------------+-----------------------+
                                |
                                v
                       +--------+-------+
                       |  Vulnerability |
                       |    Found?      |
                       +--------+-------+
                                |
                 +--------------+--------------+
                 |                             |
                 v                             v
              [YES]                          [NO]
                 |                             |
                 v                             |
        +--------+-------+                     |
        | Generate Report|                     |
        | Send Slack     |                     |
        | Alert          |                     |
        +----------------+                     |
                 |                             |
                 +-----------------------------+
                                |
                                v
                       +--------+-------+
                       | Wait 24 hours  |
                       | (configurable) |
                       +--------+-------+
                                |
                                v
                         [Repeat scan]
```

## Vulnerability Detection

```
+------------------------------------------------------------------+
|                    Detection Modules                              |
+------------------------------------------------------------------+

+------------------+     +------------------+     +------------------+
| SUBDOMAIN        |     | HOST INJECTION   |     | CVE SCANNING     |
| TAKEOVER         |     |                  |     |                  |
+------------------+     +------------------+     +------------------+
|                  |     |                  |     |                  |
| Checks for:      |     | Checks for:      |     | Checks for:      |
| - Dangling DNS   |     | - Header manip   |     | - Known exploits |
| - Unclaimed      |     | - SSRF vectors   |     | - Version-based  |
|   services       |     | - Cache poison   |     |   vulns          |
| - Cloud misconf  |     |                  |     |                  |
+------------------+     +------------------+     +------------------+
```

## Setup & Usage

```
+------------------------------------------------------------------+
|  Installation                                                     |
+------------------------------------------------------------------+

$ git clone https://github.com/alpharaoh/czar
$ cd czar
$ chmod +x requirements.sh && ./requirements.sh
$ vim config.py    # Add targets & Slack webhook
$ python3 main.py

+------------------------------------------------------------------+
|  Recommended Deployment                                           |
+------------------------------------------------------------------+

   +-------------------+
   |   VPS Instance    |
   | (DigitalOcean/AWS)|
   +-------------------+
           |
           v
   +-------------------+
   |  tmux / screen    |
   |  (persistent)     |
   +-------------------+
           |
           v
   +-------------------+
   |  python3 main.py  |
   |  (runs 24/7)      |
   +-------------------+
```

## Tech Stack

- **Language:** Python 3 (98%)
- **Scripting:** Shell (2%)
- **Alerting:** Slack webhooks

## Note

This project is a work in progress with no intention to further develop. It was effective during its testing phase in September 2020, discovering multiple security vulnerabilities before development was halted.
