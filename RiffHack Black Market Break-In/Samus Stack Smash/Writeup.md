# About The Challenge
*"A Federation checkpoint AI loops the same authorization prompt while a damaged Chozo console hums behind it. Push past the guard and see what the access vault is hiding."*

**CTF:** *RIFFHACK: Black Market Break-In(

**Difficulty:** *Easy*

**Category:** *Binary Exploitation*

[Read on neocities?](https://chainsawcannon.neocities.org/posts/Riffhack-BMBI-Samus-Stack-Smash)

# The Challenge itself
The challenge for this contains a download for an executable file and an IP address and port number
On it's own the executable file runs a prompt for Samus to state her authorization glyphs.

![terminal displaying program](https://chainsawcannon.neocities.org/posts/images/Samus-Authorization.png)

For this we needed to find some form of vulnerability so I decided to open the program in Ghidra's code browser, opening it I see a function known as vuln() which is the function that asks for the authorization glyphs and echoes them back to the user.
![ghidra code browser showing the vuln() function](https://chainsawcannon.neocities.org/posts/images/samus-vuln.png)
![ghidra code browser showing mission_clear() function](https://chainsawcannon.neocities.org/posts/images/Samus_missionclear.png)

Digging more in the code browser there was also a function known as mission_clear(), this function is used to give flag information. giving another glance to the vuln() function it is noted the function uses a vulnerable command for scanning the user input, the gets() command, this command doesn't give bounds for any input which can result in a buffer overflow if going over certain limits (in this case for the buffer in the vuln() function, any input that is over 32 bytes). Using GDB I try to obtain the mission_clear function hash.

![Terminal showing mission_clear's hash value](https://chainsawcannon.neocities.org/posts/images/Samus-function.png)

So a buffer overflow would have to redirect to 0x401216 in order to access the mission clear function. Before I knew what the IP and port were for I ended up trying multiple tactics to get to the mission_clear function, including attempting python strings (which would be ignored by the program itself), manually inputting values (which lead to both segmentation faults and illegal instructions without giving the information), I even tried rewriting the program from the code given from Ghidra to skip the main authorization function and skip straight to mission clear (which lead to "Flag not found. Contact admin") Which lead me to realizing the program is also hosted on the IP address that was given in the problem itself. I mentioned my findings to the CTF team I was working with (thanks polysec) and thanks to the help of James he developed a simple script in python using pwnscript that would perform the buffer overflow on the IP address that was given by the challenge (the following script!)

from pwn import *

p = remote("CTF_IP", CTF_PORT) *#connects to IP (replace CTF_IP with given IP, CTF_PORT with given port)*

p.sendline(b'a'*40 + p64(0x0401216 + 5)) *#sends the buffer overflow to the server*

p.interactive() *#allows for interaction on the network*

This code sends the buffer overflow to the server to access the mission_clear() function, resulting in getting the flag from the server.
