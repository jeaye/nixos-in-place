# nixos-in-place

## Debian-based (including Ubuntu)
```bash
$ [sudo] apt-get install -y squashfs-tools
```

TODO:
  logs
  describe the steps in the readme
  test on several distros
  list out the dependencies for each distro
  allow specifying the working dir; go to /tmp, by default
  add grub entry to boot previous distro (before stage2)
  check for available memory; warn (prompt) if it looks like too little
  allow -G for graphical iso https://nixos.org/releases/nixos/latest-iso-graphical-x86_64-linux
