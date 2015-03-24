# HummingBoard-i2eX vs Raspberry Pi 2

Just some quick benchmarks to compare performance of the [HummingBoard-i2eX](http://www.solid-run.com/product/hummingboard-i2ex/) with the [Raspberry Pi 2](http://www.raspberrypi.org/raspberry-pi-2-on-sale/). Obviously this is not scientific.

Both systems were running a clean install of Debian ([Cubox-i-Debian 2.6](https://github.com/igorpecovnik/Cubox-i-Debian) for the HummingBoard, [Raspbian February 2015](http://www.raspberrypi.org/downloads/) for the RPi) - both updated after installation. Both systems using [SanDisk Ultra 16GB MicroSDHC Class 10 UHS](http://www.amazon.com/dp/B007XZL7PC) flash storage. Tests performed on 21 March 2015.

_Raspberry Pi Model B and Model B+ results were not clean installs using SanDisk Ultra storage and are provided for illustration only._

## /proc/cpuinfo

### HummingBoard-i2eX

    processor   : 0
    model name  : ARMv7 Processor rev 10 (v7l)
    Features    : swp half thumb fastmult vfp edsp neon vfpv3 tls vfpd32 
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant : 0x2
    CPU part    : 0xc09
    CPU revision    : 10

    processor   : 1
    model name  : ARMv7 Processor rev 10 (v7l)
    Features    : swp half thumb fastmult vfp edsp neon vfpv3 tls vfpd32 
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant : 0x2
    CPU part    : 0xc09
    CPU revision    : 10

    Hardware    : Freescale i.MX6 Quad/DualLite (Device Tree)
    Revision    : 0000
    Serial      : 0000000000000000

### Raspberry Pi 2 Model B

    processor  : 0
    model name  : ARMv7 Processor rev 5 (v7l)
    BogoMIPS    : 38.40
    Features    : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm 
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant : 0x0
    CPU part    : 0xc07
    CPU revision    : 5

    processor   : 1
    model name  : ARMv7 Processor rev 5 (v7l)
    BogoMIPS    : 38.40
    Features    : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm 
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant : 0x0
    CPU part    : 0xc07
    CPU revision    : 5

    processor   : 2
    model name  : ARMv7 Processor rev 5 (v7l)
    BogoMIPS    : 38.40
    Features    : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm 
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant : 0x0
    CPU part    : 0xc07
    CPU revision    : 5

    processor   : 3
    model name  : ARMv7 Processor rev 5 (v7l)
    BogoMIPS    : 38.40
    Features    : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm 
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant : 0x0
    CPU part    : 0xc07
    CPU revision    : 5

    Hardware    : BCM2709
    Revision    : a21041
    Serial      : 000000001d5d6a53

## GeekBench 2.4.2 Tryout for Linux ARM (32-bit)

* Raspberry Pi Model B - [250](http://browser.primatelabs.com/geekbench2/view/2543357)
* Raspberry Pi Model B+ - [245](http://browser.primatelabs.com/geekbench2/view/2543358)
* HummingBoard-i2eX - [1099](http://browser.primatelabs.com/geekbench2/2543350)
* Raspberry Pi 2 Model B - [1328](http://browser.primatelabs.com/geekbench2/2543351)

## Build image gallery

Using [sigal](http://sigal.saimon.org/) v. 0.9.2. Catalog consists of 191 JPEG images accounting for about 1 GB on disk.

### HummingBoard-i2eX
INFO: Using 2 cores

External temperature reading: 153° F on the heat sink

	Processed 191 images and 0 videos in 701.15 seconds.

	real	11m41.867s
	user	21m44.547s
	sys		1m18.071s

### Raspberry Pi 2 Model B
INFO: Using 4 cores

External temperature reading: 147° F on the cpu (no heat sink)

	Processed 191 images and 0 videos in 425.96 seconds.

	real	7m6.940s
	user	26m26.450s
	sys		1m18.990s

### Raspberry Pi Model B
INFO: Using 1 cores

External temperature reading: 123° F on the cpu (no heat sink)

	Processed 191 images and 0 videos in 3655.31 seconds.

	real    60m58.182s
	user    58m4.090s
	sys     1m40.770s
