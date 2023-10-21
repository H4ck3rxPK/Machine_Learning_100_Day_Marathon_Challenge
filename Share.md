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


### 425. Authentication Authorization Accounting(AAA)
#### :: Privilege Levels
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

#### :writing_hand: Configure Password Strength and Management for <span style=color:red>Common Criteria</span>

```go
      --------------------------------------------------------------------------- 
    | username admin privilege 15 password 0 Cisco13579!                          |
    | !                                                                           |
    | aaa authentication login default local                                      |
    | aaa suthentication enable default none                                      |
    | !                                                                           |
    | aaa common-criteria policy Administrators                                   |
    |  min-length 1                                                               |
    |  max-length 127                                                             |
    |  char-changes 4                                                             |
    |  lifetimemonth 2                                                            |
      --------------------------------------------------------------------------- 

- That "Administrators" is policy name

- The numer "0" behind password is mean clean-text
(So, numer "7" behind password is mean encry of Cisco(Vigenère cipher). "5" is MD5)

- The "lifetimmonth" is mean the circle of password expire
```

##### TACACS+ https://github.com/christian-becker/tac_plus

##### Common-Criteria-Policy https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/sec_usr_aaa/configuration/xe-16-10/sec-usr-aaa-xe-16-10-book/sec-aaa-comm-criteria-pwd.html
