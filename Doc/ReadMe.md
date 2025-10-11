<h2 align="center">Description files</h2>

- Prime-S73P_Func.pdf - board functional diagram;

- Prime-S73P_Con.pdf - component signals available on external connectors;

- Prime-S73P_Brd.pdf - on-board component connections;

- Prime-S73P_Dim.pdf - board dimensions, connector locations, and pin numbering;

<h2 align="center">Linux system build procedure</h2>

**Package installation**  
$ sudo apt install build-essential  
$ sudo apt install bison flex swig git  
$ sudo apt install openssl libssl-dev libelf-dev  
$ sudo apt install libudev-dev libpci-dev libiberty-dev  
$ sudo apt install libncurses5-dev gparted python3  
$ sudo apt install dkms autoconf  

**Installing cross-compiler**  
*get file https://disk.yandex.ru/d/VCLHKDqusnvv8A/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabi.tar.xz*  
$ tar xvf gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabi.tar.xz  
$ sudo mv gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabi /opt/  
$ export CC=/opt/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabi/bin/arm-linux-gnueabi-  
*availability check*  
$ ${CC}gcc -v  

**Install Python for binman u-boot olders version**  
$ wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz  
*or get https://disk.yandex.ru/d/VCLHKDqusnvv8A/Python-2.7.18.tgz*  
$ tar xzf Python-2.7.18.tgz  
$ cd Python-2.7.18  
$ sudo ./configure --enable-optimizations  
$ sudo make altinstall  
$ sudo ln -s "/usr/local/bin/python2.7" "/usr/bin/python"  
$ sudo ln -s "/usr/local/bin/python2.7" "/usr/bin/python2"  

**Creating a working directory**  
$ mkdir Prime-S  
$ cd Prime-S  
Prime-S$ mkdir uSD  

**U-BOOT**  
Prime-S$ git clone https://github.com/SymTrioS/u-boot-s.git  
Prime-S$ cd u-boot-s  
Prime-S/u-boot-s$ make ARCH=arm CROSS_COMPILE=${CC} distclean  
Prime-S/u-boot-s$ make ARCH=arm CROSS_COMPILE=${CC} prime-s_defconfig  
Prime-S/u-boot-s$ make ARCH=arm CROSS_COMPILE=${CC} menuconfig  
*making necessary changes, save-exit*  
Prime-S/u-boot-s$ make ARCH=arm CROSS_COMPILE=${CC} -j16  
Prime-S/u-boot-s$ cp u-boot-sunxi-with-spl.bin ../uSD/  
Prime-S/u-boot-s$ cd ..  

**KERNEL**  
Prime-S$ git clone https://github.com/SymTrioS/linux-5.2-fps.git  
Prime-S$ cd linux-5.2-fps  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} distclean  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} prime-s_defconfig  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} menuconfig  
*making necessary changes, save-exit*  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} -j16  
*correct line 629 of the file scripts/dtc/dtc-lexer.lex.c "extern YYLTYPE yylloc;"*  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} -j16  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} modules  
Prime-S/linux-5.2-fps$ make ARCH=arm CROSS_COMPILE=${CC} INSTALL_MOD_PATH=out modules_install  
Prime-S/linux-5.2-fps$ cp arch/arm/boot/zImage ../uSD/  
Prime-S/linux-5.2-fps$ cp arch/arm/boot/dts/suniv-f1c200s-prime-s.dtb ../uSD/  
Prime-S/linux-5.2-fps$ cd ..  

**Preparing the uSD card**  
Prime-S$ cd uSD  
*create text file boot.cmd*  
Prime-S/uSD$ nano boot.cmd  
*add the following lines to a text file and save*  

setenv bootargs console=tty0 console=ttyS0,115200 panic=5 rootwait root=/dev/mmcblk0p2 rw  
load mmc 0:1 0x80C00000 suniv-f1c200s-prime-s.dtb  
load mmc 0:1 0x80008000 zImage  
bootz 0x80008000 - 0x80C00000  

Prime-S/uSD$ mkimage -C none -A arm -T script -d boot.cmd boot.scr  

*Insert, partition, and format the memory card*  
Prime-S/uSD$ gparted  
*Create two partitions: part1=~16M,fat16; part2=ext4*  
*Remember the letter designation of the Flash device, below it is designated as x: sdx1,sdx2*  
*Format partitions:*  
Prime-S/uSD$ sudo mkfs.vfat -F 16 -n boot -v /dev/sdx1  
Prime-S/uSD$ sudo mkfs.ext4 -L rootfsf -O ^metadata_csum,^64bit /dev/sdx2  
*Write u-boot:*  
Prime-S/uSD$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdx bs=1024 seek=8  
*Write Linux Kernel:*  
Prime-S/uSD$ sudo mount /dev/sdx1 /mnt  
Prime-S/uSD$ sudo cp zImage /mnt  
Prime-S/uSD$ sudo cp boot.scr /mnt  
Prime-S/uSD$ sudo cp suniv-f1c200s-prime-s.dtb /mnt  
Prime-S/uSD$ sync  
Prime-S/uSD$ sudo umount /dev/sdx1  

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A/debian12.rootfsf.tar rootFS in uSD folder or use another build*  

Prime-S/uSD$ sudo mount /dev/sdx2 /mnt  
Prime-S/uSD$ sudo tar -C /mnt/ -xf debian12.rootfsf.tar  
Prime-S/uSD$ sync  
Prime-S/uSD$ sudo mkdir -p /mnt/usr/lib/modules  
Prime-S/uSD$ sudo rm -rf /mnt/usr/lib/modules/  
Prime-S/uSD$ cd ../linux-5.2-fps  
Prime-S/uSD/linux-5.2-fps$ sudo cp -r out/lib /mnt/usr/  
Prime-S/uSD/linux-5.2-fp$ sync  
Prime-S/uSD/linux-5.2-fp$ sudo umount /dev/sdx2  

