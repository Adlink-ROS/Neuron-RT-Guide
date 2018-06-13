# Neuron-RT-Guide

Neuron-RT-Guide is a guide line for supporting realtime kernel and network on ADLINK's Neuron board.

*this guide line is heavily under constructed, please use on your risk!*

## Installation Guide

### Build from source

Currently, we use xenomai v3.0.5 as our target, which use linux kernel 4.9.38

* Prepare a working space

```bash
$> cd ~/
$> mkdir xenomai;cd xenomai
```

* Get the linux kernel driver and untar this.

```bash
$> wget https://www.kernel.org/pub/linux/kernel/v4.x/linux-4.9.38.tar.gz
$> tar xvf linux4.9.38.tar.gz
```

* Download xenomai v3.0.5 and untar it.

```bash
$> wget https://xenomai.org/downloads/xenomai/stable/xenomai-3.0.5.tar.bz2
$> tar xf xenomai-3.0.5.tar.bz2
```

* Download the xenomai patch

```bash
$> cd linux-4.9.38
$> wget https://xenomai.org/downloads/ipipe/v4.x/x86/ipipe-core-4.9.38-x86-4.patch
$> ../xenomai-3.0.5/scripts/prepare-kernel.sh --arch=x86_64 --ipipe=ipipe-core-4.9.38-x86-4.patch
```

* configure your kernel

```bash
$> make menuconfig
```

```bash
	Recommended options:
	
	* General setup
	  --> Local version - append to kernel release: -xenomai-3.0.5
	  --> Timers subsystem
	      --> High Resolution Timer Support (Enable)
	* Xenomai/cobalt
	  --> Sizes and static limits
	    --> Number of registry slots (512 --> 4096)
	    --> Size of system heap (Kb) (512 --> 4096)
	    --> Size of private heap (Kb) (64 --> 256)
	    --> Size of shared heap (Kb) (64 --> 256)
	    --> Maximum number of POSIX timers per process (128 --> 512)
	  --> Drivers
	    --> RTnet
	        --> RTnet, TCP/IP socket interface (Enable)
	            --> Drivers
	                --> New intel(R) PRO/1000 PCIe (Enable)
	                --> Realtek 8169 (Enable)
	                --> Loopback (Enable)
	        --> Add-Ons
	            --> Real-Time Capturing Support (Enable)
	* Power management and ACPI options
	  --> CPU Frequency scaling
	      --> CPU Frequency scaling (Disable)
	  --> ACPI (Advanced Configuration and Power Interface) Support
	      --> Processor (Disable)
	  --> CPU Idle
	      --> CPU idle PM support (Disable)
	* Pocessor type and features
	  --> Enable maximum number of SMP processors and NUMA nodes (Disable)
	  // Ref : http://xenomai.org/pipermail/xenomai/2017-September/037718.html
	  --> Processor family
	      --> Core 2/newer Xeon (if "cat /proc/cpuinfo | grep family" returns 6, set as Generic otherwise)
	  // Xenomai will issue a warning about CONFIG_MIGRATION, disable those in this order
	  --> Transparent Hugepage Support (Disable)
	  --> Allow for memory compaction (Disable)
	  --> Contiguous Memory Allocation (Disable)
	  --> Allow for memory compaction
	    --> Page Migration (Disable)
	* Device Drivers
	  --> Staging drivers
	      --> Unisys SPAR driver support
	         --> Unisys visorbus driver (Disable)
```

* Build the xenomai deb package

```bash
$> sudo apt install kernel-package
$> CONCURRENCY_LEVEL=$(nproc) make-kpkg --rootcmd fakeroot --initrd kernel_image kernel_headers
```

* After you complete compiling, you'll get 2 file:`linux-headers-4.9.38-xenomai-3.0.5_4.9.38-xenomai-3.0.5-10.00.Custom_amd64.deb` and `linux-image-4.9.38-xenomai-3.0.5_4.9.38-xenomai-3.0.5-10.00.Custom_amd64.deb`

* Install the xenomai using `dpkg`

```bash
$> dpkg -i linux-headers-4.9.38-xenomai-3.0.5_4.9.38-xenomai-3.0.5-10.00.Custom_amd64.deb linux-image-4.9.38-xenomai-3.0.5_4.9.38-xenomai-3.0.5-10.00.Custom_amd64.deb
```

### Install from binary

If you are using Intel's i7 code, you can download the pre-build xenomai debian package, which is under `bin/i7-6700TE/linux-image-4.9.38-xenomai-3.0.5-i7-6700te_4.9.38-xenomai-3.0.5-i7-6700te-10.00.Custom_amd64.deb`

After download that, instal it using `dpkg -i`

```bash
$> dpkg -i linux-image-4.9.38-xenomai-3.0.5-i7-6700te_4.9.38-xenomai-3.0.5-i7-6700te-10.00.Custom_amd64.deb
```

Before load your new kernel, don't forget update your grub list first.

```bash
$> sudo update-grub
```

After that, reboot your Neuron board and choose Xenomai kernel in your grup list.

## Test your realtime kernel and network

## Write your first application

## Concolusion

