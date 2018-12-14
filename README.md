cisco_pwdecrypt
===============

Originally developed to decrypt the *"enc_GroupPwd"* variable in PCF files. This tool has evolved and can also decode Cisco *type 7* and bruteforce Cisco *type 5* passwords (using dictionary attacks).

## Description

This tool is a Python port of Maurice Massar's tool (https://www.unix-ag.uni-kl.de/~massar/soft/cisco-decrypt.c)

The PCF files (.pcf) contain the VPN informations. They are generated by the Cisco VPN Client when you add a VPN profile.
Those profiles contain, among other data, the IP, the group name and the VPN shared secret.

Here is an example of a PCF file:

```bash
[main]
Description=
Host=192.168.1.1
AuthType=1
GroupName=group_test
GroupPwd=
enc_GroupPwd=886E2FC74BFCD8B6FAF47784C386A50D0C1A5D0528D1E682B7EBAB6B2E91E792E389914
767193F9114FA26C1E192034754F85FC97ED36509
EnableISPConnect=0

[...]

PeerTimeout=90
EnableLocalLAN=0
```

## Requirements

This tool requires the [pyCrypto](https://www.dlitz.net/software/pycrypto/) and [passlib](https://bitbucket.org/ecollins/passlib/wiki/Home) modules.

```bash
$ pip install pyCrypto passlib
```
**OR**
```bash
$ pip install -r requirements.txt
```

> **Note:** If you are using Microsoft Windows, **pyCrypto** could requires Microsoft Visual C++ 10.0.

## Getting Started

```bash
usage: cisco_pwdecrypt.py [-h] [-p PCFVAR] [-f PCFFILE] [-t TYPE7] [-u TYPE5]
                          [-d DICT]

Simple tool to decrypt Cisco passwords

optional arguments:
  -h, --help            show this help message and exit
  -p PCFVAR, --pcfvar PCFVAR
                        enc_GroupPwd Variable
  -f PCFFILE, --pcffile PCFFILE
                        .pcf File
  -t TYPE7, --type7 TYPE7
                        Type 7 Password
  -u TYPE5, --type5 TYPE5
                        Type 5 Password
  -d DICT, --dict DICT  Password list

$ python3 cisco_pwdecrypt.py -p 886E2FC74BFCD8B6FAF47784C386A50D0C1A5D0528D1E682B7EBAB6
B2E91E792E389914767193F9114FA26C1E192034754F85FC97ED36509
[*] Result: Th!sIsMyK3y#

$ python3 cisco_pwdecrypt.py -f BreakInSecurity_VPN.pcf
[*] Result: Th!sIsMyK3y#
```

### Cisco Type 7

```bash
$ python3 cisco_pwdecrypt.py -t 01270E454822152238671D105A
[*] Result: Th!sIsMyK3y#
```

### Cisco Type 5

**Note**: When bruteforcing Cisco *Type5* passwords, you have to escape the **'$'** sign in the password with a backslash. It's not a Python issue, this is because most shells consider strings starting with **'$'** as a variable.

```bash
$ python3 cisco_pwdecrypt.py -u "\$1\$VkQd\$Vma3sR7B1LL.v5lgy1NYc/" -w passwords.txt
[*] Bruteforcing 'type 5' hash...

        Found 10000 passwords to test.
        Testing: $1$VkQd$Vma3sR7B1LL.v5lgy1NYc/
        Hash Type = MD5
        Salt = VkQd
        Hash = Vma3sR7B1LL.v5lgy1NYc/

        [Status] 60/10000 password tested...
        [Status] 106/10000 password tested...
        [Status] 112/10000 password tested...
        [Status] 159/10000 password tested...
        [Status] 840/10000 password tested...
        [Status] 919/10000 password tested...
        [Status] 933/10000 password tested...

[*] Password Found = Password123
```

## Resources

Here are some interesting resources for this project :

- https://github.com/berzerk0/Probable-Wordlists (some wordlists)
- [Cisco Routers Password Types](https://learningnetwork.cisco.com/docs/DOC-27166)

## License

This project is released under the Apache 2 license. See LICENCE file.