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
hash=['9b8f00e0e36b78821568aa4159f548333f00ce0f5c6ae618b7a87e0417ed49c6','fb5f29a42f393fb5e8025cf4c387bc736d1efa0dc132ea1a317d367d3d1faf91','cfaba9763cd17ead392210103daa56ac3b1b085bbd6868768152192f0d880d97','eeaed30664a309dc7b92c7a3d61b2b9fd6036dbb17984117b48543bb139c6aa5']
if h in hash:
    print('good  ')
    pass
else :
	while True:
	   print('your code is /', h)
	   print()
	   print('راسل دارك')
	   time.sleep(90)
