# Common Linux forensic artifacts

## Linux persistence mechanisms

Persistence helps a malware persist on a machine after startup or any other mechanism that would stop the malware 
from running.

### Cron Jobs

    cat /etc/crontab

### Service Startup

A list of services can be found in the `/etc/init.d` directory:

    ls /etc/init.d/

### .Bashrc

    cat ~/.bashrc

System wide settings:

    /etc/bash.bashrc
    /etc/profile

## Evidence of execution

### Sudo execution history

    cat /var/log/auth.log* |grep -i COMMAND|tail

### Bash history

    cat ~/.bash_history

### File access using vim

    cat ~/.viminfo

## Log files

### Syslog

Contains messages that are recorded by the host about system activity:

    cat /var/log/syslog* | head

### Auth Logs

Information about users and authentication-related logs:

    cat /var/log/auth.log* |head

### Third-party logs

`/var/log` contains logs for third-party applications, for example Apache2 logs:

    ls /var/log/apache2/

