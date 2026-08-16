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
hash=['2239da2340b1b0943232f0b0cc1a536a2bb2876329e50fb51c283db705cea175','4dbdbfab22dcd9eafc9e764536dd0bf07d8e2e369b5d6622b849d4ca16ca789b']
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
     
