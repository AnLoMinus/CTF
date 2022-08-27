![image](https://user-images.githubusercontent.com/51442719/187024308-0a3c7205-646e-485c-9cb3-ab48f1366894.png)

# [b3dr0ck](https://tryhackme.com/room/b3dr0ck)
#### Server trouble in Bedrock.

- [ ] Task 1  Yabba-Dabba-Doo


---


## Task 1  Yabba-Dabba-Doo

### Fred Flintstone   &   Barney Rubble!

##### Barney is setting up the ABC webserver, and trying to use TLS certs to secure connections, but he's having trouble. Here's what we know...

- He was able to establish nginx on port 80,  redirecting to a custom TLS webserver on port 4040
- There is a TCP socket listening with a simple service to help retrieve TLS credential files (client key & certificate)
- There is another TCP (TLS) helper service listening for authorized connections using files obtained from the above service
- Can you find all the Easter eggs?

##### Please allow an extra few minutes for the VM to fully startup﻿.

### Answer the questions below

---

#### What is the barney.txt flag?
- Answer format: `***{********************************}`

### My Steps
- 1 run as sudo 
  ```sh 
  sudo -s 
  ```
- 2 adding dns for {targetIP}
  ```sh
  echo "10.10.85.120    bedrock.thm" >> /etc/host
  ```
- 3 run nmap scan 
  ```sh
  nmap -A -T4 --script vuln 10.10.85.120 --system-dns > nmap-bedrock.log
  ```
  - open log file
  ```sh
  less nmap-bedrock.log
  ```
  
```shell
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-27 05:37 EDT
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Stats: 0:01:03 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 66.67% done; ETC: 05:38 (0:00:14 remaining)
Stats: 0:02:41 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 66.67% done; ETC: 05:40 (0:01:02 remaining)
Stats: 0:02:51 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 66.67% done; ETC: 05:41 (0:01:07 remaining)
Stats: 0:06:20 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.75% done; ETC: 05:43 (0:00:00 remaining)
Nmap scan report for bedrock.thm (10.10.85.120)
Host is up (0.076s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-passwd: ERROR: Script execution failed (use -d to debug)
|_http-vuln-cve2013-7091: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
9009/tcp open  pichat?
| fingerprint-strings: 
|   NULL: 
|     ____ _____ 
|     \x20\x20 / / | | | | /\x20 | _ \x20/ ____|
|     \x20\x20 /\x20 / /__| | ___ ___ _ __ ___ ___ | |_ ___ / \x20 | |_) | | 
|     \x20/ / / _ \x20|/ __/ _ \| '_ ` _ \x20/ _ \x20| __/ _ \x20 / /\x20\x20| _ <| | 
|     \x20 /\x20 / __/ | (_| (_) | | | | | | __/ | || (_) | / ____ \| |_) | |____ 
|     ___|_|______/|_| |_| |_|___| _____/ /_/ _____/ _____|
|_    What are you looking for?
1 service unrecognized despi
```

- Try open with nc `${targetIP} 9009`
```sh
nc 10.10.85.120 9009
```
  - we get
  ```sh
   __          __  _                            _                   ____   _____ 
 \ \        / / | |                          | |            /\   |  _ \ / ____|
  \ \  /\  / /__| | ___ ___  _ __ ___   ___  | |_ ___      /  \  | |_) | |     
   \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \    / /\ \ |  _ <| |     
    \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) |  / ____ \| |_) | |____ 
     \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/  /_/    \_\____/ \_____|
                                                                               
                                                                               


What are you looking for? 
  ```
  ask for client certificate
  ```sh
What are you looking for? client certificate
  ```


---

#### What is fred's password?
- Answer format: `****************`

#### What is the fred.txt flag?
- Answer format: `***{********************************}`

#### What is the root.txt flag?
- Answer format: `***{********************************}`
