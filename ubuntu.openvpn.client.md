systemd管理的服务在/lib/systemd/system
ubuntu 18.04加入了openvpn-client@service
```
⋊> ~ cat /lib/systemd/system/openvpn-client@.service                                                                         11:03:23
[Unit]
Description=OpenVPN tunnel for %I
After=syslog.target network-online.target
Wants=network-online.target
Documentation=man:openvpn(8)
Documentation=https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage
Documentation=https://community.openvpn.net/openvpn/wiki/HOWTO

[Service]
Type=notify
PrivateTmp=true
WorkingDirectory=/etc/openvpn/client
ExecStart=/usr/sbin/openvpn --suppress-timestamps --nobind --config %i.conf
CapabilityBoundingSet=CAP_IPC_LOCK CAP_NET_ADMIN CAP_NET_RAW CAP_SETGID CAP_SETUID CAP_SYS_CHROOT CAP_DAC_OVERRIDE
LimitNPROC=10
DeviceAllow=/dev/null rw
DeviceAllow=/dev/net/tun rw
ProtectSystem=true
ProtectHome=true
KillMode=process

[Install]
WantedBy=multi-user.target
```
所以，讲client配置文件放在/etc/openvpn/client/下面，假设文件名是my-client.conf
那么可以通过下面命令开启服务
```
sudo systemctl start openvpn@emy-client
sudo systemctl status openvpn@emy-client
sudo systemctl enable openvpn@emy-client
```
