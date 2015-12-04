# nixos-in-place

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
  test /dev/disk/by-id
  check for x64 and download the right ISO
