# Practical investigations

## BOB! THIS ISN'T A HORSE!

Your SOC has informed you that they have gathered a memory dump from a quarantined endpoint thought to have been 
compromised by a banking trojan masquerading as an Adobe document. Your job is to use your knowledge of threat 
intelligence and reverse engineering to perform memory forensics on the infected host.

You have been informed of a suspicious IP in connection to the file that could be helpful: `41.168.5.140`

The memory file is located in `/Scenarios/Investigations/Investigation-1.vmem`

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-1.vmem windows.info
Volatility 3 Framework 1.0.1
Progress:  100.00		PDB scanning finished                     
Variable	Value

Kernel Base	0x804d7000
DTB	0x2fe000
Symbols	file:///opt/volatility3/volatility3/symbols/windows/ntkrnlpa.pdb/30B5FB31AE7E4ACAABA750AA241FF331-1.json.xz
Is64Bit	False
IsPAE	True
primary	0 WindowsIntelPAE
memory_layer	1 FileLayer
KdDebuggerDataBlock	0x80545ae0
NTBuildLab	2600.xpsp.080413-2111
CSDVersion	3
KdVersionBlock	0x80545ab8
Major/Minor	15.2600
MachineType	332
KeNumberProcessors	1
SystemTime	2012-07-22 02:45:08
NtSystemRoot	C:\WINDOWS
NtProductType	NtProductWinNt
NtMajorVersion	5
NtMinorVersion	1
PE MajorOperatingSystemVersion	5
PE MinorOperatingSystemVersion	1
PE Machine	332
PE TimeDateStamp	Sun Apr 13 18:31:06 2008
thmanalyst@ubuntu:~$ 
```

Checking processes:

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-1.vmem windows.psscan
Volatility 3 Framework 1.0.1
Progress:  100.00		PDB scanning finished                     
PID	PPID	ImageFileName	Offset	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime	File output

908	652	svchost.exe	0x2029ab8	9	226	0	False	2012-07-22 02:42:33.000000 	N/A	Disabled
664	608	lsass.exe	0x202a3b8	24	330	0	False	2012-07-22 02:42:32.000000 	N/A	Disabled
652	608	services.exe	0x202ab28	16	243	0	False	2012-07-22 02:42:32.000000 	N/A	Disabled
1640	1484	reader_sl.exe	0x207bda0	5	39	0	False	2012-07-22 02:42:36.000000 	N/A	Disabled
1512	652	spoolsv.exe	0x20b17b8	14	113	0	False	2012-07-22 02:42:36.000000 	N/A	Disabled
1588	1004	wuauclt.exe	0x225bda0	5	132	0	False	2012-07-22 02:44:01.000000 	N/A	Disabled
788	652	alg.exe	0x22e8da0	7	104	0	False	2012-07-22 02:43:01.000000 	N/A	Disabled
1484	1464	explorer.exe	0x23dea70	17	415	0	False	2012-07-22 02:42:36.000000 	N/A	Disabled
1056	652	svchost.exe	0x23dfda0	5	60	0	False	2012-07-22 02:42:33.000000 	N/A	Disabled
1136	1004	wuauclt.exe	0x23fcda0	8	173	0	False	2012-07-22 02:43:46.000000 	N/A	Disabled
1220	652	svchost.exe	0x2495650	15	197	0	False	2012-07-22 02:42:35.000000 	N/A	Disabled
608	368	winlogon.exe	0x2498700	23	519	0	False	2012-07-22 02:42:32.000000 	N/A	Disabled
584	368	csrss.exe	0x24a0598	9	326	0	False	2012-07-22 02:42:32.000000 	N/A	Disabled
368	4	smss.exe	0x24f1020	3	19	N/A	False	2012-07-22 02:42:31.000000 	N/A	Disabled
1004	652	svchost.exe	0x25001d0	64	1118	0	False	2012-07-22 02:42:33.000000 	N/A	Disabled
824	652	svchost.exe	0x2511360	20	194	0	False	2012-07-22 02:42:33.000000 	N/A	Disabled
4	0	System	0x25c89c8	53	240	N/A	False	N/A	N/A	Disabled
thmanalyst@ubuntu:~$ 
```
`reader_sl.exe` is suspicious. What is its parent process?

```text
$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-1.vmem windows.pstree
Volatility 3 Framework 1.0.1
Progress:  100.00		PDB scanning finished                     
PID	PPID	ImageFileName	Offset(V)	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime

4	0	System	0x8205bda0	53	240	N/A	False	N/A	N/A
* 368	4	smss.exe	0x8205bda0	3	19	N/A	False	2012-07-22 02:42:31.000000 	N/A
...
1484	1464	explorer.exe	0x8205bda0	17	415	0	False	2012-07-22 02:42:36.000000 	N/A
* 1640	1484	reader_sl.exe	0x8205bda0	5	39	0	False	2012-07-22 02:42:36.000000 	N/A
thmanalyst@ubuntu:~$
```

User-agent?

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-1.vmem -o /home/thmanalyst windows.memmap.Memmap --pid 1640 --dump
...
thmanalyst@ubuntu:~$ ls
pid.1640.dmp
```

Once the dump is stored use, `strings *.dmp | grep -i "user-agent"`:

```text
strings *.dmp | grep -i "user-agent"
thmanalyst@ubuntu:~$ strings *.dmp | grep -i "user-agent"
User-Agent
User-Agent: Mozilla/5.0 (Windows; U; MSIE 7.0; Windows NT 6.0; en-US)
 cs(User-Agent)
