# nixos-in-place
An all-in-one script for installing NixOS on top of any existing Linux
distribution.

## How to do it
0. **BACKUP**
1. See the
   [Platform-specifics](https://github.com/jeaye/nixos-in-place#platform-specifics)
2. Run the following (see `./install -h` for options)
```bash
$ ./install
```
3. **STOP AND VERIFY** Hit `y` to verify the installation
4. Grab some coffee while NixOS installs
5. Type in your new root password
6. Hit `y` to reboot into NixOS!

## What you get
A fresh install of NixOS 15.09, either minimal or graphical (your choice). Your
old system and all your old files still exist and are setup to mount on
`/old-root`. As far as the file system is concerned, NixOS is installed in
`/old-root/nixos` and `/` is rebound before spinning up the system; everything
else in `/old-root` is fair game to delete.

NixOS installs Grub2 on top of your existing boot loader. If you'd like to boot
into `/old-root`, you can; you just need to add the Grub entry manually.

## Platform-specifics
### Ubuntu
```bash
$ apt-get install -y squashfs-tools
```

### Debian 8.2
```bash
$ apt-get install -y squashfs-tools
```

### Arch
Arch puts `/tmp` onto its own tmpfs, so you may get a warning saying there may
not be enough space. Double check on your own, with `df -h` and feel free to
continue.
```bash
$ pacman -Sy wget squashfs-tools
```

### CentOS 7
CentOS uses LVM, which is not currently supported. I've been able to get NixOS
installed, but the kernel then fails to setup the appropriate file systems. This
is a WIP.
```bash
$ yum install wget squashfs-tools
```

## How it works
The provided `install` script will verify your system is sane, allow you to
configure some options, and then pull down the latest ISO from NixOS. It them
enters Stage 1.

### Stage 1
Once we have the NixOS live CD ISO, we mount is locally and modify it. The
modifications enable us to imbue the image with a chroot that runs Stage 2.
Before we chroot, we bind in a number of the host's devices and files into the
chroot environment to ensure that functionality like networking will work.

### Stage 2
Once we're in the NixOS live CD chroot, we modify the NixOS configs to convey
that it's not a typical setup. After that, we install to `/nixos`, which is
bound to the host's `/nixos`. NixOS then installs Grub and we're good to reboot
into our new machine!

## Thanks
Both [lethalman](https://github.com/lethalman) and
[cleverca22](https://github.com/cleverca22) were very helpful in getting this
working.  Muchas gracias.

## Donate
Feel free to shoot Bitcoins my way: **1HaMvpDjy7QJBDkcZALJr3s26FxLpv5WtJ**

For more information regarding how I use donations, see
[here](http://jeaye.com/donate/).
