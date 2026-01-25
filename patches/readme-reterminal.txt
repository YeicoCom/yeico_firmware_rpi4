status
 - works but dmesg shows kernel panics
 - wont impact other available touch panels
 - unable to rotate on boot
 - touch slightly off to the top left
cause
 - starts too early missing dependencies?
next steps 
 - review other potential dependencies
 - add accelerometer driver

got src from https://github.com/YeicoCom/seeed-linux-dtoverlays/tree/master/modules/mipi_dsi

from within ./shell.sh

rm ~/yeico_firmware_rpi4/linux/0002-panel-reterminal-mipi_dsi.patch
make linux-dirclean
make linux-patch
cp -fr build/linux-custom build/linux-patched
cp -fr ~/yeico_firmware_rpi4/patches/reterminal/* build/linux-custom/drivers/gpu/drm/panel/
(cd build && diff -ruN linux-patched linux-custom > ~/yeico_firmware_rpi4/linux/0002-panel-reterminal-mipi_dsi.patch)
cat ~/yeico_firmware_rpi4/linux/0002-panel-reterminal-mipi_dsi.patch

make linux-dirclean
make linux-rebuild
make

./artifac.sh

compare to https://github.com/YeicoCom/yeico_terminal/tree/reterminal/fw/boards/seeedstudio-cm4-reterminal/raspbian

iex(8)> dmesg
[    0.000000] Booting Linux on physical CPU 0x0000000000 [0x410fd083]
[    0.000000] Linux version 6.6.74-rt48-v8 (buildroot@buildroot) (aarch64-nerves-linux-gnu-gcc (crosstool-NG UNKNOWN) 13.2.0, GNU ld (crosstool-NG UNKNOWN) 2.40) #1 SMP PREEMPT_RT Sat Oct 11 21:20:00 UTC 2025
[    0.000000] random: crng init done
[    0.000000] Machine model: Raspberry Pi Compute Module 4 Rev 1.0
[    0.000000] Reserved memory: created CMA memory pool at 0x000000000ec00000, size 512 MiB
[    0.000000] OF: reserved mem: initialized node linux,cma, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x000000000ec00000..0x000000002ebfffff (524288 KiB) map reusable linux,cma
[    0.000000] OF: reserved mem: 0x000000003f12be60..0x000000003f12bf84 (0 KiB) nomap non-reusable nvram@0
[    0.000000] Zone ranges:
[    0.000000]   DMA      [mem 0x0000000000000000-0x000000003fffffff]
[    0.000000]   DMA32    [mem 0x0000000040000000-0x00000000fbffffff]
[    0.000000]   Normal   empty
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000000000000-0x0000000033ffffff]
[    0.000000]   node   0: [mem 0x0000000040000000-0x00000000fbffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000000000000-0x00000000fbffffff]
[    0.000000] On node 0, zone DMA32: 16384 pages in unavailable ranges
[    0.000000] On node 0, zone DMA32: 16384 pages in unavailable ranges
[    0.000000] percpu: Embedded 18 pages/cpu s34048 r8192 d31488 u73728
[    0.000000] pcpu-alloc: s34048 r8192 d31488 u73728 alloc=18*4096
[    0.000000] pcpu-alloc: [0] 0 [0] 1 [0] 2 [0] 3 
[    0.000000] Detected PIPT I-cache on CPU0
[    0.000000] CPU features: detected: Spectre-v2
[    0.000000] CPU features: detected: Spectre-v4
[    0.000000] CPU features: detected: Spectre-BHB
[    0.000000] CPU features: detected: ARM errata 1165522, 1319367, or 1530923
[    0.000000] alternatives: applying boot alternatives
[    0.000000] Kernel command line: coherent_pool=1M 8250.nr_uarts=1 snd_bcm2835.enable_headphones=0 cgroup_disable=memory numa_policy=interleave snd_bcm2835.enable_hdmi=0 snd_bcm2835.enable_hdmi=0  smsc95xx.macaddr=E4:5F:01:91:4A:4E vc_mem.mem_base=0x3ec00000 vc_mem.mem_size=0x40000000  dwc_otg.lpm_enable=0 console=ttyS0,115200 root=/dev/mmcblk0p2 rootfstype=squashfs rootwait consoleblank=0 quiet vt.global_cursor_default=0
[    0.000000] cgroup: Disabling memory control group subsystem
[    0.000000] Unknown kernel command line parameters "numa_policy=interleave", will be passed to user space.
[    0.000000] Dentry cache hash table entries: 524288 (order: 10, 4194304 bytes, linear)
[    0.000000] Inode-cache hash table entries: 262144 (order: 9, 2097152 bytes, linear)
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 967680
[    0.000000] mem auto-init: stack:all(zero), heap alloc:off, heap free:off
[    0.000000] software IO TLB: area num 4.
[    0.000000] software IO TLB: mapped [mem 0x0000000030000000-0x0000000034000000] (64MB)
[    0.000000] Memory: 3251188K/3932160K available (7360K kernel code, 898K rwdata, 1880K rodata, 1472K init, 603K bss, 156684K reserved, 524288K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
[    0.000000] rcu: Preemptible hierarchical RCU implementation.
[    0.000000] rcu:     RCU event tracing is enabled.
[    0.000000] rcu:     RCU priority boosting: priority 1 delay 500 ms.
[    0.000000] rcu:     RCU_SOFTIRQ processing moved to rcuc kthreads.
[    0.000000]  No expedited grace period (rcu_normal_after_boot).
[    0.000000]  Trampoline variant of Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 25 jiffies.
[    0.000000] NR_IRQS: 64, nr_irqs: 64, preallocated irqs: 0
[    0.000000] Root IRQ handler: gic_handle_irq
[    0.000000] GIC: Using split EOI/Deactivate mode
[    0.000000] rcu: srcu_init: Setting srcu_struct sizes based on contention.
[    0.000000] arch_timer: cp15 timer(s) running at 54.00MHz (phys).
[    0.000000] clocksource: arch_sys_counter: mask: 0xffffffffffffff max_cycles: 0xc743ce346, max_idle_ns: 440795203123 ns
[    0.000000] sched_clock: 56 bits at 54MHz, resolution 18ns, wraps every 4398046511102ns
[    0.000104] Console: colour dummy device 80x25
[    0.000135] Calibrating delay loop (skipped), value calculated using timer frequency.. 108.00 BogoMIPS (lpj=216000)
[    0.000142] pid_max: default: 32768 minimum: 301
[    0.000319] Mount-cache hash table entries: 8192 (order: 4, 65536 bytes, linear)
[    0.000346] Mountpoint-cache hash table entries: 8192 (order: 4, 65536 bytes, linear)
[    0.001359] RCU Tasks: Setting shift to 2 and lim to 1 rcu_task_cb_adjust=1 rcu_task_cpu_ids=4.
[    0.001503] rcu: Hierarchical SRCU implementation.
[    0.001505] rcu:     Max phase no-delay instances is 1000.
[    0.002147] smp: Bringing up secondary CPUs ...
[    0.002511] Detected PIPT I-cache on CPU1
[    0.002574] CPU1: Booted secondary processor 0x0000000001 [0x410fd083]
[    0.002937] Detected PIPT I-cache on CPU2
[    0.002972] CPU2: Booted secondary processor 0x0000000002 [0x410fd083]
[    0.003340] Detected PIPT I-cache on CPU3
[    0.003377] CPU3: Booted secondary processor 0x0000000003 [0x410fd083]
[    0.003413] smp: Brought up 1 node, 4 CPUs
[    0.003417] SMP: Total of 4 processors activated.
[    0.003420] CPU features: detected: 32-bit EL0 Support
[    0.003422] CPU features: detected: CRC32 instructions
[    0.003460] CPU: All CPU(s) started at EL2
[    0.003462] alternatives: applying system-wide alternatives
[    0.004430] devtmpfs: initialized
[    0.013265] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
[    0.013283] futex hash table entries: 1024 (order: 4, 65536 bytes, linear)
[    0.021971] pinctrl core: initialized pinctrl subsystem
[    0.022531] NET: Registered PF_NETLINK/PF_ROUTE protocol family
[    0.023573] DMA: preallocated 1024 KiB GFP_KERNEL pool for atomic allocations
[    0.023682] DMA: preallocated 1024 KiB GFP_KERNEL|GFP_DMA pool for atomic allocations
[    0.023865] DMA: preallocated 1024 KiB GFP_KERNEL|GFP_DMA32 pool for atomic allocations
[    0.024188] thermal_sys: Registered thermal governor 'step_wise'
[    0.024230] cpuidle: using governor menu
[    0.024328] ASID allocator initialised with 65536 entries
[    0.024412] Serial: AMBA PL011 UART driver
[    0.027217] /soc/cprman@7e101000: Fixed dependency cycle(s) with /soc/dsi@7e700000
[    0.027362] /soc/dsi@7e700000: Fixed dependency cycle(s) with /soc/i2c@7e804000/mipi_dsi@45
[    0.027375] /soc/dsi@7e700000: Fixed dependency cycle(s) with /soc/cprman@7e101000
[    0.027395] /soc/i2c@7e804000/mipi_dsi@45: Fixed dependency cycle(s) with /soc/dsi@7e700000
[    0.027416] /soc/interrupt-controller@40041000: Fixed dependency cycle(s) with /soc/interrupt-controller@40041000
[    0.027647] /soc/cprman@7e101000: Fixed dependency cycle(s) with /soc/dsi@7e700000
[    0.028085] bcm2835-mbox fe00b880.mailbox: mailbox enabled
[    0.028983] /soc/cprman@7e101000: Fixed dependency cycle(s) with /soc/dsi@7e700000
[    0.029032] /soc/dsi@7e700000: Fixed dependency cycle(s) with /soc/i2c@7e804000/mipi_dsi@45
[    0.029044] /soc/dsi@7e700000: Fixed dependency cycle(s) with /soc/cprman@7e101000
[    0.029180] /soc/i2c@7e804000: Fixed dependency cycle(s) with /soc/cprman@7e101000
[    0.029232] /soc/i2c@7e804000/mipi_dsi@45: Fixed dependency cycle(s) with /soc/dsi@7e700000
[    0.036088] raspberrypi-firmware soc:firmware: Attached to firmware from 2024-12-05T11:46:20, variant start_x
[    0.040095] raspberrypi-firmware soc:firmware: Firmware hash is 03554ca336a03ace164f36755144e0d8c060062d
[    0.048122] Modules: 29680 pages in range for non-PLT usage
[    0.048127] Modules: 521200 pages in range for PLT usage
[    0.048828] bcm2835-dma fe007000.dma-controller: DMA legacy API manager, dmachans=0x1
[    0.050087] SCSI subsystem initialized
[    0.050197] usbcore: registered new interface driver usbfs
[    0.050220] usbcore: registered new interface driver hub
[    0.050248] usbcore: registered new device driver usb
[    0.050451] pps_core: LinuxPPS API ver. 1 registered
[    0.050454] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    0.050463] PTP clock support registered
[    0.051765] vgaarb: loaded
[    0.051940] clocksource: Switched to clocksource arch_sys_counter
[    0.056730] NET: Registered PF_INET protocol family
[    0.056974] IP idents hash table entries: 65536 (order: 7, 524288 bytes, linear)
[    0.058829] tcp_listen_portaddr_hash hash table entries: 2048 (order: 4, 81920 bytes, linear)
[    0.058933] Table-perturb hash table entries: 65536 (order: 6, 262144 bytes, linear)
[    0.058943] TCP established hash table entries: 32768 (order: 6, 262144 bytes, linear)
[    0.059127] TCP bind hash table entries: 32768 (order: 9, 2621440 bytes, linear)
[    0.062140] TCP: Hash tables configured (established 32768 bind 32768)
[    0.062304] UDP hash table entries: 2048 (order: 5, 196608 bytes, linear)
[    0.062552] UDP-Lite hash table entries: 2048 (order: 5, 196608 bytes, linear)
[    0.062931] NET: Registered PF_UNIX/PF_LOCAL protocol family
[    0.062980] PCI: CLS 0 bytes, default 64
[    0.063825] Initialise system trusted keyrings
[    0.064006] workingset: timestamp_bits=46 max_order=20 bucket_order=0
[    0.064161] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    0.064567] Key type asymmetric registered
[    0.064571] Asymmetric key parser 'x509' registered
[    0.064599] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 248)
[    0.064628] io scheduler bfq registered
[    0.064918] irq_brcmstb_l2: registered L2 intc (/soc/interrupt-controller@7ef00100, parent irq: 23)
[    0.066123] pinctrl-bcm2835 fe200000.gpio: GPIO_OUT persistence: yes
[    0.069640] brcm-pcie fd500000.pcie: host bridge /scb/pcie@7d500000 ranges:
[    0.069651] brcm-pcie fd500000.pcie:   No bus range found for /scb/pcie@7d500000, using [bus 00-ff]
[    0.069668] brcm-pcie fd500000.pcie:      MEM 0x0600000000..0x067fffffff -> 0x0080000000
[    0.069684] brcm-pcie fd500000.pcie:   IB MEM 0x0000000000..0x00ffffffff -> 0x0400000000
[    0.070418] brcm-pcie fd500000.pcie: PCI host bridge to bus 0000:00
[    0.070426] pci_bus 0000:00: root bus resource [bus 00-ff]
[    0.070432] pci_bus 0000:00: root bus resource [mem 0x600000000-0x67fffffff] (bus address [0x80000000-0xffffffff])
[    0.070456] pci 0000:00:00.0: [14e4:2711] type 01 class 0x060400
[    0.070503] pci 0000:00:00.0: PME# supported from D0 D3hot
[    0.072104] pci 0000:00:00.0: bridge configuration invalid ([bus 00-00]), reconfiguring
[    0.072195] pci_bus 0000:01: supply vpcie3v3 not found, using dummy regulator
[    0.072256] pci_bus 0000:01: supply vpcie3v3aux not found, using dummy regulator
[    0.072267] pci_bus 0000:01: supply vpcie12v not found, using dummy regulator
[    0.499949] brcm-pcie fd500000.pcie: link down
[    0.500022] pci_bus 0000:01: busn_res: [bus 01-ff] end is updated to 01
[    0.500037] pci 0000:00:00.0: PCI bridge to [bus 01]
[    0.500073] pci_bus 0000:01: busn_res: [bus 01] is released
[    0.500343] pci_bus 0000:00: busn_res: [bus 00-ff] is released
[    0.504375] Serial: 8250/16550 driver, 1 ports, IRQ sharing enabled
[    0.505171] iproc-rng200 fe104000.rng: hwrng registered
[    0.505241] vc-mem: phys_addr:0x00000000 mem_base=0x3ec00000 mem_size:0x40000000(1024 MiB)
[    0.510968] brd: module loaded
[    0.513949] loop: module loaded
[    0.514271] Loading iSCSI transport class v2.0-870.
[    0.516480] bcmgenet fd580000.ethernet: GENET 5.0 EPHY: 0x0000
[    0.707979] unimac-mdio unimac-mdio.-19: Broadcom UniMAC MDIO bus
[    0.708743] dwc_otg: version 3.00a 10-AUG-2012 (platform bus)
[    0.708792] dwc_otg: FIQ enabled
[    0.708794] dwc_otg: NAK holdoff enabled
[    0.708795] dwc_otg: FIQ split-transaction FSM enabled
[    0.708800] Module dwc_common_port init
[    0.708900] usbcore: registered new interface driver usb-storage
[    0.708989] UDC core: g_ether: couldn't find an available UDC
[    0.709042] i2c_dev: i2c /dev entries driver
[    0.709581] brcmstb-i2c fef04500.i2c:  @97500hz registered in polling mode
[    0.709796] brcmstb-i2c fef09500.i2c:  @97500hz registered in polling mode
[    0.710795] sdhci: Secure Digital Host Controller Interface driver
[    0.710798] sdhci: Copyright(c) Pierre Ossman
[    0.710898] sdhci-pltfm: SDHCI platform and OF driver helper
[    0.711154] hid: raw HID events driver (C) Jiri Kosina
[    0.711199] usbcore: registered new interface driver usbhid
[    0.711201] usbhid: USB HID core driver
[    0.711250] bcm2835_vchiq fe00b840.mailbox: there is not valid maps for state default
[    0.715143] NET: Registered PF_INET6 protocol family
[    0.715739] Segment Routing with IPv6
[    0.715753] In-situ OAM (IOAM) with IPv6
[    0.715796] NET: Registered PF_PACKET protocol family
[    0.735250] Loading compiled-in X.509 certificates
[    0.744317] uart-pl011 fe201000.serial: there is not valid maps for state default
[    0.744534] uart-pl011 fe201000.serial: cts_event_workaround enabled
[    0.744744] fe201000.serial: ttyAMA1 at MMIO 0xfe201000 (irq = 30, base_baud = 0) is a PL011 rev3
[    0.745331] bcm2835-aux-uart fe215040.serial: there is not valid maps for state default
[    0.745708] printk: console [ttyS0] disabled
[    0.745980] fe215040.serial: ttyS0 at MMIO 0xfe215040 (irq = 31, base_baud = 62500000) is a 16550
[    0.746082] printk: console [ttyS0] enabled
[    0.746580] bcm2835-wdt bcm2835-wdt: Broadcom BCM2835 watchdog timer
[    0.746717] bcm2835-power bcm2835-power: Broadcom BCM2835 power domains driver
[    0.747585] /soc/dsi@7e700000: Fixed dependency cycle(s) with /soc/i2c@7e804000/mipi_dsi@45
[    0.747650] /soc/i2c@7e804000/mipi_dsi@45: Fixed dependency cycle(s) with /soc/dsi@7e700000
[    0.749140] mmc-bcm2835 fe300000.mmcnr: mmc_debug:0 mmc_debug2:0
[    0.749149] mmc-bcm2835 fe300000.mmcnr: DMA channel allocated
[    0.775235] of_cfs_init
[    0.775291] of_cfs_init: OK
[    0.775408] clk: Disabling unused clocks
[    0.814833] mmc0: SDHCI controller on fe340000.mmc [fe340000.mmc] using ADMA
[    0.815061] Waiting for root device /dev/mmcblk0p2...
[    0.855342] mmc1: new high speed SDIO card at address 0001
[    0.876258] mmc0: new DDR MMC card at address 0001
[    0.876805] mmcblk0: mmc0:0001 BJTD4R 29.1 GiB
[    0.877687]  mmcblk0: p1 p2 p3
[    0.878056] mmcblk0: mmc0:0001 BJTD4R 29.1 GiB
[    0.878344] mmcblk0boot0: mmc0:0001 BJTD4R 4.00 MiB
[    0.879002] mmcblk0boot1: mmc0:0001 BJTD4R 4.00 MiB
[    0.879459] mmcblk0rpmb: mmc0:0001 BJTD4R 4.00 MiB, chardev (244:0)
[    0.893593] VFS: Mounted root (squashfs filesystem) readonly on device 179:2.
[    0.894270] devtmpfs: mounted
[    0.895112] Freeing unused kernel memory: 1472K
[    0.895220] Run /sbin/init as init process
[    0.895230]   with arguments:
[    0.895231]     /sbin/init
[    0.895238]   with environment:
[    0.895240]     HOME=/
[    0.895246]     TERM=linux
[    0.895254]     numa_policy=interleave
[    1.052660] F2FS-fs (mmcblk0p3): Mounted with checkpoint version = 2a73989b
[    3.021119] mc: Linux media interface: v0.10
[    3.031148] videodev: Linux video capture interface: v2.00
[    3.042182] vc_sm_cma: module is from the staging directory, the quality is unknown, you have been warned.
[    3.042826] bcm2835_vc_sm_cma_probe: Videocore shared memory driver
[    3.042836] [vc_sm_connected_init]: start
[    3.043212] [vc_sm_connected_init]: installed successfully
[    3.043913] bcm2835_mmal_vchiq: module is from the staging directory, the quality is unknown, you have been warned.
[    3.050630] bcm2835_v4l2: module is from the staging directory, the quality is unknown, you have been warned.
[    3.068761] bcm2835_codec: module is from the staging directory, the quality is unknown, you have been warned.
[    3.071456] bcm2835-codec bcm2835-codec: Device registered as /dev/video10
[    3.071479] bcm2835-codec bcm2835-codec: Loaded V4L2 decode
[    3.073153] bcm2835-codec bcm2835-codec: Device registered as /dev/video11
[    3.073183] bcm2835-codec bcm2835-codec: Loaded V4L2 encode
[    3.077497] bcm2835-codec bcm2835-codec: Device registered as /dev/video12
[    3.077534] bcm2835-codec bcm2835-codec: Loaded V4L2 isp
[    3.079051] bcm2835-codec bcm2835-codec: Device registered as /dev/video18
[    3.079072] bcm2835-codec bcm2835-codec: Loaded V4L2 image_fx
[    3.080840] bcm2835-codec bcm2835-codec: Device registered as /dev/video31
[    3.080876] bcm2835-codec bcm2835-codec: Loaded V4L2 encode_image
[    3.082552] bcm2835_isp: module is from the staging directory, the quality is unknown, you have been warned.
[    3.085459] bcm2835-isp bcm2835-isp: Device node output[0] registered as /dev/video13
[    3.085729] bcm2835-isp bcm2835-isp: Device node capture[0] registered as /dev/video14
[    3.085907] bcm2835-isp bcm2835-isp: Device node capture[1] registered as /dev/video15
[    3.086045] bcm2835-isp bcm2835-isp: Device node stats[2] registered as /dev/video16
[    3.086054] bcm2835-isp bcm2835-isp: Register output node 0 with media controller
[    3.086061] bcm2835-isp bcm2835-isp: Register capture node 1 with media controller
[    3.086066] bcm2835-isp bcm2835-isp: Register capture node 2 with media controller
[    3.086071] bcm2835-isp bcm2835-isp: Register capture node 3 with media controller
[    3.088139] bcm2835-isp bcm2835-isp: Device node output[0] registered as /dev/video20
[    3.088381] bcm2835-isp bcm2835-isp: Device node capture[0] registered as /dev/video21
[    3.088550] bcm2835-isp bcm2835-isp: Device node capture[1] registered as /dev/video22
[    3.088684] bcm2835-isp bcm2835-isp: Device node stats[2] registered as /dev/video23
[    3.088695] bcm2835-isp bcm2835-isp: Register output node 0 with media controller
[    3.088702] bcm2835-isp bcm2835-isp: Register capture node 1 with media controller
[    3.088706] bcm2835-isp bcm2835-isp: Register capture node 2 with media controller
[    3.088711] bcm2835-isp bcm2835-isp: Register capture node 3 with media controller
[    3.088796] bcm2835-isp bcm2835-isp: Loaded V4L2 bcm2835-isp
[    3.091135] snd_bcm2835: module is from the staging directory, the quality is unknown, you have been warned.
[    3.095313] rpi-gpiomem fe200000.gpiomem: window base 0xfe200000 size 0x00001000
[    3.095559] rpi-gpiomem fe200000.gpiomem: initialised 1 regions as /dev/gpiomem
[    3.098524] rtc-pcf8563 1-0051: pcf8563_write_block_data: err=-5 addr=0e, data=03
[    3.098552] rtc-pcf8563 1-0051: pcf8563_probe: write error
[    3.098558] rtc-pcf8563: probe of 1-0051 failed with error -5
[    3.100569] rtc-pcf8563 3-0051: registered as rtc0
[    3.133098] cfg80211: Loading compiled-in X.509 certificates for regulatory database
[    3.161265] Loaded X.509 cert 'benh@debian.org: 577e021cb980e0e820821ba7b54b4961b8b4fadf'
[    3.161743] Loaded X.509 cert 'romain.perier@gmail.com: 3abbc6ec146e09d1b6016ab9d6cf71dd233f0328'
[    3.162128] Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
[    3.162555] Loaded X.509 cert 'wens: 61c038651aabdcf94bd0ac7ff06c7248db18c600'
[    3.181111] brcmfmac: brcmf_fw_alloc_request: using brcm/brcmfmac43455-sdio for chip BCM4345/6
[    3.188791] mcp230xx 1-0038: error -EIO: can't write IOCON 56
[    3.188827] mcp230xx: probe of 1-0038 failed with error -5
[    3.194620] mipi_dsi: Initialize kernel module
[    3.194631] mipi_dsi: (i2c_md_init) Add I2C driver
[    3.194705] mipi_dsi: Probe I2C driver
[    3.194708] mipi_dsi: (i2c_md_probe) Start
[    3.198093] mipi_dsi: (i2c_md_probe) STM32 firmware version 1.9
[    3.199736] mipi_dsi: Add MIPI-DSI device to device tree
[    3.199874] mipi_dsi: (i2c_md_probe) Add panel
[    3.200074] input: seeed-tp as /devices/platform/soc/fe804000.i2c/i2c-1/1-0045/input/input0
[    3.200247] mipi_dsi: (backlight_init) Register backlight device
[    3.200328] mipi_dsi: (backlight_update) brightness=255
[    3.203599] mipi_dsi: (i2c_md_probe) Finish
[    3.204039] mipi_dsi: (i2c_md_init) Register MIPI-DSI driver
[    3.204080] mipi_dsi: Probe MIPI-DSI driver
[    3.205060] vc4-drm gpu: bound fe400000.hvs (ops vc4_drm_unregister [vc4])
[    3.211986] mipi_dsi fe700000.dsi.0: failed to attach dsi to host: -517
[    3.249663] dwc2 fe980000.usb: supply vusb_d not found, using dummy regulator
[    3.249809] dwc2 fe980000.usb: supply vusb_a not found, using dummy regulator
[    3.302648] dwc2 fe980000.usb: DWC OTG Controller
[    3.302689] dwc2 fe980000.usb: new USB bus registered, assigned bus number 1
[    3.302757] dwc2 fe980000.usb: irq 40, io mem 0xfe980000
[    3.302867] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002, bcdDevice= 6.06
[    3.302873] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    3.302877] usb usb1: Product: DWC OTG Controller
[    3.302880] usb usb1: Manufacturer: Linux 6.6.74-rt48-v8 dwc2_hsotg
[    3.302884] usb usb1: SerialNumber: fe980000.usb
[    3.303307] hub 1-0:1.0: USB hub found
[    3.303325] hub 1-0:1.0: 1 port detected
[    3.303665] mipi_dsi: Probe MIPI-DSI driver
[    3.305235] vc4-drm gpu: bound fe400000.hvs (ops vc4_drm_unregister [vc4])
[    3.317275] mipi_dsi fe700000.dsi.0: failed to attach dsi to host: -517
[    3.317477] mipi_dsi: Probe MIPI-DSI driver
[    3.320061] vc4-drm gpu: bound fe400000.hvs (ops vc4_drm_unregister [vc4])
[    3.326441] mipi_dsi fe700000.dsi.0: failed to attach dsi to host: -517
[    3.326681] mipi_dsi: Probe MIPI-DSI driver
[    3.327509] vc4-drm gpu: bound fe400000.hvs (ops vc4_drm_unregister [vc4])
[    3.333328] mipi_dsi fe700000.dsi.0: failed to attach dsi to host: -517
[    3.364360] brcmfmac: brcmf_c_process_txcap_blob: no txcap_blob available (err=-2)
[    3.364897] brcmfmac: brcmf_c_preinit_dcmds: Firmware: BCM4345/6 wl0: Aug 29 2023 01:47:08 version 7.45.265 (28bca26 CY) FWID 01-b677b91b
[    3.367880] [drm] Initialized v3d 1.0.0 20180419 for fec00000.v3d on minor 0
[    3.370759] mipi_dsi: Probe MIPI-DSI driver
[    3.372618] vc4-drm gpu: bound fe400000.hvs (ops vc4_drm_unregister [vc4])
[    3.383181] mipi_dsi fe700000.dsi.0: failed to attach dsi to host: -517
[    3.599966] usb 1-1: new high-speed USB device number 2 using dwc2
[    3.808217] usb 1-1: New USB device found, idVendor=0424, idProduct=2514, bcdDevice= b.b3
[    3.808248] usb 1-1: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[    3.808798] hub 1-1:1.0: USB hub found
[    3.812389] hub 1-1:1.0: 4 ports detected
[    3.813548] mipi_dsi: Probe MIPI-DSI driver
[    3.814404] vc4-drm gpu: bound fe400000.hvs (ops vc4_drm_unregister [vc4])
[    3.848973] input: vc4-hdmi-0 HDMI Jack as /devices/platform/soc/fef00700.hdmi/sound/card0/input1
[    3.849741] vc4-drm gpu: bound fef00700.hdmi (ops vc4_drm_unregister [vc4])
[    3.881408] input: vc4-hdmi-1 HDMI Jack as /devices/platform/soc/fef05700.hdmi/sound/card1/input2
[    3.882679] vc4-drm gpu: bound fef05700.hdmi (ops vc4_drm_unregister [vc4])
[    3.883538] vc4-drm gpu: bound fe700000.dsi (ops vc4_drm_unregister [vc4])
[    3.884314] vc4-drm gpu: bound fe004000.txp (ops vc4_drm_unregister [vc4])
[    3.884849] vc4-drm gpu: bound fe206000.pixelvalve (ops vc4_drm_unregister [vc4])
[    3.885153] vc4-drm gpu: bound fe207000.pixelvalve (ops vc4_drm_unregister [vc4])
[    3.885424] vc4-drm gpu: bound fe20a000.pixelvalve (ops vc4_drm_unregister [vc4])
[    3.885665] vc4-drm gpu: bound fe216000.pixelvalve (ops vc4_drm_unregister [vc4])
[    3.885955] vc4-drm gpu: bound fec12000.pixelvalve (ops vc4_drm_unregister [vc4])
[    3.888829] [drm] Initialized vc4 0.0.0 20140616 for gpu on minor 1
[    3.889253] ------------[ cut here ]------------
[    3.889260] WARNING: CPU: 1 PID: 73 at drivers/gpu/drm/drm_mode_object.c:45 drm_mode_object_add+0x88/0x90 [drm]
[    3.889315] Modules linked in: snd_soc_hdmi_codec brcmfmac_wcc v3d drm_shmem_helper gpu_sched raspberrypi_hwmon dwc2 roles mipi_dsi pinctrl_mcp23s08_i2c pinctrl_mcp23s08 regmap_i2c brcmfmac sha256_generic libsha256 cfg80211 brcmutil rtc_pcf8563 raspberrypi_gpiomem snd_bcm2835(C) bcm2835_isp(C) bcm2835_codec(C) v4l2_mem2mem videobuf2_dma_contig bcm2835_v4l2(C) videobuf2_vmalloc bcm2835_mmal_vchiq(C) vc_sm_cma(C) videobuf2_memops videobuf2_v4l2 videobuf2_common videodev mc vc4 snd_soc_core snd_pcm_dmaengine snd_pcm snd_timer snd drm_display_helper cec drm_dma_helper drm_kms_helper drm drm_panel_orientation_quirks backlight uio_pdrv_genirq uio
[    3.889383] CPU: 1 PID: 73 Comm: kworker/u10:1 Tainted: G         C         6.6.74-rt48-v8 #1
[    3.889388] Hardware name: Raspberry Pi Compute Module 4 Rev 1.0 (DT)
[    3.889392] Workqueue: events_unbound deferred_probe_work_func
[    3.889406] pstate: 60000005 (nZCv daif -PAN -UAO -TCO -DIT -SSBS BTYPE=--)
[    3.889411] pc : drm_mode_object_add+0x88/0x90 [drm]
[    3.889441] lr : drm_property_create+0xcc/0x174 [drm]
[    3.889469] sp : ffffffc08136b5d0
[    3.889471] x29: ffffffc08136b5d0 x28: 0000000000000030 x27: 0000000000001e00
[    3.889478] x26: 0000000000001e00 x25: ffffffc078c92ee8 x24: ffffff80415e2000
[    3.889485] x23: ffffffc078c92fb0 x22: 0000000000000004 x21: 00000000b0b0b0b0
[    3.889491] x20: ffffff8047dafb90 x19: ffffff80415e2000 x18: ffffffc08136b510
[    3.889497] x17: 616265746972572d x16: 31647261632f3164 x15: ffffff8047daf7f0
[    3.889503] x14: ffffff8047daf7d4 x13: 0000000000000000 x12: 0000053c051e050a
[    3.889509] x11: 0000000000000000 x10: 0000000000000500 x9 : 0000000000000000
[    3.889515] x8 : ffffff8047db4380 x7 : 0000000000000000 x6 : ffffffc077035000
[    3.889521] x5 : ffffff8047db4340 x4 : ffffffc08136b590 x3 : 0000000000000000
[    3.889527] x2 : 00000000b0b0b0b0 x1 : ffffff8047dafb90 x0 : 0000000000000001
[    3.889533] Call trace:
[    3.889535]  drm_mode_object_add+0x88/0x90 [drm]
[    3.889564]  drm_property_create+0xcc/0x174 [drm]
[    3.889592]  drm_property_create_enum+0x2c/0x8c [drm]
[    3.889620]  drm_connector_set_panel_orientation+0x8c/0xac [drm]
[    3.889649]  ili9881x_get_modes+0x60/0x7c [mipi_dsi]
[    3.889660]  panel_get_modes+0x28/0x40 [mipi_dsi]
[    3.889668]  drm_panel_get_modes+0x28/0x4c [drm]
[    3.889697]  panel_bridge_connector_get_modes+0x18/0x24 [drm_kms_helper]
[    3.889720]  drm_helper_probe_single_connector_modes+0x198/0x53c [drm_kms_helper]
[    3.889739]  drm_client_modeset_probe+0x1fc/0x1160 [drm]
[    3.889767]  __drm_fb_helper_initial_config_and_unlock+0x54/0x4e0 [drm_kms_helper]
[    3.889785]  drm_fb_helper_initial_config+0x38/0x48 [drm_kms_helper]
[    3.889804]  drm_fbdev_dma_client_hotplug+0x84/0xcc [drm_dma_helper]
[    3.889819]  drm_client_register+0x58/0x9c [drm]
[    3.889847]  drm_fbdev_dma_setup+0x8c/0x134 [drm_dma_helper]
[    3.889860]  vc4_drm_bind+0x2e0/0x3b8 [vc4]
[    3.889883]  try_to_bring_up_aggregate_device+0x168/0x1d4
[    3.889891]  __component_add+0xa8/0x16c
[    3.889897]  component_add+0x14/0x20
[    3.889901]  vc4_dsi_host_attach+0x78/0x13c [vc4]
[    3.889919]  mipi_dsi_attach+0x30/0x54
[    3.889925]  mipi_dsi_probe+0x3c/0x8c [mipi_dsi]
[    3.889934]  mipi_dsi_drv_probe+0x1c/0x28
[    3.889940]  really_probe+0x148/0x2b8
[    3.889946]  __driver_probe_device+0x78/0x12c
[    3.889952]  driver_probe_device+0xd4/0x164
[    3.889957]  __device_attach_driver+0xb8/0x13c
[    3.889963]  bus_for_each_drv+0x88/0xe8
[    3.889968]  __device_attach+0xa0/0x1a0
[    3.889974]  device_initial_probe+0x14/0x20
[    3.889979]  bus_probe_device+0xac/0xb0
[    3.889985]  deferred_probe_work_func+0x88/0xc0
[    3.889990]  process_one_work+0x144/0x29c
[    3.889996]  worker_thread+0x328/0x43c
[    3.890000]  kthread+0x114/0x118
[    3.890006]  ret_from_fork+0x10/0x20
[    3.890012] ---[ end trace 0000000000000000 ]---
[    3.890040] ------------[ cut here ]------------
[    3.890042] WARNING: CPU: 1 PID: 73 at drivers/gpu/drm/drm_mode_object.c:244 drm_object_attach_property+0x6c/0xb0 [drm]
[    3.890074] Modules linked in: snd_soc_hdmi_codec brcmfmac_wcc v3d drm_shmem_helper gpu_sched raspberrypi_hwmon dwc2 roles mipi_dsi pinctrl_mcp23s08_i2c pinctrl_mcp23s08 regmap_i2c brcmfmac sha256_generic libsha256 cfg80211 brcmutil rtc_pcf8563 raspberrypi_gpiomem snd_bcm2835(C) bcm2835_isp(C) bcm2835_codec(C) v4l2_mem2mem videobuf2_dma_contig bcm2835_v4l2(C) videobuf2_vmalloc bcm2835_mmal_vchiq(C) vc_sm_cma(C) videobuf2_memops videobuf2_v4l2 videobuf2_common videodev mc vc4 snd_soc_core snd_pcm_dmaengine snd_pcm snd_timer snd drm_display_helper cec drm_dma_helper drm_kms_helper drm drm_panel_orientation_quirks backlight uio_pdrv_genirq uio
[    3.890137] CPU: 1 PID: 73 Comm: kworker/u10:1 Tainted: G        WC         6.6.74-rt48-v8 #1
[    3.890142] Hardware name: Raspberry Pi Compute Module 4 Rev 1.0 (DT)
[    3.890145] Workqueue: events_unbound deferred_probe_work_func
[    3.890152] pstate: 60000005 (nZCv daif -PAN -UAO -TCO -DIT -SSBS BTYPE=--)
[    3.890157] pc : drm_object_attach_property+0x6c/0xb0 [drm]
[    3.890185] lr : drm_connector_set_panel_orientation+0x60/0xac [drm]
[    3.890213] sp : ffffffc08136b670
[    3.890215] x29: ffffffc08136b670 x28: 0000000000000030 x27: 0000000000001e00
[    3.890222] x26: 0000000000001e00 x25: ffffffc078c92ee8 x24: ffffff80415e2000
[    3.890228] x23: 00000000fffffffd x22: ffffff8047c17d00 x21: ffffff80415e2000
[    3.890234] x20: ffffff8047c17a38 x19: ffffff8047c17970 x18: ffffffc08136b510
[    3.890240] x17: 616265746972572d x16: 31647261632f3164 x15: ffffff8047daf7f0
[    3.890246] x14: ffffff8047daf7d4 x13: 0000000000000000 x12: 0000053c051e050a
[    3.890252] x11: 0000000000000000 x10: 0000000000000500 x9 : 0000000000000001
[    3.890258] x8 : ffffff8047db4e78 x7 : 00000000c0c0c0c0 x6 : 00000000c0c0c0c0
[    3.890264] x5 : 0000000000000000 x4 : 0000000000000001 x3 : 0000000000000006
[    3.890269] x2 : 0000000000000003 x1 : ffffff8047dafb80 x0 : ffffff8047c179b0
[    3.890275] Call trace:
[    3.890277]  drm_object_attach_property+0x6c/0xb0 [drm]
[    3.890305]  ili9881x_get_modes+0x60/0x7c [mipi_dsi]
[    3.890314]  panel_get_modes+0x28/0x40 [mipi_dsi]
[    3.890323]  drm_panel_get_modes+0x28/0x4c [drm]
[    3.890351]  panel_bridge_connector_get_modes+0x18/0x24 [drm_kms_helper]
[    3.890371]  drm_helper_probe_single_connector_modes+0x198/0x53c [drm_kms_helper]
[    3.890389]  drm_client_modeset_probe+0x1fc/0x1160 [drm]
[    3.890417]  __drm_fb_helper_initial_config_and_unlock+0x54/0x4e0 [drm_kms_helper]
[    3.890436]  drm_fb_helper_initial_config+0x38/0x48 [drm_kms_helper]
[    3.890454]  drm_fbdev_dma_client_hotplug+0x84/0xcc [drm_dma_helper]
[    3.890469]  drm_client_register+0x58/0x9c [drm]
[    3.890497]  drm_fbdev_dma_setup+0x8c/0x134 [drm_dma_helper]
[    3.890510]  vc4_drm_bind+0x2e0/0x3b8 [vc4]
[    3.890530]  try_to_bring_up_aggregate_device+0x168/0x1d4
[    3.890539]  __component_add+0xa8/0x16c
[    3.890545]  component_add+0x14/0x20
[    3.890549]  vc4_dsi_host_attach+0x78/0x13c [vc4]
[    3.890568]  mipi_dsi_attach+0x30/0x54
[    3.890573]  mipi_dsi_probe+0x3c/0x8c [mipi_dsi]
[    3.890583]  mipi_dsi_drv_probe+0x1c/0x28
[    3.890588]  really_probe+0x148/0x2b8
[    3.890595]  __driver_probe_device+0x78/0x12c
[    3.890601]  driver_probe_device+0xd4/0x164
[    3.890606]  __device_attach_driver+0xb8/0x13c
[    3.890612]  bus_for_each_drv+0x88/0xe8
[    3.890617]  __device_attach+0xa0/0x1a0
[    3.890622]  device_initial_probe+0x14/0x20
[    3.890628]  bus_probe_device+0xac/0xb0
[    3.890634]  deferred_probe_work_func+0x88/0xc0
[    3.890639]  process_one_work+0x144/0x29c
[    3.890644]  worker_thread+0x328/0x43c
[    3.890648]  kthread+0x114/0x118
[    3.890654]  ret_from_fork+0x10/0x20
[    3.890659] ---[ end trace 0000000000000000 ]---
[    3.918794] mipi_dsi: Prepare panel
[    4.036162] Detected ILI9881D03: ID = 98 81 1a 00
[    4.036191] Initializing ILI9881D03 display...
[    4.162577] brcmfmac: brcmf_cfg80211_set_power_mgmt: power save enabled
[    4.169072] bcmgenet fd580000.ethernet: configuring instance for external RGMII (RX delay)
[    4.169944] bcmgenet fd580000.ethernet eth0: Link is Down
[    4.211983] mipi_dsi: Enable panel
[    4.232449] Console: switching to colour frame buffer device 90x80
[    4.248711] vc4-drm gpu: [drm] fb0: vc4drmfb frame buffer device
[    5.299269] Driver for 1-wire Dallas network protocol.
[    5.362684] wireguard: WireGuard 1.0.0 loaded. See www.wireguard.com for information.
[    5.362747] wireguard: Copyright (C) 2015-2019 Jason A. Donenfeld <Jason@zx2c4.com>. All Rights Reserved.
[    8.260426] bcmgenet fd580000.ethernet eth0: Link is Up - 1Gbps/Full - flow control off
[   15.332203] platform leds: deferred probe pending
[  726.621977] udevd[398]: starting version 3.2.14
[  726.650002] udevd[399]: starting eudev-3.2.14
[  730.025955] cog[454]: memfd_create() called without MFD_EXEC or MFD_NOEXEC_SEAL set
[  730.611959] vc4-drm gpu: swiotlb buffer is full (sz: 2392064 bytes), total 32768 (slots), used 72 (slots)
[  730.949726] vc4-drm gpu: swiotlb buffer is full (sz: 2973696 bytes), total 32768 (slots), used 348 (slots)
[  730.962147] vc4-drm gpu: swiotlb buffer is full (sz: 2392064 bytes), total 32768 (slots), used 72 (slots)
[  730.979263] vc4-drm gpu: swiotlb buffer is full (sz: 2973696 bytes), total 32768 (slots), used 348 (slots)
[  730.995573] vc4-drm gpu: swiotlb buffer is full (sz: 2392064 bytes), total 32768 (slots), used 72 (slots)
[  731.013277] vc4-drm gpu: swiotlb buffer is full (sz: 2973696 bytes), total 32768 (slots), used 348 (slots)
[  731.030508] vc4-drm gpu: swiotlb buffer is full (sz: 2392064 bytes), total 32768 (slots), used 72 (slots)
[  731.046770] vc4-drm gpu: swiotlb buffer is full (sz: 2973696 bytes), total 32768 (slots), used 348 (slots)
[  731.063417] vc4-drm gpu: swiotlb buffer is full (sz: 2392064 bytes), total 32768 (slots), used 72 (slots)
[  731.083961] vc4-drm gpu: swiotlb buffer is full (sz: 2973696 bytes), total 32768 (slots), used 348 (slots)