USER-AGENT:
User-Agent:
thmanalyst@ubuntu:~$ 
```

Was Chase Bank one of the suspicious bank domains found?

```text
thmanalyst@ubuntu:~$ strings *.dmp | grep "chase"
*chase.com*
...
<td class="steptextoff" align="center" title="Step two of three has not been completed.">Credit Card confirmation<img src="https://chaseonline.chase.com/images//spacer.gif" alt="Step two of three has not been completed." width="1" height="1"/></td>
...
```

## That Kind of Hurt my Feelings

You have been informed that your corporation has been hit with a chain of ransomware that has been hitting 
corporations internationally. Your team has already retrieved the decryption key and recovered from the attack. 
Still, your job is to perform post-incident analysis and identify what actors were at play and what occurred on 
your systems. You have been provided with a raw memory dump from your team to begin your analysis.

The memory file is located in `Scenarios/Investigations/Investigation-2.raw`

What suspicious process is running at PID 740?

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-2.raw windows.psscan
Volatility 3 Framework 1.0.1
Progress:  100.00		PDB scanning finished                     
PID	PPID	ImageFileName	Offset	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime	File output

860	1940	taskdl.exe	0x1f4daf0	0	-	0	False	2017-05-12 21:26:23.000000 	2017-05-12 21:26:23.000000 	Disabled
536	1940	taskse.exe	0x1f53d18	0	-	0	False	2017-05-12 21:26:22.000000 	2017-05-12 21:26:23.000000 	Disabled
424	1940	@WanaDecryptor@	0x1f69b50	0	-	0	False	2017-05-12 21:25:52.000000 	2017-05-12 21:25:53.000000 	Disabled
1768	1024	wuauclt.exe	0x1f747c0	7	132	0	False	2017-05-12 21:22:52.000000 	N/A	Disabled
576	1940	@WanaDecryptor@	0x1f8ba58	0	-	0	False	2017-05-12 21:26:22.000000 	2017-05-12 21:26:23.000000 	Disabled
260	664	svchost.exe	0x1fb95d8	5	105	0	False	2017-05-12 21:22:18.000000 	N/A	Disabled
740	1940	@WanaDecryptor@	0x1fde308	2	70	0	False	2017-05-12 21:22:22.000000 	N/A	Disabled
1168	1024	wscntfy.exe	0x1fea8a0	1	37	0	False	2017-05-12 21:22:56.000000 	N/A	Disabled
544	664	alg.exe	0x2010020	6	101	0	False	2017-05-12 21:22:55.000000 	N/A	Disabled
...
4	0	System	0x23c8830	51	244	N/A	False	N/A	N/A	Disabled
thmanalyst@ubuntu:~$ 
```

What is the full path of the suspicious binary in PID 740?

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-2.raw windows.dlllist | grep 740
1024resssvchost.exe	0x5f740000	0xe000	ncprov.dll	C:\WINDOWS\system32\wbem\ncprov.dll	N/A	Disabled
740	@WanaDecryptor@	0x400000	0x3d000	@WanaDecryptor@.exe	C:\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe	N/A	Disabled
740	@WanaDecryptor@	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
...
thmanalyst@ubuntu:~$ 
```

What is the parent process of PID 740? And its PID?

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-2.raw windows.pstree
Volatility 3 Framework 1.0.1
Progress:  100.00		PDB scanning finished                     
PID	PPID	ImageFileName	Offset(V)	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime

4	0	System	0x81fea8a0	51	244	N/A	False	N/A	N/A
* 348	4	smss.exe	0x81fea8a0	3	19	N/A	False	2017-05-12 21:21:55.000000 	N/A
...
* 1940	1636	tasksche.exe	0x81fea8a0	7	51	0	False	2017-05-12 21:22:14.000000 	N/A
** 740	1940	@WanaDecryptor@	0x81fea8a0	2	70	0	False	2017-05-12 21:22:22.000000 	N/A
thmanalyst@ubuntu:~$ 
```

From the current information, what malware is present on the system? WannaCry!
What DLL is loaded by the decryptor used for socket creation? `WS2_32.dll`
What mutex can be found that is a known indicator of the malware in question?

```text
thmanalyst@ubuntu:~$ python3 /opt/volatility3/vol.py -f /Scenarios/Investigations/Investigation-2.raw windows.handles | grep 1940
596gresscsrss.exe	0x82218da0B scan0x388finProcess 0x1f0fff        csrss.exe Pid 1940
596	csrss.exe	0x8222eda0	0x390	Thread	0x1f03ff	Tid 1944 Pid 1940
...
1940	tasksche.exe	0x821883e8	0x40	Mutant	0x120001	ShimCacheMutex
1940	tasksche.exe	0xe16644e0	0x44	Section	0x2	ShimSharedMemory
1940	tasksche.exe	0x822386a8	0x48	File	0x100001	\Device\KsecDD
1940	tasksche.exe	0x823d54d0	0x4c	Semaphore	0x1f0003	shell.{A48F1A32-A340-11D1-BC6B-00A0C90312E1}
1940	tasksche.exe	0x823a0cd0	0x50	File	0x100020	\Device\HarddiskVolume1\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202
1940	tasksche.exe	0x8224f180	0x54	Mutant	0x1f0001	MsWinZonesCacheCounterMutexA
1940	tasksche.exe	0x822e3b08	0x58	Mutant	0x1f0001	MsWinZonesCacheCounterMutexA0
...
1940	tasksche.exe	0xe18c02d0	0xe8	Section	0x4	
thmanalyst@ubuntu:~$ 
```

What plugin could be used to identify all files loaded from the malware working directory?

```text
windows.filescan
```

## Resources

* [Threat Spotlight: Inside the WannaCry Attack](https://blogs.blackberry.com/en/2017/06/threat-spotlight-inside-the-wannacry-attack)
* [WannaCry Malware Profile](https://www.mandiant.com/resources/blog/wannacry-malware-profile)