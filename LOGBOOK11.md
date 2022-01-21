## SeedLab

### Task 1

- In this task, we made ourselves a root CA, and generate a certificate for this CA (the root CA’s certificates are self-signed).
- In order to use OpenSSL to create certificates, we need the configuration file ``openssl.cnf``.
- By default, OpenSSL use the configuration file from ``/usr/lib/ssl/openssl.cnf``. Since we need
to make changes to this file, we make a copy it into our current directory, and instruct OpenSSL to use this
copy instead.
- Next we uncomment the ``unique_subject`` line to allow creation of certifications with the same subject and create several sub-directories, ``index.txt`` and ``serial``.
- For the ``index.txt`` file, we create an empty file. For the ``serial`` file, we put a single number in string format (e.g. 1000) in the file.
- After the set up of the configuration file ``openssl.cnf`` we run the following command to generate the self-signed certificate for the CA: ``openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \-keyout ca.key -out ca.crt``.
- Next we insert a password and the subject information.
- The output of the command are stored in two files: ca.key and ca.crt. The file ca.key contains the CA’s private key, while ca.crt contains the public-key certificate.

#### Question 1: What part of the certificate indicates this is a CA’s certificate?

- If we run ``openssl x509 -in ca.crt -text -noout`` we can find :

```
X509v3 Basic Constraints: critical
                CA:TRUE
    Signature Algorithm: sha256WithRSAEncryption
```


#### Question 2: What part of the certificate indicates this is a self-signed certificate?

- If we run ``openssl x509 -in ca.crt -text -noout`` we can find that the issuer and subject fields are the same.

```
Issuer: C = PT, ST = oPorto, L = oPorto, O = FSI, OU = L10G03 ...
        Validity
            Not Before: Jan 21 10:13:18 2022 GMT
            Not After : Jan 19 10:13:18 2032 GMT
        Subject: C = PT, ST = oPorto, L = oPorto, O = FSI, OU = L10G03 ...
```

```
X509v3 extensions:
            X509v3 Subject Key Identifier: 
                3A:09:85:9A:D0:19:D6:92:85:15:AF:5B:35:16:E9:F8:90:4A:60:DE
            X509v3 Authority Key Identifier: 
                keyid:3A:09:85:9A:D0:19:D6:92:85:15:AF:5B:35:16:E9:F8:90:4A:60:DE
```

#### Question 3: In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq. Please identify the values for these elements in your certificate and key files.

- If we run ``openssl rsa -in ca.key -text -noout`` we can find:

- public exponent: ``65537 (0x10001)``

