# Moon Prism Packet Secrets

_"A late-night idol rehearsal leak includes one selfie and a whisper of intercepted packets. The photo looks harmless, but the gradient refuses to keep the guardians secret quiet"_

**CTF**: RIFFHACK: Black Market Break-In

**Difficulty:** Easy

**Category:** Forensics



# The Challenge

The challenge for this CTF gave two files for download, an image that appears like this and a pcap file.

![Image of gradients and a the message "Sailor V Rehearsal log // Moon Prism Spectrum" Signal Kept looping past midnight. Archive everything](https://chainsawcannon.neocities.org/posts/images/luna\_selfie.png)

Opening the moon\_chat.pcap file in wireshark appears with an intercepted chat between two entities, Artemis and Luna. The chat reads as such:

**Artemis:** _"That idol selfie felt too heavy. Did you stash the intel?"_ 

**Luna:** _"Affirmative. Append the rehearsal diary after the MOONSHINE marker"_

**Artemis:** _"Go for it. Carve after the sentinel. Its still a ZIP."_

**Luna:** _"Remember, the instructions live only in this capture. Delete after listening"_

So the files mention some sort of **ZIP** file and <b>stashed intel</b> so I was safe to assume this was some form of Steganography challenge and the flag would be stored within the image itself. I found some tools for this courtesy of [0xRick](https://0xrick.github.io/lists/stego/) and used them to my advantage. First I looked within the image using exiftool on a Parrot Linux Security Edition Virtual Machine to have these results given to me in the terminal:

![Image of a Linux terminal on exiftool](https://chainsawcannon.neocities.org/posts/images/exiftoolscan.png)

There was indication of data being appended to this image as shown by the warning above, looking deeper for other tools I decided to use **Binwalk** to look at the image to give more indications.

![Image of a Linux terminal on a binwalk scan](https://chainsawcannon.neocities.org/posts/images/BinwalkScan.png)

The file recieved contains a zip file with diary\_entry.txt, binwalk can also be used to extract the data from this image so using **binwalk -e** in my terminal resulted in the following text file being extracted:

_"Dear Luna,_ 

_I slipped the rehearsal diary behind the MOONSHINE marker in the selfie, just like the idol manager asked. Anyone inspecting pixel data without carving tools should only see a pretty gradient._ 

_Flag: **bitctf{{m00n\_pr15m\_p4yl04d}}**_

_- Usagi"_



Theres the flag! This challenge was nice and was very simplistic to do, a nice warm-up for this CTF!



