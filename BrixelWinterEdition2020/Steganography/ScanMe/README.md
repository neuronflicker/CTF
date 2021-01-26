# Scan me
**Category:** [Steganography](../README.md)

**Points:** 10

**Description:**

Can you solve this scan puzzle?

It could be handy to hide messages

**Files:** qr-code.png

## Write-up
The attached *png* file is a QR Code:

<img src="qr-code.png" alt="Initial QR Code" width="500" />

If you scan this QR Code (we used *QR Droid* on Android), you get a lot of text:
```
brixelCTF brixelCTF brixelCTF brixelCTF brixelCTF 
brixelCTF brixelCTF brixelCTF brixelCTF brixelCTF
brixelCTF brixelCTF brixelCTF brixelCTF brixelCTF
brixelCTF brixelCTF brixelCTF brixelCTF brixelCTF
```
This isn't the flag, so there must be something else.

If you look closely at the top right-hand corner of QR Code, you can see a second QR Code embedded within the first. Scanning this smaller QR Code gives us a website: https://www.timesink.be/qrcode/flag.html.

> Note: At the time of writing, this website no longer exists. At the time of solving we didn't take any screenshots, so this write-up is based on the barcode scanner app history and our memory, rather than running through the solve again.

Going to the website we see another barcode, and a text entry box. To scan this we used *QR & Barcode Scanner on Android*. The result was the text *code-128-easy*. We entered this in the text box, and it showed us another barcode and text box.

Scanning this next barcode, we get the number *5449000133335*. Entering this number in the text box gave us yet another barcode and text box. 

We scanned this new barcode and it gave us the message *congratulations_this_is_the_last_barcode*.

Entering this final message in the text box gave us the flag!



