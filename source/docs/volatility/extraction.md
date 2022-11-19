# Memory extraction

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

