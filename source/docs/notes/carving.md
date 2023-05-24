# File recovery and data carving

File recovery techniques make use of the file system information and, by using this information, many files can be recovered. If the information is not correct, then it will not work. Data carving works only on raw data on the media and is not connected with file system structure. Data carving doesn’t care about any file systems which is used for storing files and is widely used for recovering deleted files and restoring valuable information from corrupted drives.

## Recovery

Most computer systems, digital devices, and file systems are typically designed to treat information in the most efficient way to enhance performance and the user experience. They are not designed to securely wipe or destroy data. In most modern file systems, only the pointer to the file is marked `available` or `unallocated`, marking the space as being available, and a new file can overwrite the previous file. Data can thus be recovered from the storage area even after deletion of a file, as long as the data area was not overwritten.

Even if an image were deleted years prior to an investigation, parts of the image might still be extracted. The image data can be rebuilt in a viewer, and represent the original material and its content, even with details or portions of the data missing.

In cases of deleted files, it is essential to carefully document traceability to the original data source, since methods used for examination may change the state of data. These actions need to be documented to maintain evidence integrity.

Corrupt, severely damaged, or partially wiped and overwritten filesystems require manually reassembling sectors or blocks into files for recovery and other low level analysis techniques.

## Carving

Carving only uses the information in the raw data, not the file system information. 

There are many carving techniques and [carving tools](https://testlab.tymyrddin.dev/docs/dfir/carving) and there is no standard method of rating or comparing between them.

Always use multiple tools to ensure that the results are accurate by comparing the results of the tools used. And keep a log of tool testing for verification of the findings.

## Parsers

Media file parsers use techniques to identify patterns or signatures associated with particular file formats and types. Most file types contain headers and footers that make them readable to the applications handling them; these applications render the file as intended for use by the user. The use of carving tools allows for selecting a set of file types to recover from a piece of media. Media carving can be quite useful in order to acquire deleted files such as previously deleted images.

Other parsers are useful for examining one of the most common electronic communication protocols: email. Email parsers can crawl through the raw data copied from a digital device and render individual emails in a manner that makes it easier to analyze them in later phases of an investigation.

The parsing of proprietary files and objects requires examining the information contained within files.

Automation is an objective in itself during the examination phase, for reasons of both forensic soundness and efficiency. Most of the tasks in the examination phase can be automated using scripts or programs. File parsing, string searches, and extraction of compressed files will significantly reduce the manual task load, in particular when working with large datasets.
