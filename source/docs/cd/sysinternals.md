# Sysinternals

## Scenario

[Cyber Defenders Sysinternals challenge](https://cyberdefenders.org/blueteam-ctf-challenges/100): A user thought they were downloading the SysInternals tool suite and attempted to open it, but the tools did not launch and became inaccessible. Since then, the user has observed that their system has gradually slowed down and become less responsive.

## Tools

* Registry Explorer
* Event Log Explorer
* AppCompatCachParser
* VirusTotal
* Web Cache View
* FTK Imager
* Autopsy

Autopsy 2.24 is included in SIFT. Autopsy 2 is obsolete (the last update was in 2010). To get 2.24 on SIFT to work, start her up as root. Doesn't work well for this challenge. After some [struggles on Kali](https://testlab.tymyrddin.dev/docs/dfir/autopsy-kali), my conclusion is to use a [Windows VM and install Autopsy 4](https://testlab.tymyrddin.dev/docs/dfir/autopsy-windows) for these challenges.

----

## Questions

Q1 What was the malicious executable file name that the user downloaded?



Q2 When was the last time the malicious executable file was modified? 12-hour format


Q3 What is the SHA1 hash value of the malware?


Q4 What is the malware's family?


Q5 What is the first mapped domain's Fully Qualified Domain Name (FQDN)?


Q6 The mapped domain is linked to an IP address. What is that IP address?


Q7 What is the name of the executable dropped by the first-stage executable?


Q8 What is the name of the service installed by 2nd stage executable?


Q9 What is the extension of files deleted by the 2nd stage executable?

