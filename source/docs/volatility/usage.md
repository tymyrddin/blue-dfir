# Using volatility

## Identifying image info and profiles 

By default, Volatility comes with all existing Windows profiles from Windows XP to Windows 10.

Image profiles can be hard to determine if you don't know exactly what version and build the machine you extracted 
a memory dump from was. In some cases, a memory file with no other context can be a given. In that case, use 
Volatility's `imageinfo` plugin. It will take the provided memory dump and assign it a list of the best possible 
OS profiles. **_OS profiles_ have been deprecated with Volatility3.** 

`imageinfo` is not always correct and can have varied results depending on the provided dump; use with caution 
and test multiple profiles from the provided list.

To get information about what the host is running from the memory dump, use `windows.info`, `linux.info` and `mac.info`. 

    python3 vol.py -f <file> <os>.info

## Listing processes and connections

Five different plugins within Volatility allow for dumping processes and network connections.

### pslist

`pslist` will get the list of processes from the doubly-linked list that keeps track of processes in memory. The 
output from this plugin will include all current processes and terminated processes with their exit times.

    python3 vol.py -f <file> windows.pslist

Some malware (rootkits), will try to hide their processes by unlinking itself from the list. By 
unlinking rom the list, their processes will not be visible when using `pslist`.

### psscan

To combat this evasion technique, we can use `psscan`. This technique of listing processes will locate processes 
by finding data structures that match `_EPROCESS`. But, it can also cause false positives.

    python3 vol.py -f <file> windows.psscan

### pstree

`pstree` does not offer any other kind of special techniques to help identify evasion like the last two plugins. 
This plugin will list all processes based on parent process ID, using the same methods as `pslist`. This can be 
useful for an analyst to get a full story of the processes and what may have been occurring at the time of extraction.

    python3 vol.py -f <file> windows.pstree

### netstat

`netstat` will attempt to identify all memory structures with a network connection.

    python3 vol.py -f <file> windows.netstat

This volatility3 command can be unstable, particularly for old Windows builds. Use other tools like 
[bulk_extractor](https://tools.kali.org/forensics/bulk-extractor) to extract a `PCAP` file from the memory file. 
In some cases, this is preferred in network connections that cannot be identified from Volatility alone.

### dlllist

`dlllist` will list all `DLL`s associated with processes at the time of extraction. This can be useful when having 
done further analysis: filter output to a specific DLL that might be an indicator for a specific type of malware 
believed to be present on the system.

    python3 vol.py -f <file> windows.dlllist

## Hunting and detection capabilities

Although Volatility offers many commands useful for hunting for malware or other anomalies within a system's memory, 
[a few are designed specifically for hunting rootkits and malicious code](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal). 
The most comprehensive documentation for these commands can be found in the 
Malware Analyst's Cookbook and DVD: Tools and Techniques For Fighting Malicious Code.


