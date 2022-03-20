# Initial configuration

|Hostname|System|Ip Address|
|:---:|:---:|:---:|
|vyos|vyos|192.168.70.10/24|
|CentOS|CentOS 8|192.168.70.20/24|
|Ubuntu Desktop|Ubuntu 20.04 Desktop|192.168.70.30/24|
# Topology
![1](https://user-images.githubusercontent.com/62337797/159183828-4952d966-9e09-492b-9ce8-a75aefebbcdc.png)

---

![2](https://user-images.githubusercontent.com/62337797/159183830-29e3e43f-96e7-4901-ba54-c26cd60a36df.png)

### Install 
[link](https://www.wireguard.com/install/)

### Generate key pairs
```
umask 077 /etc/wiregurd
cd /etc/wireguard
wg genkey | tee priv.key | wg pubkey > pub.key
wg genpsk > psk.key # only on client
```
### Configuration 

#### vyos (server) to centos
```
set interfaces wireguard wg0 address '10.0.0.1/30'
set interfaces wireguard wg0 port '51820'
set interfaces wireguard wg0 private-key 'UHjo/2X2F89QkfAb7RnUK0A8hGOi7EZi2eTFKc2fs1Q='
```
> Interface configuration
```
set interfaces wireguard wg0 peer centos allowed-ips '10.0.0.2/32'  # ip r 
set interfaces wireguard wg0 peer centos preshared-key 'IdrbZlpw7tXRuu3Rv+iYLQ9gnwjoxoq6gjan2osuNp8='
set interfaces wireguard wg0 peer centos public-key 'AZYFjr+jb8yYpjkiT0/ZZmx81p0ttT3UNSXYV2Bm538='
```
> Peer configuration
```
commit 
save
```
> start wireguard
#### centos to vyos (server)
```
[Interface]
Address = 10.0.0.2/32
PrivateKey = sLwb/65WouPOcc2DkQqKShLbOZKny9hsk0SZ195nWls=

[Peer]
PublicKey = /o0yFkW8OPgz2nwYeSiiaaEYn+LNTBzmJTYwx2vcbzA=
PresharedKey =  IdrbZlpw7tXRuu3Rv+iYLQ9gnwjoxoq6gjan2osuNp8=

AllowedIPs = 192.168.10.0/24, 10.0.0.0/30 # ip r
Endpoint = 192.168.70.10:51820 # vyos server
```
> /etc/wireguard/wg0.conf
```
systemctl start wg-quick@wg0.service
```
> start wiregurd
