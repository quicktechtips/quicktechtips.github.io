---
layout: default
title: Apache Timeouts and PHP
nav_order: 4
---

# Fixing Gateway Timeouts in Apache Caused by a Long-Running PHP Script

## Symptom

While executing a long-running PHP script, the user/admin will see a message in the browser:
"Gateway Timeout: The gateway did not receive a timely response from the upstream server or application."

## Fix

- The file locations and commands below assume the server is a Red Hat-based distrobution.
- If this is a PHP script that is timing out, then figure out which settings file is being loaded.
  - Create a `phpinfo.php` file to serve in a web browser. Required contents shown at the bottom of this page.
- Note the `Loaded Configuration File` line. It is likely `/etc/php.ini`.
- Note the line for `max_execution_time`--we want this increased. Default is 30 seconds.
- Open the PHP configuration file found previously. Change the value for `max_execution_time` to a higher value (units are seconds):
  - Example: `max_execution_time = 600`
- Open httpd configuration file (`/etc/httpd/conf/httpd.conf`)
- Add this line at the bottom, where the time correlates appropriately to the PHP value: `TimeOut 600`
- Restart Apache and PHP (assuming PHP-FPM is being used): `systemctl restart httpd php-fpm`


Contents of `phpinfo.php`

```
<?php
phpinfo();
?>
```

