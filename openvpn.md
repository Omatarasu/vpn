
# server
```
local 192.168.140.10                       # address 
port 1194                                  # port 
proto udp                                  # protocol ( add to firewall )

dev tun                                    # L3 tunnel

ca /etc/openvpn/key/ca.crt                 # certificate authority
cert /etc/openvpn/key/server.crt           # server cert 
                                           # 1. keyUsage = digitalSignature, keyEncipherment 2. extendedKeyUsage = serverAuth
key /etc/openvpn/key/server.key            # server key
dh /etc/openvpn/key/dh.pem                 # dh key
                                           # openssl dhparam -out dh.pem 4096
topology subnet                            # L3 topology

server 10.8.0.0 255.255.255.0              # Tunnel lan pool

push "route 192.168.130.0 255.255.255.0"   # Push routes to client

keepalive 5 40                             # ping every 5 seconds and if dead restart after 40 seconds


remote-cert-tls client                     # client extendedKeyUsage = clientAuth

 
 
 
 
auth SHA512                                # hash algorythm 
tls-auth /etc/openvpn/key/ta.key 0         # Preshared key
                                           # openvpn --genkey --secret ta.key
                                           # 0 - on server, 1 - on client

cipher AES-256-CBC                         # Cipher 

persist-key                                # for non-root users
persist-tun                                # for non-root users


status      /etc/openvpn/log/openvpn-status.log    # log
log         /etc/openvpn/log/openvpn.log           # log
log-append  /etc/openvpn/log/openvpn-append.log    # log

verb 3                                             # 0 is silent, except for fatal errors
                                                   # 4 is reasonable for general usage
                                                   # 5 and 6 can help to debug connection problems
                                                   # 9 is extremely verbose 
```
# Client 
```
client                                             # client
dev tun                                            # L3 tunnel
proto udp                                          # protocol udp 

remote 192.168.140.10 1194                         # server address

resolv-retry infinite                              # unless try to resolve server hostname

nobind                                             # random port for connection

;user nobody                                       # privilage after up tunnel 
;group nogroup                                     # privilage after up tunnel

persist-key                                        # for non-root users
persist-tun                                        # for non-root users

ca /etc/openvpn/key/ca.crt                         # Certificate Authority
cert /etc/openvpn/key/client.crt                   # Client cert 
                                                   # 1. keyUsage = digitalSignature, keyEncipherment 2. extendedKeyUsage = clientAuth
key /etc/openvpn/key/client.key                    # Client key

remote-cert-tls server                             # server extendedKeyUsage = serverAuth

auth SHA512                                        # hash for auth
tls-auth /etc/openvpn/key/ta.key 1                 # Preshared key
                                                   # openvpn --genkey --secret ta.key
                                                   # 0 - on server, 1 - on client

cipher AES-256-CBC                                 # Cipher 

pull                                               # Pull configs from server

status      /etc/openvpn/log/openvpn-status.log    # log
log         /etc/openvpn/log/openvpn.log           # log 
log-append  /etc/openvpn/log/openvpn-append.log    # log

verb 3                                             # 0 is silent, except for fatal errors
                                                   # 4 is reasonable for general usage
                                                   # 5 and 6 can help to debug connection problems
                                                   # 9 is extremely verbose

```
