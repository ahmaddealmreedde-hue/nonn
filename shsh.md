import subprocess
import hashlib
import time
def run(cmd):
    try:
        out = subprocess.check_output(cmd, shell=True, stderr=subprocess.DEVNULL)
        return out.decode().strip()
    except Exception:
        return ""

android_id = run("settings get secure android_id")
serial = run("getprop ro.serialno") or run("getprop ro.boot.serialno") or run("getprop ro.hardware")
brand = run("getprop ro.product.brand")
model = run("getprop ro.product.model")
mac = run("cat /sys/class/net/wlan0/address 2>/dev/null")
values = [v for v in [android_id, serial, brand, model, mac] if v]
concat = "|".join(values)

SALT = "سِرّ_خاص_قوي_غير_مُشارَك"

full = SALT + "|" + concat
h = hashlib.sha256(full.encode()).hexdigest()
hash=['f76b89ec51ac0b1a24cf9b12228e80a0c55e4f3f40cf38cf8383da455e4e69d2','6a6cd13d856450a7d1e67fbfda2ddf43bebd238574c2466bcef46b3c8b09704a','5492156e8956344a4cfd3ec430368d2d85a4c8fca36ce8a4ca7e2f30f6eb25b0','94d2721e7c93911e76fb8ab944fd687cc4d3faab6977d642726ac07a25a67f9e','2239da2340b1b0943232f0b0cc1a536a2bb2876329e50fb51c283db705cea175','4dbdbfab22dcd9eafc9e764536dd0bf07d8e2e369b5d6622b849d4ca16ca789b','89c0808f93edfe486fa090b8dc0495cc0f78971aacda586975a5d6af71965d67','7df5c0932fe9564504455d1ed7e52da785e85ba1436cde2b7cdff384da4a5faa','fb5f29a42f393fb5e8025cf4c387bc736d1efa0dc132ea1a317d367d3d1faf91','63a2cb914ace6fd17e9c308696ad2e1bcb29b8ca45510ea1dbd88026dea956fa']
if h in hash:
    print('good / هاتفك مصرح للدخول ')
    pass
else :
	while True:
	   print('your code is /', h)
	   print()
	   print('للتفعيل راسل المطور')
	   print()
	   time.sleep(90)
     
