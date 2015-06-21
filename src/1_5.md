5. Add executable in qemu
=========================

It can be useful to have a very specific executable in qemu since there is a very few number of them inside. Actually it's quite simple. Compile your program with the --static option. Example:

```Shell
gcc --static file_to_compile.c -o super_executable
```

Now go to your linux folder (where the mnt folder is) and mount the linux image:

```Shell
sudo mount disk.img mnt
```

Then you just have to copy your executable in it:

```Shell
cp super_executable mnt/bin/
```

Next time you'll launch qemu, you will be able to run your super_executable !