---
layout: default
title: Fix Time Skew in Linux
nav_order: 6
---

# How to Fix a Time Skew in Linux

## Issue

- A Linux computer/server is reporting an incorrect time. This issue can affect communication with other servers (especially in a domain environment).

## Possible Solution

- The `chronyd` service references NTP servers listed in `/etc/chrony.conf`. Check this file to see what time servers are being used--the steps below assume a bad NTP server is being referenced (like an old domain controller).
- Change the IP address/server name in `/etc/chrony.conf` to an acceptable server address in your environment.
- Restart the `chronyd` service: `systemctl restart chronyd`
- Check the result, look for a corrected time and sync status with `timedatectl`

```
$ timedatectl
    Local time: Fri 2022-02-11 10:47:29 EST
    Universal time: Fri 2022-02-11 15:47:29 UTC
    RTC time: Fri 2022-02-11 15:47:30
    Time zone: America/New_York (EST, -0500)
    NTP enabled: yes
    NTP synchronized: yes
    RTC in local TZ: no
    DST active: no
```
