---
layout: default
title: Disable User Account
nav_order: 5
---

# How to Disable a User Account in Red Hat-based Operating Systems

These steps canbe used to disable a user account without deleting it. The notes below are derived from Red Hat technical documentation:  https://access.redhat.com/solutions/57812

## Notes

- Disable a Linux user account: `chage -E0 USER`
- Using `passwd -l user` locks the use of `passwd` to authenticate the user, other authentication methods could still be used though (key-based, PAM).
- Setting the login shell to `/bin/nologin` or `/usr/sbin/nologin` only affects interactive logins--still allowing things like port forwarding.
- If worried about interfering with crontab jobs running, then changing the user's password and removing keys from `/home/USER/.ssh/authorized_keys` should suffice in the short-term
