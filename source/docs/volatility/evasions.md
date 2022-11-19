# Real world memory forensics

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
