# Initial configuration

|Hostname|System|Ip Address|
|:---:|:---:|:---:|
|srv|CentOS 8|192.168.122.60/24|
|vyos|vyos|192.168.122.50/24|
|cli|Ubuntu 20.04 Desktop|192.168.122.40/24|
# Topology
![1](https://user-images.githubusercontent.com/62337797/158861671-439ee359-e838-4c7e-a116-cabc3fc2d273.png)
---
![2](https://user-images.githubusercontent.com/62337797/158863199-85f32373-6b68-47f0-8ed6-d9f997bf28fe.png)

### Install 
```
apt install wireguard -y
```
> cli
```
sudo dnf install elrepo-release epel-release -y
sudo dnf install kmod-wireguard wireguard-tools -y
```
> srv

### Generate key pairs
```
cd /etc/wireguard
wg genkey | tee srv-priv.key | wg pubkey > srv-pub.key
```
> srv key generation
```
cd /etc/wireguard
wg genkey | tee cli-priv.key | wg pubkey > cli-pub.key
```
> cli key generation
```
cd /etc/wireguard
wg genkey | tee vyos-priv.key | wg pubkey > vyos-pub.key
```
> vyos key generation
