# Artifact analysis

For example:

* Identifying and fingerprinting devices, operating systems, and running services.
* Analysing memory dumps to discover traces of ransomware.
* Swap analysis.
* Password dumping.
* Examining a Firefox browser and Gmail artifacts.

## Common Linux forensic artifacts

### Persistence related

Persistence helps a malware persist on a machine after startup or any other mechanism that would stop a malware 
from running.

Cron jobs:

    cat /etc/crontab

A list of services can be found in the `/etc/init.d` directory:

    ls /etc/init.d/

`.bashrc`:

    cat ~/.bashrc

System wide settings:

    /etc/bash.bashrc
    /etc/profile

### Evidence of execution

Sudo execution history:

    cat /var/log/auth.log* |grep -i COMMAND|tail

Bash history:

    cat ~/.bash_history

File access using vim:

    cat ~/.viminfo

### Log files

`Syslog` contains messages that are recorded by the host about system activity:

    cat /var/log/syslog* | head

Auth logs have information about users and authentication-related logs:

    cat /var/log/auth.log* |head

`/var/log` contains logs for third-party applications, for example `Apache2` logs:

    ls /var/log/apache2/

