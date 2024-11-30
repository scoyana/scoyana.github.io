# MAC주소 변환 (feat. Python)

일반적으로 고정되어 있는 MAC주소를 변경하고, 이를 자동화 시키기위해 파이썬을 이용해보았다

eth0, wlan0 등의 MAC주소는 48bit, 16진수로 표기되어 있다(ex. 00:11:22:33:44:55)

원하는 인터페이스의 MAC주소를 변환하는 방법은 아래와 같다.

**예시1 (eth0 MAC주소 변경)**
```
ifconfig eht0 down
ifconfig eth0 hw eth 00:11:22:33:44:55
ifconfig eth0 up
```
해당 과정을 자동화하는 스크립트를 작성한다(feat. python)

```python
#!/usr/bin/env python

import subprocess
import optparse


def get_arguments():
    parser = optparse.OptionParser()
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change its MAC address")
    parser.add_option("-m", "--mac", dest="new_mac", help="New MAC address")
    (options, arguments) = parser.parse_args()
    if not options.interface:
        parser.error("[-] Please specify an interface, use --help for more info.")
    elif not options.new_mac:
        parser.error("[-] Please specify a new mac, use --help for more info.")
    return options

def change_mac(interface, new_mac):
    print("[+] changing MAC address for " + interface + " to " + new_mac)
    subprocess.call(["ifconfig", interface ,"down"])
    subprocess.call(["ifconfig", interface ,"hw", "ether", new_mac])
    subprocess.call(["ifconfig", interface ,"up"])


options = get_arguments()
change_mac(options.interface, options.new_mac)
```