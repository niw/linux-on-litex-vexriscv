OrangeCrab with ENC28J60 on SPI
===============================

A custom device tree source file to use ENC28J60 with OrangeCrab on SPI.

Prerequisites
-------------

This requires multiple patches in various repositories, not only this repository.

- [litex-boards](https://github.com/litex-hub/litex-boards)

  After setup LiteX toolchain, apply [this branch](https://github.com/niw/litex-boards/tree/add_enc28j60_to_orange_crab) to `litex-boards`.

- [linux](https://github.com/torvalds/linux)

  To build Linux image, follow original instruction and use [Buildroot](https://buildroot.org/).
  You might want to use my [buildroot-docker](https://github.com/niw/buildroot-docker) tool.
  Since `buildroot/configs/litex_vexriscv_defconfig` is patched, it will use [this branch](https://github.com/niw/linux/tree/litex-rebase-with-spi-manual-cs-control) that has an extra patch to use ENC28J60.

Usage
-----

```
$ dtc -O dtb -o images/rv32.dtb orangecrab_with_enc28j60_on_spi.dts
```

Ethernet
--------

After booting Linux and login with `root`, it should appear as `eth0`
on `ip link`.
See `dmesg` to confirm there is `enc28j60 spi0.0: Ethernet driver 1.02 loaded`
message.

To link the device up, use `ip` or simply using `udhcpc`.

```
# Manually assign IP address
$ ip addr add 192.168.1.100/24 dev eth0
$ ip link set eth0 up

# Use DHCP
$ udhcpc
```

Pinouts
-------

See [this branch](https://github.com/niw/litex-boards/tree/add_enc28j60_to_orange_crab) for details.

Use [Feathre SPI compatible pins](https://learn.adafruit.com/adafruit-feather/feather-specification#bus-pins-2861844-15)
for each SCK, MOSI, and MISO.

Use GPIO 10 for CS, GPIO 11 for interrupt, which are extended as same as [Adafruit Ethernet FeatherWing Pinouts](https://learn.adafruit.com/adafruit-wiz5500-wiznet-ethernet-featherwing/pinouts).
