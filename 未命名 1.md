CMCCTelnet@FM104-TW:~ # cat /proc/cmdline 
console=ttyS0,115200 clk_ignore_unused root=/dev/mtdblock5 mtdparts=qspi-nand:640k@0x0(uboot),128k@0xa0000(hwconfig),41600k@0xc0000(rootfs-1),49152k@0x2960000(framework-1),8192k@0x5960000(userconfig),41600k@0x6160000(rootfs-2),49152k@0x8a00000(framework-2),65280k@0xba00000(framework-apps),256k@0xf9c0000(backup-config) init=/sbin/init bootv=V1.2.0(02/27/2024-20:19:58) rootwait user_debug=0xff
CMCCTelnet@FM104-TW:~ # cat /proc/mtd 
dev:    size   erasesize  name
mtd0: 000a0000 00020000 "uboot"
mtd1: 00020000 00020000 "hwconfig"
mtd2: 028a0000 00020000 "rootfs-1"
mtd3: 03000000 00020000 "framework-1"
mtd4: 00800000 00020000 "userconfig"
mtd5: 028a0000 00020000 "rootfs-2"
mtd6: 03000000 00020000 "framework-2"
mtd7: 03fc0000 00020000 "framework-apps"
mtd8: 00040000 00020000 "backup-config"

 
console=ttyS0,115200 clk_ignore_unused root=/dev/mtdblock2 mtdparts=qspi-nand:640k@0x0(uboot),128k@0xa0000(hwconfig),41728k@0xc0000(rootfs-1),49152k@0x2980000(framework-1),8192k@0x5980000(userconfig),41728k@0x6180000(rootfs-2),49152k@0x8a40000(framework-2),65024k@0xba40000(framework-apps),256k@0xf9c0000(panic),256k@0xfa00000(backup-config) init=/sbin/init bootv=V1.2.0(03/20/2024-20:35:55) rootwait user_debug=0xff
CMCCTelnet@FM104-TW:~ # cat /proc/mtd 
dev:    size   erasesize  name
mtd0: 000a0000 00020000 "uboot"
mtd1: 00020000 00020000 "hwconfig"
mtd2: 028c0000 00020000 "rootfs-1"
mtd3: 03000000 00020000 "framework-1"
mtd4: 00800000 00020000 "userconfig"
mtd5: 028c0000 00020000 "rootfs-2"
mtd6: 03000000 00020000 "framework-2"
mtd7: 03f80000 00020000 "framework-apps"
mtd8: 00040000 00020000 "panic"
