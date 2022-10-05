---
layout: default
title: Apache Ownership and Permissions
nav_order: 2
---

# Setting Proper Permissions and Ownership for Apache Web Documents

## Notes

- Web documents should be owned by `apache:apache`
- Web users should be members of the `apache` group
- Permissions should be 775 on `/var/www/html` (Red Hat based distro)

## Example

```
usermod -aG apache username
chown -R apache:apache /var/www/html/
chmod -R 775 /var/www/html/
```
