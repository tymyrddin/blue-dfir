# Android analysis

## Add image to Autopsy

1. Open the Autopsy tool and select the Create New Case option.
2. Enter all the necessary case details, including the name of the case, the location where data needs to be stored, ... and click ***Next***.
3. Enter a case number and researcher details, and click on ***Finish***.
4. Click on the ***Add Data Source*** button, add the image file to be analysed, set the correct time zone and click on ***Next***.
5. Choose the modules you want to run on the image: `Android Analyzer`, `Exif Parser`, `Keyword Search`, `PhotoRec Carver`, and `Recent Activity` at minimum. Maybe also the `Extension Mismatch Detector`. Click on ***Next***.

It takes a few minutes to parse through the image depending on its size. There may be some errors or warning messages.

## Analysing an image using Autopsy

When the image is loaded, expand the file present under Data Sources to see data present in the image: Call logs, contacts, GPS trackpoints and messages are extracted by Android Analyzer module, EXIF metadata is extracted by EXIF Parser module, files with wrong extensions are detected by Extension Mismatch Detector module, and web cookies, web downloads, web history/web searches are extracted by Recent Activity module. And Autopsy also supports automatic deleted files recovery from ext4 file systems.

## Resources

* [About DFIR/Tools & Artifacts > Android](https://aboutdfir.com/toolsandartifacts/android/)