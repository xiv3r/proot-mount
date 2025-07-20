# proot-mount
mount any rootfs using termux proot

# Depend
```
pkg update && pkg install proot proot-distro -y
```

# Mount rootfs using proot
> - save file in `/bin/proot-mount`
> - add permission `chmod 700 /bin/proot-mount`
> - download any linux rootfs.tar.xz file and extract using `tar -xJf file.tar.xz`
> - rename `rootfs-arm64` from the script to the rootfs file directory create by tar after decompressing the tarball.
> - `DIRECTORY` contains root file system

```bash
#!/data/data/com.termux/files/usr/bin/bash -e

cd $HOME
unset LD_PRELOAD
cmdline="proot
    --link2symlink
    -L 
    -0 
    --ashmem-memfd 
    -r DIRECTORY
    -b /dev
    -b /proc
    -b /sys
    -b /sdcard
    -b DIRECTORY/root:/dev/shm
    -w /root
       /usr/bin/env -i 
       HOME=/root 
       PATH=/usr/local/sbin:/usr/local/bin:/bin:/usr/bin:/sbin:/usr/sbin
       TERM=$TERM
       LANG=C.UTF-8
       /bin/bash --login"

if [ "$#" -eq 0 ]; then
    exec $cmdline
else
    $cmdline -c "$@"
fi
```
