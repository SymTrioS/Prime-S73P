## Preparing a uSD card:

Prime-S73P/uSD$ gparted

*Create two partitions: part1=~16M,fat16; part2=ext4*

*Remember the letter designation of the Flash device, below it is designated as x: sdx1,sdx2*

*Format partitions:*

Prime-S73P/uSD$ sudo mkfs.vfat -F 16 -n boot -v /dev/sdx1  
Prime-S73P/uSD$ sudo mkfs.ext4 -L rootfsf -O ^metadata_csum,^64bit /dev/sdx2

*Write u-boot:*

Prime-S73P/uSD$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdx bs=1024 seek=8

*Write Linux Kernel:*

Prime-S73P/uSD$ sudo mount /dev/sdx1 /mnt  
Prime-S73P/uSD$ sudo cp zImage /mnt  
Prime-S73P/uSD$ sudo cp boot.scr /mnt  
Prime-S73P/uSD$ sudo cp suniv-f1c200s-prime-s.dtb /mnt  
Prime-S73P/uSD$ sync  
Prime-S73P/uSD$ sudo umount /dev/sdx1  

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A/debian12.rootfsf.tar rootFS in uSD folder or use another build*

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A/linux-5.2-fps.zip Linux drivers library and unzip in uSD folder*

*Write RootFS debian12:*

Prime-S73P/uSD$ sudo mount /dev/sdx2 /mnt  
Prime-S73P/uSD$ sudo tar -C /mnt/ -xf debian12.rootfsf.tar  
Prime-S73P/uSD$ sync  
Prime-S73P/uSD$ sudo mkdir -p /mnt/usr/lib/modules  
Prime-S73P/uSD$ sudo rm -rf /mnt/usr/lib/modules/  
Prime-S73P/uSD$ cd linux-5.2-fps  
Prime-S73P/uSD/linux-5.2-fps$ sudo cp -r out/lib /mnt/usr/  
Prime-S73P/uSD/linux-5.2-fps$ sync  
Prime-S73P/uSD/linux-5.2-fps$ sudo umount /dev/sdx2  

