# Building the kernel

```bash
sudo make -j 8 menuconfig
sudo make -j 8 modules_install
sudo make -j 8 install
sudo update-initramfs -c -k ($Version)
reboot
gcc hello_test.c -o hello.o && ./hello.o
dmesg
```