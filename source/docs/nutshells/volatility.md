# Volatility

## Memory extraction

Extracting a memory dump can be done in several ways, depending on the requirements of the investigation:

* FTK Imager
* Redline
* DumpIt.exe
* win32dd.exe / win64dd.exe
* Memoryze
* FastDump

Most tools will output a `.raw` file, with some exceptions like Redline using its own agent and session structure.

For virtual machines, gathering a memory file can be done by collecting the virtual memory file from the host 
machine’s drive. The file extension depends on the hypervisor used:

* VMWare - .vmem
* Hyper-V - .bin
* Parallels - .mem
* VirtualBox - .sav file *this is only a partial memory file

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

## Real world memory forensics

When dealing with an advanced adversary, you may encounter malware, most of the time rootkits that will employ 
very nasty evasion measures that will require you as an analyst to dive into the drivers, mutexes, and hooked 
functions. A number of modules can help us in this journey to further uncover malware hiding within memory.

## Hooking

The first evasion technique we will be hunting is hooking; there are five methods of hooking employed by 
adversaries:

* `SSDT` Hooks
* `IRP` Hooks
* `IAT` Hooks
* `EAT` Hooks
* Inline Hooks

### SSDT

Focusing on hunting `SSDT` hooking  (for now) as this one of the most common techniques when dealing with malware 
evasion and the easiest plugin to use with the base volatility plugins.

`SSDT` stands for System Service Descriptor Table. The Windows kernel uses this table to look up system functions. 
An adversary can hook into this table and modify pointers to point to a location the rootkit controls.

There can be hundreds of table entries: analyse the output further or compare against a baseline. A suggestion 
is to use this plugin after investigating the initial compromise and working off it as part of your lead investigation.

    Syntax: python3 vol.py -f <file> windows.ssdt

## Driver files

Adversaries will also use malicious driver files as part of their evasion. Volatility offers two plugins to list drivers.

### modules

The `modules` plugin will dump a list of loaded kernel modules; this can be useful in identifying active malware. 
If a malicious file is idly waiting or hidden, this plugin may miss it.

This plugin is best used once you have further investigated and found potential indicators to use as input for 
searching and filtering.

    python3 vol.py -f <file> windows.modules

### driverscan

The `driverscan` plugin will scan for drivers present on the system at the time of extraction. This plugin can help 
identify driver files in the kernel that the modules plugin might have missed or which were hidden.

It is recommended to have a prior investigation before moving on to this plugin. It is also recommended to look 
through the `modules` plugin before `driverscan`.

    python3 vol.py -f <file> windows.driverscan

In most cases, `driverscan` will come up with nothing. 

## Other plugins

Other plugins which can be helpful when attempting to hunt for advanced malware in memory:

* modscan
* [driverirp](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#driverirp)
* [callbacks](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#callbacks)
* [idt](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#idt)
* [apihooks](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#apihooks)
* moddump
* handles

Some of these are only present on Volatility2 or are part of third-party plugins.



