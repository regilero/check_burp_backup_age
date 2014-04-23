check_burp_backup_age
=====================

Nagios local check for burp's timestamps backup age.

Always check github for last version: https://github.com/regilero/check_burp_backup_age

Burp backup project: (http://burp.grke.org/)

Author: regilero (https://github.com/regilero)

Usage
-----

<pre>
./check_burp_backup_age.py -h
usage: check_burp_backup_age [-h] [-v] -H [HOSTNAME] [-d [DIRECTORY]]
                             [-w [WARNING]] [-c [CRITICAL]]

Local check, Check freshness of last backup for a given host name. Running on
the backup server this program will check the timestamp file of the last
backup for a given host and get the age of this last successful run. This age
is then compared to thresolds to generate alerts.

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program version
  -H [HOSTNAME], --hostname [HOSTNAME]
                        hostname (directory name for burp) [default: None]
  -d [DIRECTORY], --directory [DIRECTORY]
                        base directory path for backups (where are the
                        backups?) [default: /backups]
  -w [WARNING], --warning [WARNING]
                        Warning thresold, time in minutes before going to
                        warning [default: 1560]
  -c [CRITICAL], --critical [CRITICAL]
                        Critical thresold, time in minutes before going to
                        critical [default: 1800]

Note that this is a local check, running on the backup server. So the hostname
argument is not used to perform any distant connection.
</pre>

Important
---------

  * This is a **local check**, so use it with **NRPE** or **check_by_ssh** or such tools.
  * warning and critical thresolds are expressed in **minutes**, the default values are:
    * warning: 26 hours so 1560 minutes
    * critical: 30 hours so 1800 minutes

Examples
--------


    ~# ./check_burp_backup_age.py  -d /data/burp -H foo.example.com
    BURP OK - Last backup is fresh enough: 0 day(s) 21 hour(s) 23 minute(s) (1283<1560)

    ~# ./check_burp_backup_age.py  -d /data/burp -H foo.example.com -w 1200 -c 1500
    BURP WARNING - Last backup starts to get old: 0 day(s) 21 hour(s) 23 minute(s) (1283>=1200)

    ~# ./check_burp_backup_age.py  -d /data/burp -H foo.example.com -w 800 -c 1200
    BURP CRITICAL - Last backup is too old: 0 day(s) 21 hour(s) 23 minute(s) (1283>=1200)

    ~# ./check_burp_backup_age.py  -d /data/burp -H bar.example.com
    BURP CRITICAL - Host backup directory /data/burp/bar.example.com does not exists

    :~# ./check_burp_backup_age.py  -d /backups -H bar.example.com
    BURP CRITICAL - Base backup directory /backups does not exists
