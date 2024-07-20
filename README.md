# ZTE-OLT

##ZTE 8 & 16 PORT OLT LIVE CONF 
===========================================


configure terminal
hostname OLT_HKUKG024_MOHAN_NAGAR_2_ZTE8P
clock timezone UTC 5 30
exit
clock set 15:35:20 01-04-2023


configure terminal
system-user
authorization-template 1
bind aaa-authorization-template 2001
exit
authentication-template 1
bind aaa-authentication-template 2001
exit
user-name admin
password Vineet@321
bind authorization-template 1
bind authentication-template 1
exit
authorization-template 1
local-privilege-level 15
exit
exit
line telnet server enable

vlan 3500
name MGMT
exit
vlan 3501
name HSI
exit
vlan 99
name ROM
exit

interface vlan3500
ip address 172.16.76.15 255.255.255.0
exit

ip route 0.0.0.0 0.0.0.0 172.16.76.1

spantree
enable
mode rstp
exit


interface xgei-1/1/1
no shutdown
switchport mode trunk
switchport vlan 3500,3501,99 tag
exit
interface xgei-1/1/2
shutdown
switchport mode trunk
switchport vlan 3500,3501,99 tag
exit
interface xgei-1/1/3
 shutdown
switchport mode trunk
switchport vlan 3500,3501,99 tag
exit
interface xgei-1/1/4
 shutdown
switchport mode trunk
switchport vlan 3500,3501,99 tag
exit
interface xgei-1/1/5
shutdown
switchport mode trunk
switchport vlan 3500,3501,99 tag
exit
interface xgei-1/1/6
shutdown
switchport mode trunk
switchport vlan 3500,3501,99 tag
exit


##AFTER LIVE
pon
onu-type ZTEG-F660 gpon description 4FE,2POTS,4WIFI
onu-type ZTE-F660 gpon description 4FE,2POTS,4WIFI
onu-type ZTE-F670L gpon description 4FE,1POTS,4WIFI
onu-type ZTEG-F670L gpon description 4FE,1POTS,4WIFI
onu-type-if ZTEG-F660 eth_0/1
onu-type-if ZTEG-F660 eth_0/2
onu-type-if ZTEG-F660 eth_0/3
onu-type-if ZTEG-F660 eth_0/4
onu-type-if ZTEG-F660 pots_0/1
onu-type-if ZTEG-F660 pots_0/2
onu-type-if ZTEG-F660 wifi_0/1
onu-type-if ZTEG-F660 wifi_0/2
onu-type-if ZTEG-F660 wifi_0/3
onu-type-if ZTEG-F660 wifi_0/4
onu-type-if ZTEG-F670L eth_0/1
onu-type-if ZTEG-F670L eth_0/2
onu-type-if ZTEG-F670L eth_0/3
onu-type-if ZTEG-F670L eth_0/4
onu-type-if ZTEG-F670L pots_0/1
onu-type-if ZTEG-F670L wifi_0/1
onu-type-if ZTEG-F670L wifi_0/2
onu-type-if ZTEG-F670L wifi_0/3
onu-type-if ZTEG-F670L wifi_0/4
onu-type-if ZTE-F660 eth_0/1
onu-type-if ZTE-F660 eth_0/2
onu-type-if ZTE-F660 eth_0/3
onu-type-if ZTE-F660 eth_0/4
onu-type-if ZTE-F660 pots_0/1
onu-type-if ZTE-F660 pots_0/2
onu-type-if ZTE-F660 wifi_0/1
onu-type-if ZTE-F660 wifi_0/2
onu-type-if ZTE-F660 wifi_0/3
onu-type-if ZTE-F660 wifi_0/4
onu-type-if ZTE-F670L eth_0/1
onu-type-if ZTE-F670L eth_0/2
onu-type-if ZTE-F670L eth_0/3
onu-type-if ZTE-F670L eth_0/4
onu-type-if ZTE-F670L pots_0/1
onu-type-if ZTE-F670L wifi_0/1
onu-type-if ZTE-F670L wifi_0/2
onu-type-if ZTE-F670L wifi_0/3
onu-type-if ZTE-F670L wifi_0/4
exit



gpon
profile tcont 1g type 4 maximum 1024000
profile tcont 5M type 1 fixed 5210
onu profile sip voice proxy 192.168.253.228 registrar 192.168.253.228
onu profile vlan HSI3501 tag-mode tag cvlan 3501
profile tcont 1g type 4 maximum 1124000
profile tcont 5M type 1 fixed 5211
exit

interface gpon_olt-1/3/1
no shutdown
exit
interface gpon_olt-1/3/2
no shutdown
exit
interface gpon_olt-1/3/3
no shutdown
exit
interface gpon_olt-1/3/4
no shutdown
exit
interface gpon_olt-1/3/5
no shutdown
exit
interface gpon_olt-1/3/6
no shutdown
exit
interface gpon_olt-1/3/7
no shutdown
exit
interface gpon_olt-1/3/8
no shutdown
exit





pon
onu-profile gpon line TR69
vport-mode manual
tcont 1 name HSI_VOICE profile 1g
tcont 2 name ROM profile 1g
gemport 1 name HSI_VOICE tcont 1
gemport 2 name ROM tcont 2
service HSI_VOICE gemport 1 vlan 3501
service ROM gemport 2 vlan 99
vport 1 map-type vlan
vport-map 1 2 vlan 99
vport-map 1 1 vlan 3501
$
onu-profile gpon service TR69
voip protocol sip
type PROFILE-DEFAULT
voip-ip ipv4 mode dhcp vlan-profile HSI3501 host 1
exit
exit
exit
write



snmp version v2c enable
snmp-server community public view AllView rw
snmp-server community LDHn3T view AllView rw
logging trap-enable notification
snmp-server enable trap
snmp-server host 192.168.253.165 trap version 2c public


logging snmp
accept off
no match cmdlog
exit
exit
write



tacacs enable
system-user
default-privilege-level 15
user-default
bind authentication-template 1
bind authorization-template 1
exit
exit


tacacs-client source-interface vlan3501
tacacs-server timeout 1
tacacs-server deadtime 1
tacacs-server key pix321
tacacs-server host 192.168.253.196
tacplus group-server standard
server 192.168.253.196
exit
aaa-authentication-template 2001
aaa-authentication-type tacacs-local
authentication-tacacs-group standard
exit

aaa-authorization-template 2001
aaa-authorization-type tacacs-local
authorization-tacacs-group standard
exit


