<h2 align="center">Preparing a usD card:</h2>

mk@mpc:~/Prime-S73P/uSD$ gparted

*Create two partitions: part1=~16M,fat16; part2=ext4*

*Remember the letter designation of the Flash device, below it is designated as x: sdx1,sdx2*

*Format partitions:*

mk@mpc:~/Prime-S73P/uSD$ sudo mkfs.vfat -F 16 -n boot -v /dev/sdx1

mk@mpc:~/Prime-S73P/uSD$ sudo mkfs.ext4 -L rootfsf -O ^metadata_csum,^64bit /dev/sdx2

*Write u-boot:*

mk@mpc:~/Prime-S73P/uSD$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdx bs=1024 seek=8

*Write Linux Kernel:*

mk@mpc:~/Prime-S73P/uSD$ sudo mount /dev/sdx1 /mnt

mk@mpc:~/Prime-S73P/uSD$ sudo cp zImage /mnt

mk@mpc:~/Prime-S73P/uSD$ sudo cp boot.scr /mnt

mk@mpc:~/Prime-S73P/uSD$ sudo cp suniv-f1c200s-prime-s.dtb /mnt

mk@mpc:~/Prime-S73P/uSD$ sync

mk@mpc:~/Prime-S73P/uSD$ sudo umount /dev/sdx1

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A rootFS in uSD folder or use another build*

*Write RootFS debian12:*

mk@mpc:~/Prime-S73P/uSD$ sudo mount /dev/sdx2 /mnt

mk@mpc:~/Prime-S73P/uSD$ sudo tar -C /mnt/ -xf debian12.rootfsf.tar

mk@mpc:~/Prime-S73P/uSD$ sync

mk@mpc:~/Prime-S73P/uSD$ sudo mkdir -p /mnt/usr/lib/modules

mk@mpc:~/Prime-S73P/uSD$ sudo rm -rf /mnt/usr/lib/modules/

mk@mpc:~/Prime-S73P/uSD$ cd linux-5.2-fp

mk@mpc:~/Prime-S73P/uSD/linux-5.2-fps$ sudo cp -r out/lib /mnt/usr/

mk@mpc:~/Prime-S73P/uSD/linux-5.2-fps$ sync

mk@mpc:~/Prime-S73P/uSD/linux-5.2-fps$ sudo umount /dev/sdx2

