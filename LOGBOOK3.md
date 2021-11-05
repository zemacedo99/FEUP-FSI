# Trabalho realizado na Semana #3

## Identificação

- item1
- item2
- item3
- item4

## Catalogação

- item1
- item2
- item3
- item4

## Exploit

Vulnerability Type: Denial of Service
Attack Type: Local

There are some preconditions to exploit this weakness:
- The user must have received or sent a gif file;
- The user’s IMEI needs to be known;
- Have access to the targeted device;

Knowing that, the next step is to craft the malicious emoji using the IMEI and a python script (https://github.com/offensive-security/exploitdb-bin-sploits/raw/master/bin-sploits/46853.zip) that crafts a malicious emoji file. Then, rewrite the file in /sdcard/tencent/MicroMsg/[User_ID]/emoji/[WXGF_ID] with the crafted emoji.
After that, everytime the emoji is used in a message the application instantly crash.

## Ataques

There are no records of attacks using this exploit. However, there is a proof of concept at https://drive.google.com/open?id=1x1Z3hm4j8f4rhv_WUp4gW-bhdtZMezdU.
Due to the preconditions necessary to execute this attack, the probability of damage in a grand scale is small. Furthermore, deleting the app’s files and reinstalling it should fix the issue.
