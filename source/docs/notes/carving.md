# File recovery and data carving

File recovery techniques make use of the file system information and, by using this information, many files can be recovered. If the information is not correct, then it will not work. Data carving works only on raw data on the media and is not connected with file system structure. Data carving doesn’t care about any file systems which is used for storing files and is widely used for recovering deleted files and restoring valuable information from corrupted drives.

Always use multiple tools to ensure that the results are accurate by comparing the results of the tools used. And keep a log of tool testing for verification of the findings.

## Linux

* `foremost` is a simple and effective command line interface (CLI) tool that recovers files by reading their headers and footers.
* `recoverjpeg` can be used to recover `.jpg` images. The recovered files will be saved in the Home folder and will not have their original names, but instead will be named in numerical order, starting from `00000.jpg`.
* `scalpel`, created as an improvement of a much earlier version of `foremost`, aims to address the high CPU and RAM usage issues of foremost when carving data. Unlike foremost, file types of interest must be specified by the investigator in the `scalpel` configuration file (`etc/scapel/scalpel.conf`).
* `bulk_extractor` extracts additional types of information that can be very useful in investigations, such as email addresses, credit card numbers, urls visited (or bookmarked), online searches, social media profiles, etc.