- private exponent:
```
    00:c0:ad:21:96:4a:8e:e1:1b:b4:45:d8:cb:b1:fc:
    bd:39:91:45:09:5a:5f:26:ee:8a:38:fb:8d:fb:d2:
    f1:70:3c:33:97:2f:60:7c:6d:f9:f8:71:84:9e:18:
    1e:cb:6d:d6:08:3c:2f:73:b8:8f:bd:37:ca:eb:bf:
    d7:fd:06:a9:0b:bf:e3:cc:6b:5a:02:90:d9:9d:29:
    1b:6c:0e:79:f4:9f:e3:bd:d7:5b:88:ae:67:e1:ee:
    23:c3:bf:61:48:9c:6f:18:4d:27:cf:c6:71:43:fc:
    7a:a5:ac:5d:59:5e:c9:bb:1b:33:55:89:e2:cd:f1:
    df:2a:0d:08:96:0a:0a:9b:14:6e:1a:b3:b6:f8:74:
    6a:0a:b4:dd:d1:01:c1:4d:85:69:1a:e2:75:9e:98:
    8e:9e:58:11:c6:fb:af:fe:0d:1c:84:31:5f:59:60:
    2a:b8:74:8e:cc:90:5d:9a:6c:31:44:62:5a:51:40:
    cf:7c:d1:e1:7b:e7:79:25:54:75:8b:4e:b1:98:57:
    00:3f:fa:ba:aa:a0:f0:f3:3b:8f:2a:d5:e3:a8:f8:
    51:55:d7:58:65:00:3e:ec:00:68:b6:21:f4:53:de:
    4e:0c:86:ad:1a:1a:28:c1:55:e8:43:b6:8a:44:54:
    6b:c7:dd:05:8a:7c:e6:fa:cd:64:1f:63:70:10:21:
    ca:a7:0f:e4:0b:0b:35:38:4e:f2:3b:d3:8d:3d:ed:
    b5:30:51:7f:3e:d8:d7:93:ef:f6:43:a8:9a:40:d0:
    26:5f:c9:53:d2:30:52:2a:99:58:dc:7d:d0:13:a5:
    38:e7:0d:1a:6d:a4:9c:5c:13:d6:e8:be:1d:92:ad:
    42:71:10:ce:56:7b:51:1c:03:39:22:8d:bf:f7:5f:
    ee:33:00:29:51:f6:0a:2b:b7:57:f2:08:e1:08:79:
    43:f6:30:38:89:27:91:1c:81:4a:d5:88:19:3f:f4:
    01:19:a2:8c:df:c7:f5:3f:d7:12:2f:bd:fb:44:aa:
    15:03:87:da:29:f9:44:64:61:95:35:d4:ea:6c:0b:
    07:43:a0:62:a7:68:da:cd:ef:f7:4e:16:45:3c:ab:
    af:ec:f4:68:e8:c4:16:04:55:7e:5b:05:f0:3f:19:
    ce:9b:4c:86:f5:12:5e:cf:5a:5d:e1:76:b8:70:09:
    7b:3d:8f:9b:bf:25:44:a5:57:4e:11:ae:2c:c1:51:
    84:d2:b1:ea:b7:71:a2:02:62:bc:f5:14:61:59:34:
    17:3c:48:48:7c:89:ab:5d:c7:d6:fa:ba:bf:8f:31:
    a7:8f:81:e2:0e:4c:8d:7a:79:81:cd:bd:95:a5:3d:
    a8:1e:1b:c1:65:a9:2a:8d:4e:1e:60:84:a3:09:5a:
    04:c6:31
```

