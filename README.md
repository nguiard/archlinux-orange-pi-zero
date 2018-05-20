# Arch Linux on OrangePi Zero

Here is a working SD card image for installing Arch Linux on an OrangePi Zero. It basically follows [this excellent build guide][build-guide] but can save you a lot of time if you just want to get an image up an running, especially with getting the ethernet support to work.

The image is located [here][image]. (It is an unofficial image, not supported by ArchLinux, use at your own risk)

[build-guide]: https://github.com/ubitux/archlinuxarm-orangepi_zero
[image]: https://dl.ng3.io/opz/ArchLinuxARM-OrangePiZero-latest.img.xz

## Install

You need an SD card of at least 2GB. You can resize the partition to use your whole SD card after installing.

### Quick'n dirty

Replace 'sdX' with the device corresponding to your SD card.

```
curl https://dl.ng3.io/opz/ArchLinuxARM-OrangePiZero-latest.img.xz | xzcat | sudo dd of=/dev/sdX bs=1M status=progress
```

### Safer

Download the image, check the checksum, and install the traditional way. SHA256:

```
7265860878c03acff4f3ea36d66eb8c806a1ba2ec9589498176a0889869ddaba  ArchLinuxARM-OrangePiZero-latest.img.xz
```

Steps:

```
$ wget https://dl.ng3.io/opz/ArchLinuxARM-OrangePiZero-latest.img.xz
$ sha256sum ArchLinuxARM-OrangePiZero-latest.img.xz
7265860878c03acff4f3ea36d66eb8c806a1ba2ec9589498176a0889869ddaba  ArchLinuxARM-OrangePiZero-latest.img.xz
$ xzcat ArchLinuxARM-OrangePiZero-latest.img.xz | sudo dd of=/dev/sdX bs=1M status=progress
```

On Mac, use `dd of=/dev/rdiskX bs=1m conv=sync` (see [why dd is slow on Mac][slow-dd-mac]).

On Windows, use a tool like [Etcher][etcher].

[slow-dd-mac]: http://daoyuan.li/solution-dd-too-slow-on-mac-os-x/
[etcher]: https://etcher.io/

### Resize the partition to use the whole SD card

Once the system is working, use `fdisk /dev/mmcblk0` ([fdisk][fdisk]) as root to resize the partition. Press `d` to delete the current partition, then press `n` to create a new partition, and press `Enter` 4 times to accept the defaults. fdisk will give you defaults that work and make the partition as big as possible. Then answer `N` when fdisk asks about removing the existing ext4 signature.

Finally press `w` to write the changes.

Reboot, and use `resize2fs /dev/mmcblk0p1` as root to update the filesystem size. You're done!

[fdisk]: https://wiki.archlinux.org/index.php/Fdisk

## Ethernet

Ethernet now works with linux-armv7-rc 4.17.rc5-1 and most probably newer versions.

Feedbacks and comments are welcome.

## Additional resources

- Arch forum thread about Orange Pi Zero installation: [https://archlinuxarm.org/forum/viewtopic.php?f=60&t=11790][forum-arch]

[forum-arch]: https://archlinuxarm.org/forum/viewtopic.php?f=60&t=11790
