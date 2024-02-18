+++
author = "causehhc"
title = "Dedicated server use OpenVPN for LAN Multiplayer Game"
date = "2024-02-18"
description = ""
tags = ["Ubuntu"]
categories = ["中间件"]

+++

## 1. Overview
```log
SERVER
|          |          | 
CLIENT_1 - CLIENT_2 - CLIENT_3
```
## 2. Server
### 2.1 Install OpenVPN
`sudo apt-get install openvpn`
### 2.2 Establish PKI
OpenVPN Needs to establish a PKI (public key infrastructure), Simply, you need to have `ca.crt`, `server.crt`, `server.key`, `dh2048.pem` under `/etc/openvpn/`.

A sample file will be used here (it works but is **not safe**), please refer to `6.Link/OpenVPN detail` for details.

- ca.crt: `sudo cp /usr/share/doc/openvpn/examples/sample-keys/ca.crt /etc/openvpn/`
- server.crt: `sudo gunzip -c /usr/share/doc/openvpn/examples/sample-keys/server.crt.gz > /etc/openvpn/server.crt`
- server.key: `sudo cp /usr/share/doc/openvpn/examples/sample-keys/server.key /etc/openvpn/`
- dh2048.pem: `sudo cp /usr/share/doc/openvpn/examples/sample-keys/dh2048.pem /etc/openvpn/`

### 2.3 Define config
Edit `/etc/openvpn/server.conf`, similarly, the sample file is in `/usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz`

*An available configuration for "Sid Meier's Civilization® VI":*
```log
port 1194
proto udp
dev tap
ca ca.crt
cert server.crt
key server.key
dh dh2048.pem
server 10.8.0.0 255.255.255.0
client-to-client
duplicate-cn
keepalive 10 120
user nobody
group nogroup
persist-key
persist-tun
verb 3
comp-lzo no
```

### 2.4 Start Server
```bash
cd /etc/openvpn
sudo openvpn --config server.conf
```

### 2.5 Check Status
Execute `ifconfig` command and you can see:
```log
tap0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.8.0.1  netmask 255.255.255.0  broadcast 10.8.0.255
        inet6 fe80::8053:cfff:fe8f:5df2  prefixlen 64  scopeid 0x20<link>
        ether 82:53:cf:8f:5d:f2  txqueuelen 100  (Ethernet)
        RX packets 270745  bytes 66208047 (66.2 MB)
        RX errors 0  dropped 921  overruns 0  frame 0
        TX packets 4355  bytes 304866 (304.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## 3. Client
*Every client needs to do this, Take win10 as an example*
### 3.1 Install OpenVPN
Download and Install from: https://openvpn.net/community-downloads/
### 3.2 Establish PKI
Client also Needs to establish a PKI (public key infrastructure), Simply, you need to have `ca.crt`, `client.crt`, `client.key`, under `$INSTALL_PATH\OpenVPN\config\`.

A sample file will be used here (it works but is **not safe**), please refer to `6.Link/OpenVPN detail` for details.

Copy from Server on `/usr/share/doc/openvpn/examples/sample-keys/`:

<!-- ![client_pki](./static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_pki.png) -->
![client_pki](../../../posts/static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_pki.png)

### 3.3 Define config
Edit `$INSTALL_PATH\OpenVPN\config\client.ovpn`, similarly, the sample file is in `$INSTALL_PATH\OpenVPN\sample-config\client.ovpn`

*An available configuration for "Sid Meier's Civilization® VI":*
```log
client
dev tap
proto udp
remote $YOUR_SERVER_IP 1194
ca ca.crt
cert client.crt
key client.key
user nobody
group nogroup
persist-key
persist-tun
verb 3
comp-lzo no
key-direction 1
```

### 3.4 Start Client
Start OpenVPN GUI from WIN_START and Right click on the connect button.

<!-- ![client_start](./static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_start.png) -->
![client_start](../../../posts/static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_start.png)

### 3.5 Check Status
After the connection is successful, there will be a prompt and the IP address assigned to the client will be reported.

<!-- ![client_start_ok](./static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_start_ok.png) -->
![client_start_ok](../../../posts/static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_start_ok.png)

## 4. Verify
If both clients are started, you can verify whether they belong to the same LAN by ping.
<!-- ![client_ping](./static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_ping.png) -->
![client_ping](../../../posts/static/Dedicated%20server%20use%20OpenVPN%20for%20LAN%20Multiplayer%20Game/client_ping.png)

## 5. Other
*There are other steps you need to take for specific games like:*
### 5.1 Sid Meier's Civilization® VI
Each client needs to use WinIPBroadcast and turn off the firewall.
- detail: https://blog.yllhwa.com/2023/02/24/%E6%96%87%E6%98%8E6%E8%81%94%E6%9C%BA%E6%96%B9%E6%B3%95%E4%B8%8E%E5%AE%9E%E8%B7%B5/
### 5.2 Stellaris
Nothing needs special attention.
### 5.3 7 Days To Die
It is more recommended to use Frp.
### 5.4 Cities Skylines
It is more recommended to use Frp.
### 5.5 RimWorld
Nothing needs special attention.

## 6. Link
- Old method use Frp: https://causehhc.github.io/2021/04/use-frp-to-achieve-intranet-penetration/
- OpenVPN detail: https://openvpn.net/community-resources/how-to/
- OpenVPN setup example1: https://zhou-yuxin.github.io/articles/2017/Linux%20%E6%90%AD%E5%BB%BAOpenVPN%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%92%8C%E5%AE%A2%E6%88%B7%E7%AB%AF%EF%BC%88%E4%B8%80%EF%BC%89%E2%80%94%E2%80%94%E6%9C%80%E7%AE%80%E9%85%8D%E7%BD%AE/index.html
- OpenVPN setup example2: https://blog.linux2.win/openvpn/

