Using VPN
```
sudo openvpn user.ovpn
```

- `openvpn` is the VPN client,
- `user.ovpn` file is the VPN key

Show IP
```
ifconfig
```

Show accessible Networks
```
netstat -rn
```
Here can see that the 10.129.0.0/16 network used for HTB Academy machines is accessible via the `tun0` adapter via the 10.10.14.0/23 network.
![[Pasted image 20250215221232.png]] 



