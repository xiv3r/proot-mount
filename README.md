# proot-mount
mount any rootfs using proot in termux

# Depend
```
apt update && apt install proot -y
```

# Mount rootfs using proot
> - save file in `/bin/proot-mount`
> - add permission `chmod 700 /bin/proot-mount`
> - download any linux rootfs.tar.xz file and extract tar -xJf
> - rename `rootfs-arm64` from the script to the rootfs file directory create by tar after decompressing the tarball.
```bash
#!/data/data/com.termux/files/usr/bin/bash -e

cd ~/
KERNEL=$(uname -r)
unset LD_PRELOAD

cmdline="proot \
    -L \
    -0 \
    --link2symlink \
    -k $KERNEL \
    -r rootfs-arm64 \
    -b /dev \
    -b /proc \
    -b /sys \
    -b /sdcard \
    -b /etc/hosts \
    -b rootfs-arm64/root:/dev/shm \
    -w /root \
    /usr/bin/env -i \
    HOME=/root \
    TMPDIR=/tmp \
    LANG=C.UTF-8 \
    PATH=/usr/local/sbin:/usr/local/bin:/bin:/usr/bin:/sbin:/usr/sbin \
    TERM=$TERM \
    /bin/bash --login"

if [ "$#" -eq 0 ]; then
    exec $cmdline
else
    $cmdline -c "$@"
fi
```
