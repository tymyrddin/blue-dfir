# Linux incident response

## Timestamps

To add timestamps to `.bash_history`, at the bottom of `~/.bashrc` or `/etc/profile`, add:

```bash
export HISTTIMEFORMAT='%T %d/%m/%y '
```

## Processes 

Track parent-child processes:

```shell
ps -aux --forest
```

Get an overview of every running process running from a non-standard path:

```shell
sudo ls -l /proc/[0-9]*/exe 2>/dev/null | awk '/ -> / && !/\/usr\/(lib(exec)?|s?bin)\// {print $9, $10, $11}' | sed 's,/proc/\([0-9]*\)/exe,\1,'
```

List every process:

```shell
sudo ls -l /proc/[0-9]*/exe 2>/dev/null | awk '/ -> / {print $NF}' | sort | tac
```


## Networks

Get a quick overview of network activity:

```shell
netstat -plunt
```

This alternative also helps re-visualise the originating command and user that a network connection belongs to:

```shell
sudo lsof -i
```

## Files

### Enumerating

Recursively look for particular file types, and once you find the files get their hashes:

```shell
find . type f -exec sha256sum {} \; 2> /dev/null | grep -Ei '.asp|.js' | sort
```

Recursively list out folders and filders in their tree-branch relationship:

```shell
tree
```

Tree and show the users who own the files and directories:

```shell
tree -u
```

Stack with a `grep` to find a particular user:

```shell
tree -u | grep 'root'
```

Get information about a file:

```shell
stat file.txt
```

### Files and dates

Note: timestamps can be manipulated and can't be trusted during an IR.

Print the files and their corresponding timestamp:

```shell
find . -printf "%T+ %p\n"
```

Show all files created between two dates:

```shell
find -newerct "01 Jun 2021 18:30:00" ! -newerct "03 Jun 2021 19:00:00" -ls | sort
```

### Compare files

```shell
vimdiff file1.txt file2.txt
```

## Persistence

Persistence helps a malware persist on a machine after startup or any other mechanism that would stop a malware 
from running.

### Cron jobs

```shell
cat /etc/crontab
```

### Services

A list of services can be found in the `/etc/init.d` directory:

```shell
ls /etc/init.d/
```

### Bash

Check `.bashrc`:

```shell
cat ~/.bashrc
```

System wide settings:

```shell
/etc/bash.bashrc
/etc/profile
```

## Evidence of execution

Execution history of `sudo`:

```shell
cat /var/log/auth.log* |grep -i COMMAND|tail
```

Bash history:

```shell
cat ~/.bash_history
```

Check file access using `vim`:

```shell
cat ~/.viminfo
```

## Log files

`Syslog` contains messages that are recorded by the host about system activity:

```shell
cat /var/log/syslog* | head
```

`Auth` logs have information about users and authentication-related logs:

```shell
cat /var/log/auth.log* | head
```

`/var/log` contains logs for third-party applications, for example `Apache2` logs:

```shell
ls /var/log/apache2/
```

## Resources

* [You Don’t Know Jack About .bash_history - SANS DFIR Summit 2016](https://www.youtube.com/watch?v=wv1xqOV2RyE)