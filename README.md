# ClamAV
-----------------

An ansible role that installs, configures and sets up scheduled tasks for managing clamav.

A [useful reference](http://blog.clamav.net/2016/03/configuring-on-access-scanning-in-clamav.html) for deciding how you want to configure clamav for your particular environment

Limitations
-----------

1. On-access protection does not seem to work against mount points e.g. you can add '/' as a mount point but it won't detect /var/eicar.txt as a virus; if I add /var as a folder on-access scanning works fine.
2. On-access scanning requires the clamav daemon to run as root

Requirements
------------

At the moment this has only been tested with Centos 7 servers.

Quickstart
----------

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

This gets you clamav running, a daily scan and database updates.

Configuration variables
-----------------------

For more low-level configuration details the `clamav_clamd_config` and `clamav_freshclam_config` variables can be overridden; some typical default settings are included below.

```
clamav_clamd_config:
  PidFile: '{{ clamav_run_dir }}/clamd.pid'
  DatabaseDirectory: '{{ clamav_data_dir }}'
  TemporaryDirectory: '/var/tmp'
  LocalSocket: '{{ clamav_run_dir }}/clamd.sock'
  FixStaleSocket: 'yes'
  TCPSocket: '{{ clamav_clamd_port }}'
  TCPAddr: 127.0.0.1
  MaxConnectionQueueLength: 30
  MaxThreads: 50
  ReadTimeout: 300
  User: '{{ clamav_user }}'
  AllowSupplementaryGroups: 'yes'
  ScanPE: 'yes'
  ScanELF: 'yes'
  DetectBrokenExecutables: 'yes'
  ScanOLE2: 'yes'
  ScanMail: 'yes'
  ScanArchive: 'yes'
  ArchiveBlockEncrypted: 'no'
  LogFile: /var/log/clamd.log
  LogFileMaxSize: 0
  LogTime: 'yes'
  LogSyslog: 'yes'
  LogRotate: 'no'
  OnAccessMaxFileSize: 5242880
  OnAccessPrevention: 'no'
  OnAccessExcludeUID: 0
clamav_freshclam_config:
  DatabaseDirectory: '{{ clamav_data_dir }}'
  DatabaseOwner: '{{ clamav_user }}'
  DatabaseMirror: database.clamav.net
  UpdateLogFile: /var/log/freshclam.log
  LogSyslog: 'yes'
  LogTime: 'yes'
  LogRotate: 'no'
```

Of note are the clamav_clamd_config.LogFile key; this is used in the ansible role so if your overriding these variables then this __must__ exist otherwise you'll get variable 'undefined' errors on execution.

When overriding these variables remember that ansible by default will __not__ merge hashes; this means to tweak the LogRotate value in `clamav_freshclam_config` above you would need to duplicate the whole block as and then make your change as per below:
```
clamav_freshclam_config:
  DatabaseDirectory: '{{ clamav_data_dir }}'
  DatabaseOwner: '{{ clamav_user }}'
  DatabaseMirror: database.clamav.net
  UpdateLogFile: /var/log/freshclam.log
  LogSyslog: 'yes'
  LogTime: 'yes'
  LogRotate: 'yes'
```

License
-------

MIT

Author Information
------------------

This role was created in 2017 by Caoimhin Graham on behalf of [Kainos Software](https://www.kainos.com)
