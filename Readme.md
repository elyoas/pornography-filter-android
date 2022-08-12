## Pornography filter for Android devices:

### How to setup:

#### 1. Install LineageOS & Gapps:
https://wiki.lineageos.org/devices/

https://wiki.lineageos.org/gapps/

#### 2. Install hosts:
```
./hosts/Makeforphone.sh
adb push hosts /storage/self/primary/Backups/
adb devices
adb root
adb remount
adb shell
> rm /etc/hosts
> cat storage/self/primary/Backups/hosts >> /etc/hosts
> rm storage/self/primary/Backups/hosts
```
#### 3. Install custom iptables rules:
Thanks to **Evgeny Zinoviev**.

https://ch1p.io/lineageos-run-shell-script-at-boot-as-root/

https://ch1p.io/lineageos-iptables-block-app-internet/

```
# Step 1: Create the file:
vim /system/etc/init/myboot.rc

# The file should contain the following two lines:
on boot
    exec u:r:su:s0 root root -- /system/etc/myboot.sh

# Change its mode to:
chmod 0755 /system/etc/init/myboot.rc
ls -a1l /system/etc/init/myboot.rc
-rwxr-xr-x 1 root root 63 2021-11-21 16:43 /system/etc/init/myboot.rc

# Step 2: Create the file:
vim /system/etc/myboot.sh
chmod 0755 /system/etc/myboot.sh
ls -a1l /system/etc/myboot.sh
-rwxr-xr-x 1 root root 38246 2021-11-21 19:03 /system/etc/myboot.sh

# Contains:
#!/system/bin/sh

/system/bin/iptables -I OUTPUT 1 -m string --string "porn" --algo kmp -j REJECT
/system/bin/iptables -I OUTPUT 1 -m string --string "xxx" --algo kmp -j REJECT
... put rest of commands here ...
```

