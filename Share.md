### 1. SD-Access 架構
![](https://hackmd.io/_uploads/S1kDUvfGa.png)
![](https://hackmd.io/_uploads/ByJVYOfMp.png)

- c is Fabric Border Node
- d is Intermediate Node

**fabric edge node作用**
圖說為連接終端之節點，架構霧煞煞

LISP(Location ID Separation Protocol)

### 2.  VTY user privilege level Default=1
``` shell
R1# sh run | begin line con
line con 0
    exec timeout 0 0
    privilege level 15
    logging synchronous
    stopbits 1
line aux o
    exec timeout 0 0
    privilege level 15
    logging synchronous
    stopbits 1
line vty 0 4
    password 7 045802150C2E
    login
line vty 5 15
    password 7 045802150C2E
    login
end

R1# sh run | include aaa | enable
no aaa new-model
```
line: 進入行模式的命令(console、aux、vty、tty)
![](https://hackmd.io/_uploads/BJppjPzzT.png)

vty: 虛擬終端埠(Virtual terminal for remote console access) for telnet, ssh.

<font color="red">**不填寫權限預設為1**</font>

權限
![](https://hackmd.io/_uploads/S1dPKwff6.png)

ref: https://www.youtube.com/watch?v=hO-mzTUcyFA
https://www.jannet.hk/authentication-authorization-accounting-aaa-zh-hant/


### 3. RIB vs. FIB
RIB - Routing information base(***sh ip route***)
- Static Route
- Dynamic Route

![](https://hackmd.io/_uploads/B1QDQdMf6.png)

FIB - Forward Information base(***sh ip cef***)

*基於RIB路由後的路徑更新於FIB，簡化Route Lookup程序*
*透過來源IP的prefix判斷並轉發*
![](https://hackmd.io/_uploads/rJQ9mOzMp.png)

ref: https://www.jannet.hk/routing-decision-zh-hant/

### 4. Ansible
* 自動化組態管理工具
* 基於SSH
* 管理節點不限作業系統(Windows、Linux)
* ansible ***ad hoc*** / playbook = linux shell / shell script
* Ansible Tower => 中控

### 5. Hidden SSID

手動輸入隱藏SSID才連得到Wifi

### 6. 無線AP的FlexConnect模式



#### 先備知識

* WLC
    * AP的中控
    * Cisco **W**ireless **L**AN **C**ontroller
* CAPWAP
    * AP與中控使用的協議
    * 無線接入點控制和配置協議
    * Control and Provisioning of Wireless Access Points
    * 2009年出現，前身：Lightweight Access Point Protocol (LWAPP)
    * 用於無線終端接入點（AP）和無線網絡控制器（AC）之間的通信交互，實現AC進行所關聯的AP集中管理和控制。

##### 運作模式圖解
![](https://hackmd.io/_uploads/Skudf2oGT.png)

#### AP運作模式

* Local
    * AP預設模式 
    * tunnel所有流量回controller
    * 必須連到controller
    * 不支援MESH

* FlexConnect
    * 不支援MESH
    * Switching Modes
        * tunnel所有流量回controller
        * 直接轉發
        * ![](https://hackmd.io/_uploads/B11nC2oGp.png)
    * Operation Modes
        * Connected 
        * Standalone
 
* Bridge 
    * 支援MESH
    * 2台AP互通
* Flex+Bridge
    * 支援MESH
    * 2台AP互通 且 可tunnel所有流量回controller
    * ![](https://hackmd.io/_uploads/SJ2qyRoGa.png)


#### 選項詳解
* 選項A
    * Connected模式下Local及FlexConnect 都可以偵測非法惡意AP
    * ![](https://hackmd.io/_uploads/HyvduTozT.png)
* 選項B
    * 應該是指Flex+Bridge模式
* 選項C
    * 原文: FlexConnect mode is a feature that is designed to allow specified CAPWAP-enabled APs to exclude themselves from managing data traffic between clients and infrastructure
    * 翻譯: FlexConnect 模式可以**不**透過已開啟CAPWAP的AP，管理用戶端及基礎網路設備之間資料流量

*ref: https://www.cisco.com/c/en/us/td/docs/wireless/controller/8-5/config-guide/b_cg85/flexconnect.html*


### 7. OSPF Network type

https://www.jannet.hk/open-shortest-path-first-ospf-zh-hant/


![](https://hackmd.io/_uploads/B1ICORsfa.png)



三種類型
* Broadcast
    * 透過廣播(Broadcast)自動發現鄰居(Neighbor)節點
![](https://hackmd.io/_uploads/ByHkz1hza.png)

* Point to Point
    * 兩點互聯不需要廣播
![](https://hackmd.io/_uploads/S1KlzyhGp.png)

* Non-Broadcast
    * 手動輸入Neighbor
![](https://hackmd.io/_uploads/rkMGGkhza.png)


連接模式
* Broadcast to Broadcast
* Non-broadcast to Non-broadcast
* Point-to-Point to Point-to-Point
* Broadcast to Non-broadcast Networks (adjust hello and dead timers)
* Point-to-Point to Point-to-Multipoint Networks (adjust hello and dead timers)



![](https://hackmd.io/_uploads/HJ9JbJ3Ga.png)

https://afrozahmad.com/blog/ospf-network-types-explained/


A. point-to-multipoint to nonbroadcast 
B. broadcast to nonbroadcast 
C. point-to-multipoint to broadcast 
D. broadcast to point-to-point

B

### 8. FTD 接線模式

![](https://hackmd.io/_uploads/BJjTzkhGa.png)

https://www.cisco.com/c/zh_tw/support/docs/security/firepower-ngfw/200924-configuring-firepower-threat-defense-int.html

### 9. VRF Lite/RD, RT觀念

VRF= L3 的 vlan (切割路由表)
VRF=(VRF Lite + MPLS)

https://www.cisco.com/c/en/us/td/docs/optical/15000r8_0/ethernet/454/guide/d80ether/r8vrf.pdf

https://www.jannet.hk/multi-protocol-label-switching-mpls-zh-hant/#Route_Target

### 10.Cisco TrustSec

Cisco Identity Services Engine (ISE)功能
身分識別+流量帶安全TAG(SGT)

### 424. Python data struction to Json
##### py_data
```python!
# device.py
device = [
    {
        "name": "distsw1",
        "ip": "192.168.155.1",
        "type": "Catalyst C9407R",
        "user": "netadmin",
        "pass": "66674431c3577d398613263c0bfb6fe5"
    }
]
```
##### json_dump
```python!
import json
from device import device  # 從 device.py 導入 device

json_string = json.dumps(device)
print(json_string)
```
### 425. Cisco Digital Network Architecture(DNA)
![](https://hackmd.io/_uploads/SkRUKUbzT.png)
```rust!
GPT 4.0 EXPLAIN

Design: this phase is the planning and design process of the network. 

Management: this phase involves the daily management and maintenance of network devices. 

Deployment: once the network has been designed and planned, it needs to be deployed in a physical or virtual environment.

Provisioning: this phase is the process of configuring and enabling network resources to meet specific business or application requirements.

Assurance: this phase involves monitoring the performance, health, and security of the network and ensuring it operates at the expected level.
```
##### *Ref - SD-Access Deployment https://www.sdnlab.com/23526.html*

### 426. Authentication Authorization Accounting(AAA)
#### Privilege Levels
```go
| Level    | Description                                                                 |
|:-------- | --------------------------------------------------------------------------- |
| 0        | Only enable, disable, exit, help, logout.                                   |
| -------- | --------------------------------------------------------------------------- |
| 1        | User Mode. i.e., show version, show inventory etc., and not allow to config.|
| -------- | --------------------------------------------------------------------------- |
| 2-14     | Commands available from manager to set.                                     |
| -----    | --------------------------------------------------------------------------- |
| 15       | Privilege Mode/Enable Mode.                                                 |
| -------  | --------------------------------------------------------------------------- |
```
##### *Ref - Cisco ISO privilege level https://learningnetwork.cisco.com/s/blogs/a0D3i000002eeWTEAY/cisco-ios-privilege-levels*

#### :writing_hand: Configure Password Strength and Management for <span style=color:red>Common Criteria</span>

```go!
  ---------------------------------------------------------------------------- 
| username admin privilege 15 password 0 Cisco13579!                            |
| !                                                                             |
| aaa authentication login default local                                        |
| aaa authentication enable default none                                        |
| !                                                                             |
| aaa common-criteria policy Administrators                                     |
|  min-length 1                                                                 |
|  max-length 127                                                               |
|  char-changes 4                                                               |
|  lifetimemonth 2                                                              |
  ---------------------------------------------------------------------------- 

- That "Administrators" is policy name

- The numer "0" behind password is mean clear-text
(numer "7" is mean encry of Cisco(Vigenère cipher). "5" is MD5)

- The "lifetimmonth" is mean the circle of password expire

So, Need to add "username [admin] common-criteria-policy [Administrators] password [0][password]"
```

##### *Ref - TACACS+ https://github.com/christian-becker/tac_plus*

##### *Ref - Common-Criteria-Policy https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/sec_usr_aaa/configuration/xe-16-10/sec-usr-aaa-xe-16-10-book/sec-aaa-comm-criteria-pwd.html*

### 427. Maximum Transmission Unit(MTU) vs. Don't Fragment(DF)
#### Control Plane Policing(CoPP)

##### *Ref - CoPP https://www.cisco.com/c/zh_tw/support/docs/switches/nexus-7000-series-switches/116043-copp-nexus7000-tshoot-00.html*

##### *Ref - DF-BIT PING https://community.cisco.com/t5/switching/what-is-meaning-of-df-bit-in-ping-or-do-not-fragment/td-p/4750652*

### <span style=color:red>***428. Virtual vs. Hardware Switch Compare**</span>

### 429. Virtual Router Redundancy Protocol(VRRP)

#### <span style=color:red>**Experimental**</span>
![](https://hackmd.io/_uploads/rkSZbZSf6.png)

```go!
#R1
  --------------------------------------------------------------------------- 
| SW1(config)#interface fa0/17                                                 |   
| SW1(config-if)#vrrp 1 ip 192.168.1.3                                         |
| SW1(config-if)#vrrp 1 priority 150                                           |
| SW1(config-if)#vrrp 1 authentication md5 key-string mykey                    |
  --------------------------------------------------------------------------- 

#R2
  --------------------------------------------------------------------------- 
| SW1(config)#interface fa0/19                                                 |   
| SW1(config-if)#vrrp 1 ip 192.168.1.3                                         |
| SW1(config-if)#vrrp 1 authentication md5 key-string mykey                    |
  --------------------------------------------------------------------------- 
```

#### :writing_hand: Config of Track
```go!
#R1
  ------------------------------------------------------------------------ 
| R1#show running-config interface fa0/0                                     |
| Building configuration...                                                  |
|                                                                            | 
| Current configuration : 192 bytes                                          | 
| !                                                                          | 
| interface FastEthernet0/0                                                  |
|  ip address 192.168.3.5 255.255.255.0                                      |
|  duplex full                                                               |
|  vrrp 1 ip 192.168.3.1                                                     |
|  vrrp 1 priority 110                                                       | 
|  vrrp 1 authentication text cisco                                          |
|  vrrp 1 track 20 decrement 20                                              |
| end                                                                        |
|                                                                            |
| R1#show running-config | include track 20                                  |
|  track 20 ip route 10.10.1.1 255.255.255.255 reachability                  |
  ------------------------------------------------------------------------ 

#R2
  ------------------------------------------------------------------------ 
| R2#show running-config interface fa0/0                                     |
| Building configuration...                                                  |
|                                                                            | 
| Current configuration : 141 bytes                                          | 
| !                                                                          | 
| interface FastEthernet0/0                                                  |
|  ip address 192.168.3.2 255.255.255.0                                      |
|  duplex full                                                               |
|  vrrp 1 ip 192.168.3.1                                                     |
|  vrrp 1 authentication text cisco                                          |
| end                                                                       
  ------------------------------------------------------------------------ 

- The authentication text is mean clear-text not MD5.

- The "Track" is configuration is used to adjust the VRRP priority if reachability destination successful in route table.
```

### 430. <span style=color:darkblue>802.1x (Port-based)</span> of Network Access Control (NAC) 

<img src="https://hackmd.io/_uploads/Hy7FoArX6.png" width=650>

#### Strong Authentication and Authorization (L2)
```python!
The IEEE 802.1X protocol operates at the data link layer of the network and is executed before a user gains access to the network, which can be either Ethernet/802.3 or WLAN. Authentication is implemented through the EAP protocol, and authorization is controlled by the RADIUS protocol.
```
##### *Ref - Wiki https://zh.m.wikipedia.org/zh-tw/IEEE_802.1X*
### 431. NetFlow
![image.png](https://hackmd.io/_uploads/HJzwh-UQ6.png)
```go!
  ------------------------------------------------------------------------ 
| flow record Recorder                                                      |     
|  match ipv4 protocol                                                      |
|  match ipv4 source address                                                |
|  match ipv4 destination address                                           |
|  match transport source-port                                              |
|  match transport destination-port                                         |
| !                                                                         |
| flow exporter Exporter                                                    |
|  destination 192.168.100.22                                               |
|  transport udp 2055                                                       |
| !                                                                         |
| flow monitor Monitor                                                      |
|  exporter Exporter                                                        |
|  record Recorder                                                          |
| !                                                                         |
| at-analytics                                                              |
|  ip flow-export destination 192.168.100.22 2055                           |
| !                                                                         |
| interface gi1                                                             |
|  ip flow monitor Monitor input                                            |
|  ip flow monitor Monitor output                                           |
|  at-analytics enable                                                      |
| !                                                                         |
  ------------------------------------------------------------------------ 

- flow interface defines interface-level commands.

- flow record defines matching criteria (like protocol type, IP addresses, port numbers, etc.)
*** "Traffic must match a match rule definition before it can be collected and sent. You cannot collect and send data that is not matched." ***

- flow monitor defines framework used to capture and analyze traffic data passing through the device. It associates one or more flow records with a flow exporter to specify which data should be captured and how this data should be processed.

- flow exporter defines NetFlow data will be sent to and how it will be sent. 

So, I think need to add SNMP interface table "under the flow exporter".

Answer B:
collect interface input-snmp
collect interface output-snmp

Answer D:
Router(config)# flow exporter FLOW-EXPORTER-1
Router(config-flow-exporter)# option interface-table
```
##### *Ref - NetFlow https://www.jannet.hk/netflow-zh-hant/*
##### *Ref - Aruba intro NetFlow https://www.arubanetworks.com/techdocs/AOS-CX/10.11/HTML/cli_8360/Content/Chp_flow/flow_cmds/flo-exp.htm*

### 432. Wireless Access Point (AP) 

[![image.png](https://hackmd.io/_uploads/BybOBs4mT.png)
](https://www.tp-link.com/tw/blog/224/%E7%84%A1%E7%B7%9Aap%E6%98%AF%E4%BB%80%E9%BA%BC-3%E6%8B%9B%E6%95%99%E4%BD%A0%E6%8C%91%E5%B0%8D%E7%84%A1%E7%B7%9Aap-%E6%89%93%E9%80%A0%E9%AB%98%E5%AF%86%E5%BA%A6%E7%B6%B2%E8%B7%AF%E7%92%B0%E5%A2%83/)

### *433. 434. DNS query

### 435. DNA Center API
![image.png](https://hackmd.io/_uploads/B1UsUiVmT.png)

#### :writing_hand: **So, what is UUID?** 
#### UUID can uniquely identify network hardware devices, such as being used to identify network devices in the <span style=color:darkcyan>**Cisco DNA Center API**</span>.
![image.png](https://hackmd.io/_uploads/SJ2XTsNXa.png)



##### *Ref - What is UUID? https://www.mparticle.com/blog/what-is-a-uuid/*
##### *Ref - UUID vs. Crypto.randomUUID and NanoID https://medium.com/@matynelawani/uuid-vs-crypto-randomuuid-vs-nanoid-313e18144d8c*

### *436. HSRP

### 437. HTTP POST request
```go!
  --------------------------------------------------------------------------- 
| def get_token():                                                             |   
|    device_url = "https://192.168.1.1/dna/system/api/v1/auth/token"           |
|    http_result = requests.post(device_url, auth=("test", "test399079338!"))  |
|    if http_result.status_code != requests.codes.ok:                          |
|        print("Call failed! Review get_token().")                             |
|        sys.exit()                                                            |
|    return (http_result.json()['Token'])                                      |
  --------------------------------------------------------------------------- 

request.codes.ok is mean HTTP Status Code 200!
```



    
        
        
    
