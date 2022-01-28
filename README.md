# slackware-tagfiles
Tag files for a custom Slackware install.

The use case of this setup is to run a somewhat minimal installation of Slackware on a low-end box. These tagfiles include base requirements, basic network utilities and some useful tools such as screen.

You would probably need to modify the tagfiles if you want to run anything else than ext4 without LVM.

## Build a custom ISO

rsync the desired Slackware release:
```bash
rsync -avzh rsync://ftp.osuosl.org/slackware/slackware64-current/ /opt/slackware64/
```

Replace the tagfiles with the ones in this repo.

Optionally, delete all nlot-to-be installed packages. This can be done something like:
```bash
cd /opt/slackware64 && find ./ -type d -mindepth 1 -maxdepth 1 | while read dir; do
    cat $dir/tagfile | while read pkg; do
        if [[ $pkg =~ (.+)?:SKP ]]; then
            rm -v $dir/"${BASH_REMATCH[1]}"*
        fi
    done
done
```
You might want to rename isolinux to syslinux if you get an error while trying to boot from the iso:
```bash
 mv isolinux/isolinux.bin isolinux/syslinux.bin
 mv isolinux/isolinux.cfg isolinux/syslinux.cfg
 mv isolinux syslinux
 ```

Create the iso:
```bash
mkisofs -o /opt/slackware-15-current.iso -R -J -V "Slackware Install"  -x ./extra -x ./patches -x ./source -x ./testing -x ./usb-and-pxe-installers -b syslinux/syslinux.bin -c syslinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -hide-rr-moved -hide-joliet-trans-tbl -sort syslinux/iso.sort -v -d -N -A "Slackware 15 minimal.slarm" .
```
