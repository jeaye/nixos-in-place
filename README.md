# nixos-in-place
An all-in-one script for installing NixOS on top of any existing Linux
distribution.

nixos-in-place is known to work on Ubuntu, Debian, CentOS, Fedora, Arch, and
Slackware, including x86 and x86_64 variants with and without LVM.

## How to do it
0. **BACKUP**
1. See the
   [Platform-specifics](https://github.com/jeaye/nixos-in-place#platform-specifics)
   for your distribution
2. Run the following (see `./install -h` for options)
```bash
$ ./install
```
3. **STOP AND VERIFY**, then hit `y` to confirm
4. Grab some coffee while NixOS installs
5. Type in your new root password
6. Hit `y` to reboot into NixOS!

## What you get
A fresh install of NixOS 15.09, either minimal or graphical (your choice). Your
old system and all your old files still exist and are setup to mount on
`/old-root`. As far as the file system is concerned, NixOS is installed in
`/old-root/nixos` and `/` is rebound before spinning up the system; everything
else in `/old-root` is fair game to delete.

NixOS installs GRUB2 on top of your existing boot loader. If you'd like to boot
into `/old-root`, you can; you just need to add the GRUB entry manually in your
Nix files.

## Platform-specifics
### Ubuntu 15.10
See [LVM](https://github.com/jeaye/nixos-in-place#lvm).
```bash
$ apt-get install -y squashfs-tools
```

### Debian 8.2
See [LVM](https://github.com/jeaye/nixos-in-place#lvm).
```bash
$ apt-get install -y squashfs-tools
```

### Arch
See [tmpfs](https://github.com/jeaye/nixos-in-place#tmpfs).
```bash
$ pacman -Sy wget squashfs-tools
```

### CentOS 7
See [LVM](https://github.com/jeaye/nixos-in-place#lvm).
```bash
$ yum install wget squashfs-tools
```

### Fedora 23
See [LVM](https://github.com/jeaye/nixos-in-place#lvm) and
[tmpfs](https://github.com/jeaye/nixos-in-place#tmpfs).
```bash
$ dnf install squashfs-tools
```

### Slackware 14.1 (and -current)
Both Slackware 14.1 and -currrent have a bug where `/proc/self/mountinfo`
references a non-existent `/dev/root`. You'll need to manually create a link to
your file system root for GRUB to install properly. Example: `ln -s /dev/sda1
/dev/root`.  For the dependencies, you'll need to go through AUR; use
[sbopkg](http://blog.jeaye.com/2015/07/09/sbopkg/) or something else.
```bash
$ sbopkg squashfs-tools
```

### LVM
Systems may use LVM. In which case, you'll need to specify `-g` with the proper
device for GRUB (likely `/dev/sda`). If your guessed GRUB device looks like
`/dev/mapper/xxx` and not `/dev/xxx` then you're likely using LVM.

### tmpfs
Systems may put `/tmp` onto its own tmpfs, so you may get a warning saying there
may not be enough space. Double check on your own, with `df -h`, and feel free
to continue.

## How it works
In short:

1. Install into `/nixos`
2. Wipe out boot loader with GRUB
3. Bind `/nixos` to `/`
4. Bind the old `/` to `/old-root`

More descriptively, the provided `install` script will verify your system is
sane, allow you to configure some options, and then pull down the latest ISO
from NixOS. It them enters Stage 1.

### Stage 1
Once we have the NixOS live CD ISO, we mount is locally and modify it. The
modifications enable us to imbue the image with a chroot that runs Stage 2.
Before we chroot, we bind in a number of the host's devices and files into the
chroot environment to ensure that functionality like networking will work.

### Stage 2
Once we're in the NixOS live CD chroot, we modify the NixOS configs to convey
that it's not a typical setup. After that, we install to `/nixos`, which is
bound to the host's `/nixos`. NixOS then installs GRUB and we're good to reboot
into our new machine!

## Thanks
Both [lethalman](https://github.com/lethalman) and
[cleverca22](https://github.com/cleverca22) were very helpful in getting this
working.  Muchas gracias.

## Donate
Feel free to shoot Bitcoins my way: **1HaMvpDjy7QJBDkcZALJr3s26FxLpv5WtJ**

For more information regarding how I use donations, see
[here](http://jeaye.com/donate/).
