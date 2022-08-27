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
  ask for client certificate COPY OUTPUT INTO NEW `client-cert` file
  ```sh
 __          __  _                            _                   ____   _____ 
 \ \        / / | |                          | |            /\   |  _ \ / ____|
  \ \  /\  / /__| | ___ ___  _ __ ___   ___  | |_ ___      /  \  | |_) | |     
   \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \    / /\ \ |  _ <| |     
    \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) |  / ____ \| |_) | |____ 
     \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/  /_/    \_\____/ \_____|
                                                                               
                                                                               


What are you looking for? client certificate
Sounds like you forgot your certificate. Let's find it for you...

-----BEGIN CERTIFICATE-----
MIICoTCCAYkCAgTSMA0GCSqGSIb3DQEBCwUAMBQxEjAQBgNVBAMMCWxvY2FsaG9z
dDAeFw0yMjA4MjcwOTMwMTFaFw0yMzA4MjcwOTMwMTFaMBgxFjAUBgNVBAMMDUJh
cm5leSBSdWJibGUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCmIMJM
RFiUGyzVutsyzXE2Qag4hCALGhLY1Ypgl1QX2P6Ljc6SrbIC5t19ACWCY/3dxQK1
MxIg4HfmO6KIF6D2dBbHDSckzLwUo08llAkaY74eyLzP/Kf3FxNx1f9gn7FarLpl
sX/zaQAwLUqyc0E+d8mrSNFwoQmqD+lqrfZZ5j+1kpvzMvYnM62+t0RlngK4kGeI
DHGmEk22bD4+xfq+1rXwPqayIMvouGCyDsvMAtG/H/JE02ZKx87rwFvoeLwPsXkb
WNhaVxJR+n+oUSfLhbgHPlvg2ZkJe4wv+sBUDpGBb0IcSG5GoxIcEfSljMtI1+uy
c+tyj64u2eXIkZe1AgMBAAEwDQYJKoZIhvcNAQELBQADggEBAEeEfrQDa5oO0VAa
eE5QV335yy9SRxAzHyX10RTrOJtuO4KqI70Z+07xG4i5Z1/3QFEbgp/xIdLh7WiX
IzFPyz3B8QgG6lur4hQMkS1y5Q3bvST7Cf5E6QDCdE8nEwIWnLz+WpScfg1vjyFa
JBvlBPBdVRBKq4bVFY+7LB6mIdxhaAhsV3mLjERHZhtDHWKyxGvfjnUQPYu+ZKuM
15SyOXYw8y65jmE2rvNOoSzFJVO88cSM1Upbj2kgVESf2WYeKUbsX5HX4KwgbVQ8
WaOMALFBBrD5xViuDxsEe1ebmQ03KqL54RHecML/bozgfsSkGlvavG6xuAXnqXQq
uIlaZqo=
-----END CERTIFICATE-----


What are you looking for? 
  ```

- ask for private, COPY OUTPUT INTO NEW `rsa-key` file
```sh
 __          __  _                            _                   ____   _____ 
 \ \        / / | |                          | |            /\   |  _ \ / ____|
  \ \  /\  / /__| | ___ ___  _ __ ___   ___  | |_ ___      /  \  | |_) | |     
   \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \    / /\ \ |  _ <| |     
    \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) |  / ____ \| |_) | |____ 
     \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/  /_/    \_\____/ \_____|
                                                                               
                                                                          
What are you looking for? private
Sounds like you forgot your private key. Let's find it for you...


-----BEGIN RSA PRIVATE KEY-----
MIIEpgIBAAKCAQEApiDCTERYlBss1brbMs1xNkGoOIQgCxoS2NWKYJdUF9j+i43O
kq2yAubdfQAlgmP93cUCtTMSIOB35juiiBeg9nQWxw0nJMy8FKNPJZQJGmO+Hsi8
z/yn9xcTcdX/YJ+xWqy6ZbF/82kAMC1KsnNBPnfJq0jRcKEJqg/paq32WeY/tZKb
8zL2JzOtvrdEZZ4CuJBniAxxphJNtmw+PsX6vta18D6msiDL6Lhgsg7LzALRvx/y
RNNmSsfO68Bb6Hi8D7F5G1jYWlcSUfp/qFEny4W4Bz5b4NmZCXuML/rAVA6RgW9C
HEhuRqMSHBH0pYzLSNfrsnPrco+uLtnlyJGXtQIDAQABAoIBAQCe0ozPGzxQBBb+
EpFDZXPJukWYGoED/B5unOCZbbOgxPy98InAYzzfV1YDDmPN38ix/4qSL0wykEcw
nmzJjUV+uQeZr3Jv1Sgu1t5w/7EgQKFfjuwsL9FpAe++Eif/eUy2cpIVbPf1froG
VRanulZy4VS1Y32QHvU9V88BBWWTFWoHkVndFswLNL28I/Fg+jeopq+BpqQEKR4e
n7sFTpe77mP5swtPX0RwNkxJNzRHcfGTO65WfnkzojEdsGJp2F1IkfsjTDQy7f7F
VKYVebJFfyR5mHlkYiJcoYZHfTQHS/6JNPIIuP5fwn+hwdq1sBlgdCowLhJW8UM6
9vB8NQVBAoGBANJhE3fJaXd1c0BpAiJYxeEeIAJu9HNmVZZTces3SPyYefi5LSJ1
qg7I/TwwJmTlLF6NyPSjE0u79S1tkF9IMl91wgQ5lMxYGlDjKlR4IahbngLHHWT+
1qdwWYfjqQ3zLaU32yAnbj5+OnNzbZgXWadOed+DtOFMQxIWzD3z31uxAoGBAMon
IkBbpfe+ApkTEYERPPv05hNORlcrpodpU+/gmsloR2+QFk4d/0sV33bEIB5wtuDe
cuL/21uTa/6wHVzu4CYukeluHB+49el3AOx1GDamlJsNkP2hE6XWx+laDciSISpv
zMpWHcIhpguKv3LtbgrJT2vxEJ/GJOLgaD/rITFFAoGBAI3yWAtbx6CFi7Tq5Ti9
gw5IoDpkGOYAJ0FdniCh1coxKyMJ9o0orQx6ynqg1lb/VjeaHPwLSAqykFQNd/sC
IJLORpFJNL/HtkHbdIU35SXOY0fmh0vMspKZOJ96mWdDLAotLNl+IWFjFBcvy8Ny
BdjgF1UbbaESLrL21On8MTmRAoGBAIgFFu3Y/O6aomLfSrren3slCJ5a38eNrmqU
u46/QUdd7BssB2YelwWtvQPL6ZSx4Mujwgftgmq24kant8otTRND6Jf5p+DMcmLZ
2PxBub4kDf/afAG8nVzMDQ19s6KOeNR2D4ThtvpF69T+Ud2B1rZZSCBoPvhSucUS
m/LOQjJFAoGBALA0FQXrKnqqAQDChRC33HAVEPJ5g4Ck5746hgnL/LAHbR8/vOi1
Pc8vNEZ0iD1sqO3DAyT3/PI1CwYn+lYvYuFX22VBTTMhkEWl8/81Ju7RpNnn/Wjk
Yq+pcXM2VivL5KsLQgJIRESCEQ/3Fg/FUZ+5s1Y9px54xbGOeINjS+LQ
-----END RSA PRIVATE KEY-----
```


---

#### What is fred's password?
- Answer format: `****************`

#### What is the fred.txt flag?
- Answer format: `***{********************************}`

#### What is the root.txt flag?
- Answer format: `***{********************************}`
