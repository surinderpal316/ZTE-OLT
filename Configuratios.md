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


#Modem Configuration
##ZTE CONFIGUATION 8,32,16 PORT
-------------------------------------

##ZTE 8 PORT Modem Configuration
----------
interface gpon_olt-1/1/1
onu 4 type ZTE-F670L sn ALCLEB132795
exit
interface gpon_onu-1/1/1:4
sn-bind enable sn
tcont 1 name HSI profile 1g
gemport 1 name HSI tcont 1
exit
pon-onu-mng gpon_onu-1/1/1:4
broadcast-limit ethuni eth_0/2 rate-limit 20
loop-detect ethuni eth_0/2 enable
service HSI gemport 1 vlan 721
vlan port eth_0/2 mode trunk
wan-ip ipv4 mode dhcp vlan-profile HSI721 host 1
exit
interface vport-1/1/1.4:1
service-port 1 user-vlan 721 vlan 721
exit
exit

##ZTE 32 PORT Modem Configuration
-----------
interface gpon_olt-1/3/2
onu 56 type ZTE-F670L sn ZTEGC8EEBEDF
exit
interface gpon_onu-1/3/2:56
sn-bind enable sn
tcont 1 name HSI profile 1g
gemport 1 name HSI tcont 1
exit
pon-onu-mng gpon_onu-1/3/2:56
broadcast-limit ethuni eth_0/2 rate-limit 20
loop-detect ethuni eth_0/2 enable
service HSI gemport 1 vlan 1221
vlan port eth_0/2 mode trunk
wan-ip ipv4 mode dhcp vlan-profile HSI1221 host 1
exit
interface vport-1/3/2.56:1
service-port 1 user-vlan 1221 vlan 1221
exit
exit
write

##ZTE 16 PORT Modem Configuration 
-----------
interface gpon-olt_1/2/14
no onu 5
onu 5 type ZTE-F660 sn ZTEGC8E1EB2C
!
interface gpon-onu_1/2/14:5
tcont 1 name HSI_VOICE profile 1g
tcont 2 name ROM profile 1g
gemport 1 name HSI_VOICE tcont 1
gemport 2 name ROM  tcont 2
service-port 1 vport 1 user-vlan 1221 vlan 1221 
service-port 2 vport 2 user-vlan 99 vlan 99 
	!

pon-onu-mng gpon-onu_1/2/14:5
service HSI_VOICE gemport 1 vlan 1221
service ROM gemport 2 vlan 99
voip protocol sip
voip-ip mode dhcp vlan-profile HSI1221 host 1
loop-detect ethuni eth_0/1 enable
loop-detect ethuni eth_0/2 enable
loop-detect ethuni eth_0/3 enable
loop-detect ethuni eth_0/4 enable
!
exit
write


## Trans Modem
##NOKIA TRANS ON ZTE OLT
------------------------

## 8-port

interface gpon_olt-1/1/9
no on 20
onu 20 type ZTE-F660 sn ALCLB3AE4316
!

interface gpon_onu-1/1/9:20
  tcont 1 name HSI profile 1g
  gemport 1 name HSI tcont 1
!
pon-onu-mng gpon_onu-1/1/9:20
  service HSI gemport 1 vlan 721
!
interface vport-1/1/9.20:1
  service-port 1 user-vlan 721 vlan 1721
!

-------------------------------------------
## 16-port

interface gpon-olt_1/1/11
no onu 1
onu 1 type ZTE-F660 sn ALCLFA648E98	 
!
interface gpon-onu_1/1/11:1
  name ####
  sn-bind enable sn
  tcont 1 name hsi profile 1g
  gemport 1 tcont 1
  service-port 1 vport 1 user-vlan 881 vlan 3501
!

pon-onu-mng gpon-onu_1/1/11:1
service hsi gemport 1 vlan 881
loop-detect ethuni eth_0/1 enable
loop-detect ethuni eth_0/2 enable
loop-detect ethuni eth_0/3 enable
loop-detect ethuni eth_0/1 enable
!

