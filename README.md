Fail2ban
=========

A role that installs and configures clamav.

A [useful reference](http://blog.clamav.net/2016/03/configuring-on-access-scanning-in-clamav.html) for deciding how you want to configure clamav for your particular environment

Limitations
=========

1. On-access protection does not seem to work against mount points e.g. you can add '/' as a mount point but it won't detect /var/eicar.txt as a virus; if I add /var as a folder on-access scanning works fine.
2. On-access scanning requires the clamav daemon to run as root

Requirements
------------

At the moment this has only been tested with Centos 7 servers.

1. Typical Role Setup
-----------------

Simply configure the default role variables as below to enable on-access scanning and setup a daily scan via crontab.

```
clamav_onaccess_scanning: true
clamav_daily_cron_enabled: true
clamav_onaccess_conf:
  mount_path: []
  include_path:
   - '/home'
   - '/opt'
  exclude_path: []
```

You can also modify the `clamav_clamd_config` variable if you need to change any other scanning configuration parameters; similarly `clamav_freshclam_config` can use used to configure database updates.

License
-------

MIT

Author Information
------------------

This role was created in 2017 by Caoimhin Graham on behalf of [Kainos Software](https://www.kainos.com)
