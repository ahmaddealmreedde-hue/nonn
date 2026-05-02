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
hash=['f2fa637d0b3e518870979685ec43903ee9735ea40a2a1d9f17537735e27c9758','2239da2340b1b0943232f0b0cc1a536a2bb2876329e50fb51c283db705cea175','294b6221ca2add3f64f9d583b20bce571fc61daf1acb85687b4d73342d1b6c53','8e177d4d82fa50f878397ee915f02e851491f7295460f83eee9450e3b0091ff5']
if h in hash:
    print('good  ')
    pass
else :
	while True:
	   print('your code is /', h)
	   print()
	   print('راسل دارك')
	   time.sleep(90)
