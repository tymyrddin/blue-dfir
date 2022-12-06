# Autopsy

Autopsy is an open-source and powerful digital forensics platform. Several features within Autopsy have been developed by the Department of Homeland Security Science and Technology funding. 

Before diving into Autopsy and analysing data, there are a few steps to perform; such as identifying the data source and what Autopsy actions to perform with the data source. 

Basic workflow:

* Create/open the case for the data source you will investigate
* Select the data source you wish to analyse
* Configure the ingest modules to extract specific artefacts from the data source
* Review the artefacts extracted by the ingest modules
* Create the report

Case Analysis | Create a New Case

To prepare a new case investigation, you need to create a case file from the data source. When you start Autopsy, there will be three options. You can create a new case file using the "New Case" option. Once you click on the "New Case" option, the Case Information menu opens, where information about the case is populated.

* Case Name: The name you wish to give to the case
* Base Directory: The root directory that will store all the files specific to the case (the full path will be displayed)
* Case Type: Specify whether this case will be local (Single-user) or hosted on a server where multiple analysts can review (Multi-user)