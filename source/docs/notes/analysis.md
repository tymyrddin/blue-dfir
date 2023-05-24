# Artifact analysis

In the analysis phase, the digital objects to be used as digital evidence to support or refute a hypothesis of a crime, incident, or event, are determined. For example:

* Identifying and fingerprinting devices, operating systems, and running services.
* Analysing memory dumps to discover traces of ransomware.
* Swap analysis.
* Password dumping.
* Examining a Firefox browser and Gmail artifacts.

Statistical methods, manual analysis, techniques for understanding protocols and data formats, linking of multiple data objects (for example with data mining), and timelining are some of the techniques that are used during analysis.

## Anti-forensics

Digital forensics is comprehensive and challenging work by itself. And certain techniques such as media wiping and encryption and obfuscation are used to make forensic analysis of digital evidence even harder. 

For example, malware developers use anti-forensics mechanisms and techniques making forensic analysis of both the functionality and origin of malicious software difficult. Encryption of configuration files is a typical technique, especially for botnet malware where the botnet masters want to avoid breaches of the C&C mechanisms used to control the bots.

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

## Resources

* [ForensicArtifacts](https://github.com/ForensicArtifacts)
* [Forensics Artifacts documentation](https://artifacts.readthedocs.io/en/latest/)
* [National Software Reference Library (NSRL)](https://www.nist.gov/itl/ssd/software-quality-group/national-software-reference-library-nsrl)
