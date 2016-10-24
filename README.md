# NuttX on STM32F4

> Test on STM32F4 Discovery Board (STM32F407VG)

## Preparation

- `arm-none-eabi-` toolchain
- git
- build-essential flex bison texinfo gettext gperf
- libusb-1.0.0 libncurses5-dev minicom

## Build Steps

1. Init

   ```
   git clone https://github.com/robbie-cao/nuttx-stm32f4.git
   cd nuttx-stm32f4
   git submodule init
   git submodule update --recursive
   ```

2. Build `stlink`

   ```
   cd stlink
   make
   ```

   Refer to https://github.com/texane/stlink

3. Build `kconfig-frontends`

   ```
   cd tools/kconfig-frontends
   ./configure --enable-mconf
   -or-
   ./configure --disable-shared --enable-static --enable-mconf
   make
   sudo make install
   sudo ldconfig
   ```

4. Build nuttx

   ```
   cd nuttx/tools

   # this step will not provide any output, but copy the configuration to
   # nuttx-git/nuttx/.config

   ./configure.sh stm32f4discovery/nsh    # nsh console/UART2 - need UART-TTL to USB cable eg FTDI
   # -or-
   ./configure.sh stm32f4discovery/usbnsh # nsh console/usb - need microUSB to USB cable

   # and return to the nuttx directory
   cd ..

   make menuconfig
   -> Select "Build Setup -> Build Host Platform -> Linux"
   make
   ```

5. Flashing

   ```
   # chdir to nuttx-stm32f4 root
   cd /path/to/nuttx-stm32f4
   export PATH=$PATH:`pwd`/stlink
   st-flash write nuttx/nuttx.bin 0x8000000 (add sudo if there's permission issue)
   ```

## Reference

- http://nuttx.org/doku.php?id=wiki:howtos:stm32f4discovery_unix
- https://github.com/robbie-cao/note/blob/master/nuttx-stm32.md
- https://github.com/Samsung/iotjs/wiki/Build-for-NuttX
- https://github.com/Samsung/iotjs/blob/master/targets/nuttx-stm32f4/README.md
