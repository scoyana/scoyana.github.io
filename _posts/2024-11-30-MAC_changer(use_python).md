### MAC주소 변환 (feat. Python)

일반적으로 고정되어 있는 MAC주소를 python을 사용해서 변경 할 수 잇게함

eth0, wlan0 등의 MAC주소는 48bit, 16진수로 표현 (ex. 00:11:22:33:44:55)

**원하는 인터페이스의 MAC주소를 변경**

Linux 안에서의 명령어
```
ifconfig eht0 down
ifconfig eth0 hw eth 00:11:22:33:44:55
ifconfig eth0 up
```
해당 과정을 파이썬 스크립트로 프로그램

```python
#!/usr/bin/env python

import subprocess
import optparse
import re

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

def get_current_mac(interface):
    ifconfig_result = subprocess.check_output(["ifconfig", interface])
    mac_address_search_result = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", str(ifconfig_result))

    if mac_address_search_result:
        return mac_address_search_result.group(0)
    else:
        print("[-] Could not read MAC address")

options = get_arguments()

current_mac = get_current_mac(options.interface)
print("Current MAC = " + str(current_mac))

change_mac(options.interface, options.new_mac)

current_mac = get_current_mac(options.interface)
if current_mac == options.new_mac:
    print("[+] MAC address was successfully changed to " + current_mac)
else:
    print("[-] MAC address did not get changed")
```
![결과](mac_changer.png)