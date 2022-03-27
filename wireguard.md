# client to vyos
```
[Interface]
Address = 10.0.0.2/32                                         # address in tunnel
PrivateKey = 4Fb9+VO/VT7oKgUwZ1NOHNOceA1+SHl0GiJTDOX2b1w=     # client private key

[Peer]
PublicKey = o2sMsvbUWhb+7xP2Or9ddZCZj8tIS2mdV5Qx/IblTA0=      # vyos public key 
Endpoint = 192.168.140.20:51820                               # vyos address
PresharedKey = HqUsKzxk45RCXDBi8MSJCbWcAQdNfTvPkIJuKKds28c=   # preshared key
AllowedIPs = 10.0.0.0/30, 192.168.120.0/24                    # ip routes 
```
> /etc/wireguard/wg0.conf
```
systemctl start wg-quick@wg0.service
```
> up tunnel
# client to rtr
```
[Interface]                                                   
Address = 10.0.0.6/32                                         # address in tunnel
PrivateKey = 4Fb9+VO/VT7oKgUwZ1NOHNOceA1+SHl0GiJTDOX2b1w=     # client private key

[Peer]
PublicKey = ZlWIeu33dTcsl/BsUp8U9fOu022o+9UUv4NEryVweCw=      # rtr public key
Endpoint = 192.168.140.10:51820                               # rtr address
PresharedKey = HqUsKzxk45RCXDBi8MSJCbWcAQdNfTvPkIJuKKds28c=   # preshared key
AllowedIPs = 10.0.0.4/30, 192.168.130.0/24                    # ip routes
```
> /etc/wireguard/wg1.conf
```
systemctl start wg-quick@wg1.service
```
> up tunnel
# rtr to client
```
[Interface]
Address = 10.0.0.5/30                                         # address in tunnel
ListenPort = 51820                                            # listening port ( add to firewall )
PrivateKey = QF/VgLZykw7MCjDkd6BH12qW3ZBORmGcDBZ3UKsWCnU=     # rtr private key

[Peer]
PublicKey = 8y5HfTNerH25zp1LB2Nc7WTfNCfbvvI30XtnGQE6qWY=      # client public key
AllowedIPs = 10.0.0.6/32                                      # ip routes
PresharedKey = HqUsKzxk45RCXDBi8MSJCbWcAQdNfTvPkIJuKKds28c=   # presharted key
```
> /etc/wireguard/wg0.conf
```
systemctl enable --now wq-quick@wg0.service
```
> up tunnel
# vyos to client 
```
set interfaces wireguard wg0 address '10.0.0.1/30'                                       # address in tunnel
set interfaces wireguard wg0 port '51820'                                                # listening port
set interfaces wireguard wg0 private-key 'CNeYP3gAvRlOfsG6S7S/vZglDTj70wAf+okZAN0SflI='  # vyso private key

set interfaces wireguard wg0 peer client allowed-ips '10.0.0.2/32'                                       # ip routes
set interfaces wireguard wg0 peer client preshared-key 'HqUsKzxk45RCXDBi8MSJCbWcAQdNfTvPkIJuKKds28c='    # preshared key
set interfaces wireguard wg0 peer client public-key '8y5HfTNerH25zp1LB2Nc7WTfNCfbvvI30XtnGQE6qWY='       # client public key
```
> configure wireguard
```
commit   # apply configuratin  
save     # save to config file
```
> up tunnel
