# nixos-in-place
An all-in-one script for installing NixOS on top of any existing Linux
system without using live media. When you reboot, you're in NixOS.

nixos-in-place is known to work on Ubuntu, Debian, CentOS, Fedora, Arch, and
Slackware, including x86 and x86_64 variants, with and without LVM, including
systems on [Digital
Ocean](https://github.com/jeaye/nixos-in-place#digital-ocean) droplets and
[Hetzner Cloud](https://github.com/jeaye/nixos-in-place/issues/41)!

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
5. Hit `y` to reboot into NixOS! (root's password will be 'nixos')
6. Remove hard-coded password from `/nixos/etc/nixos/nixos-in-place.nix` and set
   one manually

## What you get
A fresh install of NixOS, either minimal or graphical (your choice). Your
old system and all your old files still exist and are setup to mount on
`/old-root`. As far as the file system is concerned, NixOS is installed in
`/old-root/nixos` and `/` is rebound before spinning up the system; everything
else in `/old-root` is fair game to delete.

NixOS installs GRUB2 on top of your existing boot loader. If you'd like to boot
into `/old-root`, you can; you just need to add the GRUB entry, from
`/old-root/boot/grub`, manually in your Nix files.

## Platform-specifics
### Digital Ocean
For use on DO droplets, follow the normal steps for your platform (Debian has
shown the best results) but add the `-d` flag to `./install` (see `-h` for more
info). Once installed, if you clean up `/old-root`, you must keep
`/old-root/etc/network` around; DO needs it!

The default configuration for NixOS disables SSH, so you'll need to use the DO
console, once you've finished the installation, in order to setup which services
you'd actually like.

I recommend installing from a tmux session, to avoid SSH timeouts and losing
access to your install part-way through. Seriously, use tmux or screen.

### Ubuntu 15.10
See [LVM](https://github.com/jeaye/nixos-in-place#lvm).
```bash
$ apt-get install -y squashfs-tools git
```

### Debian 8.2
See [LVM](https://github.com/jeaye/nixos-in-place#lvm).
```bash
$ apt-get install -y squashfs-tools git
```

### Arch
See [tmpfs](https://github.com/jeaye/nixos-in-place#tmpfs).
```bash
$ pacman -Sy wget squashfs-tools git
```

### CentOS 7
See [LVM](https://github.com/jeaye/nixos-in-place#lvm).
```bash
$ yum -y install wget squashfs-tools git
```

### Fedora 23
See [LVM](https://github.com/jeaye/nixos-in-place#lvm) and
[tmpfs](https://github.com/jeaye/nixos-in-place#tmpfs).
```bash
$ dnf -y install wget squashfs-tools git
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

### All platforms
It's recommended that you have 1GB of available RAM for unsquashing the NixOS
live media. If you're on a machine with less RAM, such as a VPS with 512MB, you
can quickly make a swap file for the install.

```bash
$ dd if=/dev/zero of=swap bs=1M count=1024
$ mkswap ./swap
$ chmod 600 ./swap
$ swapon ./swap
```

To remove it, after the install.

```bash
$ swapoff ./swap
$ rm -f ./swap
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
Once we have the NixOS live CD ISO, we mount it locally and modify it. The
modifications enable us to imbue the image with a chroot that runs Stage 2.
Before we chroot, we bind in a number of the host's devices and files into the
chroot environment to ensure that functionality like networking will work.

### Stage 2
Once we're in the NixOS live CD chroot, we modify the NixOS configs to convey
that it's not a typical setup. After that, we install to `/nixos`, which is
bound to the host's `/nixos`. NixOS then installs GRUB and we're good to reboot
into our new machine!

## Testing
The testing suite can be run like so (requires vagrant):

```bash
$ ./test/run-all
```