- modulus:
```
    00:f4:27:8c:e7:60:16:3b:a3:6c:e1:0b:4c:55:ca:
    e2:6d:22:dd:56:e6:b1:ac:eb:ad:41:a0:ed:b5:12:
    fa:9b:06:d8:41:3b:cc:15:d4:bd:92:0a:c4:23:cd:
    00:5b:ee:67:d9:01:a0:7f:63:86:01:69:d0:27:18:
    f5:ee:70:f4:92:a8:75:c9:9d:f4:bb:d9:a1:3d:98:
    20:38:1e:e0:87:8e:1d:c1:cb:c9:41:50:09:58:d0:
    98:11:66:3b:dc:f6:b7:b6:21:4b:07:73:63:f9:46:
    e1:32:af:af:2b:05:05:06:07:4e:e1:0e:06:49:12:
    9a:1b:83:19:69:aa:c1:de:d2:4d:db:40:69:13:62:
    aa:d5:7b:05:45:5c:1a:59:6a:cb:aa:bd:f7:85:6e:
    ed:38:49:b1:8f:75:47:f1:c5:0f:6f:60:1e:4e:78:
    dd:84:84:a1:be:fc:20:07:7e:36:cc:23:88:87:71:
    54:75:00:36:df:68:fe:f5:a5:e0:be:e2:df:ef:69:
    40:44:b7:e3:a2:63:10:4f:7f:28:74:f5:ae:9b:fd:
    19:d5:7b:ff:cd:c0:a6:d9:94:c2:b8:09:ac:9a:be:
    9b:82:00:37:48:93:5c:4c:00:bd:91:07:66:5f:23:
    98:40:c9:8b:45:53:11:fd:bc:1c:df:09:74:d6:a7:
    44:b5:e1:d1:8c:30:64:0b:a5:e0:d0:f5:48:f2:f6:
    e8:6a:0d:1f:79:62:31:a8:24:f8:1f:dd:03:80:2b:
    9b:53:bc:a6:2a:cd:47:05:1d:07:b8:6c:ae:6c:a1:
    1d:e4:66:2e:be:10:31:99:2f:3d:10:0a:d4:46:e9:
    34:a1:7d:16:38:ef:85:b5:c8:62:bc:5d:b0:53:fe:
    a6:8d:9b:3f:f2:a3:8c:94:25:71:68:47:53:81:34:
    f6:5c:f9:b5:cb:c5:68:2d:a5:96:24:34:4b:24:fc:
    ab:31:b7:ec:48:7d:07:67:19:aa:c6:0c:ed:3e:4f:
    9a:c3:29:84:b4:d7:82:5b:ee:b8:2c:ea:c7:bb:6c:
    19:3c:5b:ee:ed:e9:fd:89:51:fc:0b:e3:28:d0:aa:
    29:e5:34:98:de:00:3d:d9:27:e4:1b:a3:de:63:cb:
    c8:30:f4:d1:de:22:50:41:b8:55:70:30:97:7e:a7:
    c4:3f:bf:91:a7:0c:69:7d:18:9d:3d:e5:d4:ac:bf:
    57:e6:3b:30:1e:90:7b:40:3a:bc:0a:3f:ae:c8:ce:
    18:c0:2d:6b:d7:b4:6e:91:f7:d4:11:74:4a:89:ab:
    61:8c:cc:cb:1b:5f:c0:58:23:f8:7a:25:04:65:6d:
    01:5e:bf:e4:65:dc:a8:5a:bb:09:1f:b6:fe:88:e5:
    1d:68:b9
```

prime1:
```
    00:fc:79:ac:fb:c3:6c:9d:b9:42:19:93:f4:9c:82:
    d8:7a:71:55:a0:0a:57:d0:67:50:29:f1:08:cd:df:
    9a:25:b7:3a:8e:a1:02:40:8e:7b:fb:b1:8c:68:02:
    51:b0:ae:24:90:4a:b8:e1:39:1c:aa:6b:7d:4f:0b:
    75:7a:ad:b8:3a:a8:62:44:9b:cb:9e:83:c2:48:49:
    e8:9e:9f:c9:b6:f1:0a:c4:ae:b1:97:cf:e8:b6:a8:
    7a:f2:bc:99:4c:02:47:f6:e7:49:12:da:97:f0:25:
    64:f7:33:83:12:25:a5:6b:be:88:3e:3f:79:cf:4d:
    84:08:8a:db:10:33:71:1c:f5:23:66:27:bd:c0:03:
    49:96:ba:08:4d:ac:35:51:93:98:a4:db:d8:2d:8e:
    1a:70:a1:00:47:f9:53:82:3d:46:1b:d0:de:e9:7e:
    ac:65:ad:a4:3f:2b:69:21:3f:29:b4:5b:93:30:ba:
    fb:6d:69:69:50:20:d2:71:66:60:93:06:eb:64:42:
    55:53:e1:35:89:92:bc:e6:1d:45:01:4c:73:e2:f1:
    4b:cf:00:dc:5a:26:a2:34:d0:48:74:96:1b:57:39:
    c2:18:41:5a:e1:67:5b:7b:ad:13:5d:ea:ea:a0:40:
    c2:06:76:f8:61:d6:0d:dc:39:79:7d:ae:1c:09:12:
    c7:dd
```
prime2:
```
    00:f7:90:23:0a:ab:e1:27:de:cd:3b:66:d1:8f:f9:
    1b:a2:92:42:b4:4c:02:08:05:44:8a:70:80:41:08:
    85:95:4f:cf:15:cb:ba:af:6e:ce:bf:f9:6b:2b:0a:
    e4:0c:a8:0f:95:1b:5d:9d:10:6f:ab:e9:92:b5:9f:
    32:e9:c6:a0:90:b6:8a:ef:b1:45:6c:8b:49:d6:21:
    c9:54:e3:aa:3b:50:67:89:d5:ff:0b:02:df:ac:e2:
    87:63:dc:fb:36:bf:50:c0:01:78:2c:8e:db:c6:4d:
    de:5b:2b:2b:7d:1a:1d:c3:ac:16:72:0c:36:45:6b:
    93:7a:bc:d4:82:82:43:77:46:b0:c9:70:ed:9c:dd:
    0e:86:97:07:38:ec:94:96:8a:3a:3e:47:7d:d2:10:
    78:9a:08:b3:17:88:77:5a:ba:4c:99:19:42:86:bc:
    95:fb:5b:b7:34:90:6e:8f:21:a9:28:36:15:cb:d9:
    9e:a1:98:6c:da:50:c2:f2:79:70:94:5c:ac:91:f3:
    cd:5d:f0:4c:2a:51:2a:c4:ff:d3:da:99:35:20:ed:
    45:c0:f5:cc:f2:80:e1:cf:e9:07:4d:bb:7f:e6:49:
    0f:0c:da:e3:78:8f:f6:ea:69:b5:41:5a:74:f7:a9:
    20:5e:d9:1d:2e:50:e1:3b:ab:e2:38:7c:1b:84:cf:
    64:8d
```

