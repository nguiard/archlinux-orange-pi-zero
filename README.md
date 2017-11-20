# Arch Linux on OrangePi Zero

Here is a working SD card image for installing Arch Linux on an OrangePi Zero. It basically follows [this excellent build guide][build-guide] but can save you a lot of time if you just want to get an image up an running, especially with getting the ethernet support to work.

The image is located [here][image].

[build-guide]: https://github.com/ubitux/archlinuxarm-orangepi_zero
[image]: https://dl.ng3.io/opz/ArchLinuxARM-OrangePiZero-20171120.img.xz

## Install

You need an SD card of at least 2GB. You can resize the partition to use your whole SD card after installing.

### Quick'n dirty

Replace 'sdX' with the device corresponding to your SD card.

```
curl https://dl.ng3.io/opz/ArchLinuxARM-OrangePiZero-20171120.img.xz | xzcat | sudo dd of=/dev/sdX bs=1M status=progress
```

### Safer

Download the image, check the checksum, and install the traditional way. SHA256:

```
d0f676f1228f288cce857728ed8176409553dc9c239bd5d053cfc61b9c592325  ArchLinuxARM-OrangePiZero-20171120.img.xz
```

Steps:

```
$ wget https://dl.ng3.io/opz/ArchLinuxARM-OrangePiZero-20171120.img.xz
$ sha256sum ArchLinuxARM-OrangePiZero-20171120.img.xz
d0f676f1228f288cce857728ed8176409553dc9c239bd5d053cfc61b9c592325  ArchLinuxARM-OrangePiZero-20171120.img.xz
$ xzcat ArchLinuxARM-OrangePiZero-20171120.img.xz | sudo dd of=/dev/sdX bs=1M status=progress
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

## Important Warning!

The linux kernel is pinned to version 4.13-rc7 in this image. This is unfortunate but is required in order to keep the ethernet support working, as newer versions break it at the time of writing. If you try a newer kernel and get it to work, please notify me.

## Additional resources

- Arch forum thread about Orange Pi Zero installation: [https://archlinuxarm.org/forum/viewtopic.php?f=60&t=11790][forum-arch]

[forum-arch]: https://archlinuxarm.org/forum/viewtopic.php?f=60&t=11790
