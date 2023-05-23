# Data acquisition

There are multiple scenarios in which data is acquired for a digital forensic investigation. Procedures which can be used in every environment:

1. Make sure a write blocker is attached/active.
2. Identify devices, partitions, input and output files, and parameters 
3. Make image
4. Hash the evidence before and during, or after, acquisition to provide proof that it was not tampered with.

## Write blocking

This is when a forensic investigator gains access to the electronic device(s) containing raw data that has been identified as relevant for the specific case.

The original evidence should only be used to create forensic copies or images. A write blocker (either in hardware or in software) is typically used to guarantee that no data is written to the original storage media. If a hardware write blocker is not available, software versions are available as standalone features in Caine, and as a part of some commercial and open source tools, such as EnCase and Autopsy. Choose a write blocker based on functionality, not just cost.

## Data imaging and hashing

***Imaging*** is the exact copying of data either as a file, folder, partition, or entire storage media or drive. When doing a regular copy of files and folders, not all files may be copied due to their attributes being set to the system or even hidden. To prevent files from being left out, a ***bit-stream copy*** is used in which every bit is copied or imaged exactly as it is on the current medium as if taking a picture or snapshot of the data, creating a ***physical image***.

A digital signature (cryptographic hash) is calculated both for the original media and for the copy, to verify that the evidence was not tampered with or modified after the evidence was acquired. The stronger the cryptographic algorithm used, the less chance of it being attacked or compromised. This means that the integrity of the evidence and physical images created remain intact, which will prove useful in forensic cases and expert testimony.

## Volatile evidence

The term ***forensically sound manner*** means leaving the smallest possible footprint during collection to minimise the amount of data being changed with the collection. To collect volatile evidence, start from the most to the least volatile:

1. Live system
2. Running
3. Network
4. Virtual
5. Physical

Approach volatile data collection with the same mindset as creating forensic images. Document each step, because any interaction with the machine to collect volatile data, which will change the evidence. Changes made do not affect what is under investigation, but changes are being made to the system; In a legal case a question can be asked about potential changes to the evidence while testifying at a administrative or judicial proceeding.

## Collecting RAM

RAM is considered to be the most volatile of all volatile data.

Collecting the volatile data may not always be possible, depending on the specific set of circumstances on the scene. 
If, for example, there is a destructive process running on the machine and the information to be collected is being altered or overwritten, you may not want to take the time to collect the RAM as evidence is being manipulated.
If it is a remote connection causing the destructive process, document the connection, sever the connection, and
then collect the RAM. It depends. 

If the attacker is connected remotely and is accessing highly sensitive data, you will probably want to interrupt the connection.If it is not critical information, it may be useful to let the attacker maintain access while you collect the RAM.

## Making a working copy

* A forensic copy is a straight bit-for-bit copy of the source to the destination. Not very common. Make sure the  destination device has no old data on it to prevent cross-contamination between the current and a past investigation. Recover deleted files, file slack, and partition slack. 
* A forensic image or forensic evidence file is a bit-for-bit copy of the source device, with the data stored in a forensic image format (DD, E01, AFF). Recover deleted files, file slack, and partition slack.
* A logical forensic image is restricted to specific datasets. It is not allowed to access the entire container. This can be used when extracting data from a server (the server can not be shut down) by making logical copies of the files and folders pertinent to an investigation. Deleted files, file slack, and partition slack are not recovered.

## Memory acquisition in Linux

On the command-line, use `dd`, the data dump tool, and/or`dc3dd`, the enhancement of `dd`. Guymager, has built-in case-management abilities and also has many functional similarities to `dc3dd`, but it comes as a GUI tool and may be easier to use.

## Memory acquisition in Windows

On Windows, use FTK Imager (RAM and disk images) or Belkasoft Ram Capturer (only RAM acquisition).


