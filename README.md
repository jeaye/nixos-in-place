# nixos-in-place

## How to do it
1. Install the dependencies
2. Run the following (see `./install -h` for options)
```bash
# As root!
./install
```
3. Hit `y` to verify the installation
4. Grab some coffee while NixOS installs
5. Type in your new root password
6. Hit `y` to reboot into NixOS

## What you get
A fresh install of NixOS 15.09, either minimal or graphical (your choice). Your
old system and all your old files still exist and are setup to mount on
`/old-root`. As far as the file system is concerned, NixOS is installed in
`/old-root/nixos` and `/` is rebound before spinning up the system; everything
else in `/old-root` is fair game to delete.

## Debian-based (including Ubuntu)
```bash
$ [sudo] apt-get install -y squashfs-tools
```

TODO:
  describe the steps in the readme
  test on several distros
  list out the dependencies for each distro
  add grub entry to boot previous distro (before stage2)
  check for available memory; warn (prompt) if it looks like too little
  find out how much memory it actually uses
    minimal: 3GB
  clean up mounts
