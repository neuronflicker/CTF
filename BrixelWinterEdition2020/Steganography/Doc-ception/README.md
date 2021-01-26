# Doc-ception
**Category:** [Steganography](../README.md)

**Points:** 10

**Description:**

Need to hide something? why not a word document?

You need to dig deeper

**This flag is not in the usual format, you can enter it with or without the brixelCTF{flag} format**

**Files:** loremipsum.docx

## Write-up
We opened the document, but it just contained the standard *Lorem Ipsum* text. We tried highlighting to see if there was any extra text, and also tried making hidden text visible. We also tried looking at the document information. There didn't seem to be anything in the document itself that contained the flag.

Our knowledge of the format of Word Document files helped here. One way to recover some data from corrupted Word documents is to try unzipping it and getting what images/text you could from inside the *docx* file. This can be done because *docx* is just a zipped format. We unzipped the file using [7-zip](https://www.7-zip.org/).

> Note: If you want to use the Windows zip extractor, you have to rename the file to *loremipsum.zip*, but 7-zip enables you to unzip it without renaming

During the unzip, 7-zip asked if we wanted to overwrite *loremipsum.docx* as the *docx* file contained another file named that. We clicked *Auto Rename*.

Looking at the result, we could find nothing containing a flag. Because the challenge was named *Doc-ception*, we wondered if we could unzip the new version of *loremipsum.docx* (now renamed *loremipsum_1.docx* by 7-zip) to go deeper inside (as is also suggested by the description). 

We did unzip it, and inside was a file named *flag.txt* which contained the flag.





