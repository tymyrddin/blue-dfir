# First responder

A dedicated first responder who is alerted and called to the scene usually has some knowledge or understanding of the workings of devices, networks, and perhaps even of the IT infrastructure in the community or organisation, if applicable. First responders do not have to be may not be skilled in digital forensics or digital investigations.

First responders are responsible for securing the scene and ensuring that the data, peripherals, equipment, and storage are not used, tampered with, removed, or compromised by unauthorised individuals, so as not to compromise a possible investigation for a legal case. The below therefor assumes heading for a legal case. Better too much than too little.

Documentation of the scene is usually also done by first responders to aid in further investigations. Documentation of the scene can include photographs, video, voice recording, and manual documentation of:

* The room where the device is located (desk, ceiling, entrance/exit, windows, lighting, electrical outlets, and data drops)
* The state of the device (on, off, power light blinking)
* The screen contents and whether the device is on (operating system, running programs, date and time, wired and/or wireless network connectivity)
* Books, notes, and pieces of paper
* Connected and disconnected cables

Once the scene has been secured and documented, the forensic investigation can start: the process of acquiring what can be considered as physical evidence, for example:

* Computer system units
* Laptops
* Tablets
* Fixed and removable storage media
* Cables and chargers

## Order of volatility

When collecting data, a best practice would be to collect evidence in the order of most to least volatile, if possible:

* RAM: Data that may have been encrypted by the user may be stored in an unencrypted state in the RAM. Logged-in users, opened programs, accessed files, and running processes can all be extracted and analysed if the RAM and paging file are analysed. If the device is switched off or rebooted, this data and evidence can easily be lost.
* Running processes
* Active network connections
* System settings
* Storage media

This means acquire the contents of the memory and the paging file first as they are the most volatile and can be easily lost, resulting in the loss of very useful information, including unencrypted passwords, user and program information, network connection information, and other types of useful data.

## Chain of custody (CoC)

CoC is a form that legally ensures the integrity of evidence as it is exchanged between individuals, and provides a level of accountability as personal identification is required when completing the forms. This form gives an exact log and account of the transportation and the exchange between parties, from collection at the scene to a presentation in a court. 

## Live acquisition

Investigating a live system means that logs and timestamps will be generated while the system is being examined. Therefore, thorough documentation will justify the actions taken and not be viewed as evidence tampering. When investigating a powered-on device:

* Move the mouse or glide your fingers across the touchpad if you suspect the device may be in a sleep state. Do not click on the buttons as this may open or close programs and processes.
* Photograph and record the screen and all visible programs, data, time, and desktop items.
* Data stored in RAM and paging files must be collected with as little modification to the data as possible. Use Guymager, dc3dd in Kali Linux. CAINE and Helix can also be used for acquiring RAM and the paging file.
* Unplug the power cord on desktops and remove the battery, if possible, on portables (and read ***Post-mortem acquisition*** below on the issue).

## Post-mortem acquisition

At boot time, a PC, mobile phone, or media player executes boot activities that can overwrite previously cached data. Powered-off devices should never be turned on (unless done so by a forensic investigator), to ensure that existing data is not erased and that new data is not written. Devices can appear as if they are off while in a sleep or hibernate state. As a simple test, the mouse can be moved and monitors (if any) can be switched on to check state. Even if they are in an off state, photograph the screen and ports.

Depending on the incident type, use a pair of latex gloves, to avoid leaving additional fingerprints on the system and components.

Phones, media players, laptops, or any devices that can communicate over any network can potentially be altered while being seized or even after they have been seized. This can be caused by either deliberate tampering or unintentional changes (humidity or electricity). Switch the device to airplane mode to avoid any further connections and communications, and consider the use of containers that can shield the device from external radiofrequency (RF) sources, such as a Faraday bag.

When investigating portable and mobile devices in an already off state, it is suggested that the battery be removed (if possible) and also placed in an evidence bag to ensure that there will be no way of accidentally turning on the device once unplugged.  Note that removing the battery can alter the contents in the volatile memory, even when in an off state. Whether to remove the battery depends on the type of incident.

## Write blocking

This is when a forensic investigator gains access to the electronic device(s) containing raw data that has been identified as relevant for the specific case.

The original evidence should only be used to create forensic copies or images. A write blocker (either in hardware or in software) is typically used to guarantee that no data is written to the original storage media. If a hardware write blocker is not available, software versions are available as standalone features in Caine, and as a part of some commercial and open source tools, such as EnCase and Autopsy. Choose a write blocker based on functionality, not just cost.

## Data imaging and hashing

***Imaging*** is the exact copying of data either as a file, folder, partition, or entire storage media or drive. When doing a regular copy of files and folders, not all files may be copied due to their attributes being set to the system or even hidden. To prevent files from being left out, a ***bit-stream copy*** is used in which every bit is copied or imaged exactly as it is on the current medium as if taking a picture or snapshot of the data, creating a ***physical image***.

A digital signature (cryptographic hash) is calculated both for the original media and for the copy, to verify that the evidence was not tampered with or modified after the evidence was acquired. The stronger the cryptographic algorithm used, the less chance of it being attacked or compromised. This means that the integrity of the evidence and physical images created remain intact, which will prove useful in forensic cases and expert testimony.

## Resources

* [Computer forensics: Chain of custody [updated 2019]](https://resources.infosecinstitute.com/topic/computer-forensics-chain-custody/)