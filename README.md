# Neuron-RT-Guide

Neuron-RT-Guide is a guide line for supporting realtime kernel and network on ADLINK's Neuron board.

*this guide line is heavily under constructed, please use on your risk!*

## Installation Guide

### Build from source

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

