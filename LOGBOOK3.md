# Work performed during the week #3

## Identification


DoS Wechat with an emoji

This vulnerability affected a product code base of WeChat, developed by Tencent, for Android up to version 7.0.4, a free messaging and calling app mostly used in China.

It occurs in the function vcodec2_hls_filter in libvoipCodec_v7a.so (Emoji File Handler) allowing attackers to cause a denial of service.

The Common Vulnerabilities and Exposures (CVE) Program has assigned the ID CVE-2019-11419 to this issue.

## Cataloguing


The author of this exploit was Louis Hong Nhat Pham (https://sg.linkedin.com/in/hongnhat), a Cyber Security Researcher at Nanostrea Technological University. He published the weakness and the exploit at 14 May 2019 (https://awakened1712.github.io/hacking/hacking-wechat-dos/).
There is not known any bug-bounty.
    

CVSS v3.0 : 5.5 /10 - MEDIUM 
CVSS v2.0 : 4.3 /10 - MEDIUM
Exploitability : 1.8 / Impact : 3.6
    
This DoS bug was reported to Tencent, but they decided not to fix because it’s not critical.


## Exploit


Vulnerability Type: Denial of Service
Attack Type: Local
    
There are some preconditions to exploit this weakness:
        - The user must have received or sent a gif file;
        - The user's IMEI needs to be known;
        - Have access to the targeted device;
        
Knowing that, the next step is to craft the malicious emoji using the IMEI and a python script (https://github.com/offensive-security/exploitdb-bin-sploits/raw/master/bin-sploits/46853.zip) that crafts a malicious emoji file. Then, rewrite the file in /sdcard/tencent/MicroMsg/[User_ID]/emoji/[WXGF_ID] with the crafted emoji.
After that, everytime the emoji is used in a message the application instantly crash.
    

## Attacks

There are no records of attacks using this exploit. However, there is a proof of concept at https://drive.google.com/open?id=1x1Z3hm4j8f4rhv_WUp4gW-bhdtZMezdU.
Due to the preconditions necessary to execute this attack, the probability of damage in a grand scale is small. Furthermore, deleting the app's files and reinstalling it should fix the issue.


    
