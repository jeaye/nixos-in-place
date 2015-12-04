# nixos-in-place

## Debian-based (including Ubuntu)
```bash
$ [sudo] apt-get install -y squashfs-tools
```

TODO:
  logs
  specify the proper device, not /dev/sda
  allow changing of ext4
  describe the steps in the readme
  test on several distros
  list out the dependencies for each distro
  allow specifying the working dir; go to /tmp, by default
  add grub entry to boot previous distro (after stage2)