### Task 2

- In this task we want to generate a certificate request for our web server.
- The command ``openssl req -newkey rsa:2048 -sha256 \-keyout server.key -out server.csr \-subj "/CN=www.l10g03.com/O=L10G03 Inc./C=US" \-passout pass:dees`` generate a CSR for www.l10g03.com.
- The command generate a pair of public/private key, and create a certificate signing request from the public key.
- We can use the following command to look at the decoded content of the CSR and private key files:

```
openssl req -in server.csr -text -noout
openssl rsa -in server.key -text -noout
```
- To allow a certificate to have multiple names, the X.509 specification defines ``Subject Alternative Name (SAN)`` to be attached to a certificate. - Using the SAN extension, we specify several hostnames in the ``subjectAltName`` field of a certificate.
- We add that option to the "openssl req" command ``-addext "subjectAltName = DNS:www.l10g03.com,  \DNS:www.l10g032022.com, \DNS:www.l10g03fridaymonday.com"``
- Getting the command ``openssl req -newkey rsa:2048 -sha256 \-keyout server.key -out server.csr \-subj "/CN=www.l10g03.com/O=L10G03 Inc./C=US" \-passout pass:dees -addext "subjectAltName = DNS:www.l10g03.com,  \DNS:www.l10g032022.com, \DNS:www.l10g03fridaymorning.com"``.


### Task 3

- In this task we want to generate a certificate for our server.
- The default setting in ``openssl.cnf`` does not allow the ``openssl ca`` command to copy the extension field from the request to the final certificate. To enable that, we uncomment the line ``copy_extensions = copy`` of our copy of the configuration file.
- We use the command ``openssl ca -config myCA_openssl.cnf -policy policy_anything \-md sha256 -days 3650 \-in server.csr -out server.crt -batch \-cert ca.crt -keyfile ca.key `` to turn the certificate signing request (server.csr) into an X509 certificate (server.crt), using the CA’s ca.crt and ca.key.
- After signing the certificate,  we use the command ``openssl x509 -in server.crt -text -noout`` to print out the decoded content of the certificate, and check whether the alternative names are included:

```
X509v3 Subject Alternative Name: 
            DNS:www.l10g03.com, DNS:www.l10g032022.com, DNS:www.l10g03fridaymorning.com
```

### Task 4

- 

### Task 5

- 

### Task 6

- 


