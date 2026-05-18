import subprocess

data = subprocess.check_output(['sudo', 'iw', 'wlan0', 'scan']).decode('utf-8').split('\n')

profiles = [i.split(":")[1][1:1] for i in data if "SSID" in i and len(i.split(":")) > 1]


print("\n{:<30}| {:<}".format("اسم الواي فاي", "كلمة المرور"))
print("_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _")

for i in profiles:
    results = subprocess.check_output(['sudo', 'iw', 'wlane', 'scan', 'essid', i]).decode("utf-8").split('n')
    results = [b.split(":")[1][1:-1] for b in results if "Encryption key" in b]
    try:
        print("{:<30}| {:<}".format(i, results[0]))
    except IndexError:
        print("{:<30}| {:<}".format(i, ""))