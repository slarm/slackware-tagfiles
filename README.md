# slackware-tagfiles
Tag files for a custom Slackware install.

The use case of this setup is to run a customized installation of Slackware on a low-end box, serving my own purposes. These tagfiles include base requirements, basic network utilities and some useful tools such as screen.

Modify the tagfiles as needed. My setup supports ext4 on LUKS + LVM.


## Build a custom ISO

rsync the desired Slackware release:
```bash
rsync -avzh rsync://ftp.osuosl.org/slackware/slackware64-15.0/ /opt/slackware64/
```

Replace the tagfiles with the ones in this repo.

Optionally, delete all not-to-be installed packages. This can be done with something like:
```bash
cd /opt/slackware64/slackware64 && find ./ -type d -mindepth 1 -maxdepth 1 | while read dir; do
    cat $dir/tagfile | while read pkg; do
        if [[ $pkg =~ (.+)?:SKP ]]; then
            rm -v $dir/"${BASH_REMATCH[1]}"-*
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
mkisofs -o /opt/slackware-15.iso -R -J -V "Slackware Install"  -x ./extra -x ./patches -x ./source -x ./testing -x ./usb-and-pxe-installers -b syslinux/syslinux.bin -c syslinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -hide-rr-moved -hide-joliet-trans-tbl -sort syslinux/iso.sort -v -d -N -A "Slackware 15 custom.slarm" .
```

## Full package list

```
a_base
a: aaa_glibc-solibs
a: aaa_libraries
a: aaa_terminfo	
a: acl
a: attr
a: bash
a: bin
a: bzip2
a: coreutils
a: cpio
a: cracklib
a: dbus
a: dcron
a: devs
a: dialog
a: e2fsprogs
a: elogind
a: etc
a: eudev
a: exfatprogs
a: file	  	
a: findutils
a: gawk
a: glibc-zoneinfo
a: gptfdisk
a: grep
a: grub
a: gzip
a: hostname
a: infozip
a: kbd
a: kernel-firmware
a: kernel-generic
a: kernel-huge
a: kernel-modules
a: kmod
a: less
a: libgudev
a: libpwquality
a: logrotate
a: mkinitrd
a: mlocate
a: nvi
a: openssl-solibs
a: pam
a: pkgtools
a: procps-ng
a: rpm2tgz
a: sed
a: shadow
a: sharutils
a: sysklogd
a: syslinux
a: sysvinit
a: sysvinit-functions
a: sysvinit-scripts
a: tar
a: time
a: tree
a: utempter
a: util-linux
a: which
a: xz
ap: groff
ap: htop
ap: lsof
ap: man-db
ap: man-pages
ap: neofetch
ap: screen
ap: slackpkg
d: binutils
d: git
d: perl
l: brotli
l: libnl3
l: libpcap
l: libseccomp
l: libunistring
l: ncurses
n: ca-certificates
n: curl
n: cyrus-sasl
n: dhcpcd
n: gnupg
n: gnupg2
n: gnutls
n: iproute2
n: iptables
n: iputils
n: libmnl
n: lynx
n: net-tools
n: network-scripts
n: nghttp2
n: openssh
n: openssl
n: wget
```