# nixos-in-place
An all-in-one script for installing NixOS on top of any existing Linux
distribution.

## How to do it
0. **BACKUP**
1. Install the dependencies
2. Run the following (see `./install -h` for options)
```bash
$ ./install
```
3. Hit `y` to verify the installation
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

## Dependencies
### Debian-based (including Ubuntu)
```bash
$ apt-get install -y squashfs-tools
```

### Arch
```bash
$ pacman -Sy squashfs-tools
```
