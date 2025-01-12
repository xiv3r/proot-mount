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
unset LD_PRELOAD

cmdline="proot \
    --link2symlink \
    -L \
    -0 \
    --ashmem-memfd \
    -r (replace) \ # replace the guest directory
    -b /dev \
    -b /proc \
    -b /sys \
    -b /sdcard \
    -b /etc/resolv.conf \
    -b (replace)/root:/dev/shm \ # replace the guest root directory
    -w /root \
       /usr/bin/env -i \
       HOME=/root \
       PATH=/usr/local/sbin:/usr/local/bin:/bin:/usr/bin:/sbin:/usr/sbin \
       TMP=/tmp \
       TERM=$TERM \
       LANG=C.UTF-8 \
       /bin/bash --login"

if [ "$#" -eq 0 ]; then
    exec $cmdline
else
    $cmdline -c "$@"
fi
```
