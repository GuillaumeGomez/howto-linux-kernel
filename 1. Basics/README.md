Basics
======

You now have two possibilities:
 * You download a linux (ubuntu for example) and you launch it in a virtual machine.
 * You download qemu and the linux kernel.

Since there already have a lot of tutorials that explain how to run a linux in a virtual machine, I won't give an explanation. So you can move to another tutorial. However, for the others, I'll explain step by step how to run linux kernel with qemu.

First, download [qemu](http://wiki.qemu.org/Download).

Then, we have to build a busybox. Here are the commands you have to enter:
```Shell
cd # to go back to your home directory
mkdir linux_test # now we create a linux folder where everything will be done
cd linux_test
git clone git://git.busybox.net/busybox
make defconfig # All features, no debug
make menuconfig # Setup static linking
make
sudo make install # You can do this without being root but it can fails if you're not
```

Now let's build a rootfs (without it, launching linux kernel will be hard !):
```Shell
dd if=/dev/zero of=disk.img bs=1M count=16
mkfs.ext4 disk.img -L root
mkdir mnt
mount disk.img mnt
cp -r ../busybox/_install/* mnt # the busybox can be different on your computer, just replace it with yours
# Setup mounts
cd mnt
mkdir -p etc/init.d proc sys dev
echo 'proc    /proc   proc    defaults' >> etc/fstab
echo 'sysfs   /sys    sysfs   defaults' >> etc/fstab
echo 'mount -a' >> etc/init.d/rcS
chmod +x etc/init.d/rcS
```

Now let's get to the long part. Don't worry, it's not complicated, just very long. You'll understand why in a few seconds. Now let's download and compile the linux kernel:
```Shell
git clone --depth 1 git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
make defconfig
make
```

![compiling !](http://imgs.xkcd.com/comics/compiling.png)

And now just wait. Take a coffee or something.

Done ? Good ! Now we can start !

##Launching qemu

Very simple:

```Shell
qemu-system-x86_64 -kernel /path/to/linux/arch/x86/boot/bzImage -hda /path/to/rootfs/disk.img -append "root=/dev/sda console=ttyS0" -nographic
```

If it launched successfully, congratulations ! To quit qemu, you now just have to kill it (no kidding). To help you:

```Shell
kill $(ps aux | grep '[q]emu' | awk '{print $2}')
```

Here is ending the first tutorial.