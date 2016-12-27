Flashing Firmware
-----------------

Once you have some firmware that you would like to run in a
processor you will need to copy your program into the microprocessor's
FLASH memory bank. This process is known as "flashing" the
processor and unfortunately it is often for different chips. To
give you a head start on figuring out how to flash something for
your chip.

##Memory Images

By default, the makefiles create a `.elf` file when they build. This
file contains the machine code and debugging symbols so that you can
debug your program using `gdb`.

You can create either an Intel HEX file or a simple binary image file
by specifying the make target `hex` or `bin`. For example in the `blink`
directory typing `make bin` will create a file `blink.bin` which is simply
the bytes you need to write into memory. If you create a hex file, then
the file will contain lines that both have the data and the address where
the data is expected to be written.

##OpenOCD

[OpenOCD](https://github.com/ntfreak/openocd) is an open source package
which provides both gdb server support and processor flashing support
for a wide variety of processors.  It is combined with a **user supplied
programmer** tool to use either the JTAG or SWD protocols to program
the flash on your processor's chip.

To operate it requires two things; first is to identify an interface
for it to use, the second is to identify a target. So for example

`openocd -f interface/stlink-v2.cfg -f target/stm32f0x.cfg`

Tells `openocd` to use the ST-Link V2 interface protocol and it will be
programming an `stm32f0x` family chip. A complete programming command
would then be

`openocd -f interface/stlink-v2.cfg -f target/stm32f0x.cfg program foo.elf`

Without telling it to program the board it will run in "server" mode and listen
on port 3333 for gdb to connect to it.

##ST-Link

[ST-Link](https://github.com/texane/stlink) is another program that
is specific to ST controllers and is used to read and write the flash
area. If you have an ST-Link compatible interface then you can use

`st-link write foo.bin 0x80000000`

As a way of writing the binary contents of `foo.bin` into the chip at 0x80000000.

##Black Magic Probe

[Black Magic Probe](https://github.com/blacksphere/blackmagic/) is an
open source package that you can run on either your own hardware or
[purchased hardware](https://1bitsquared.com/collections/all-products/products/black-magic-probe).
It appears to your computer as a serial port and you connect to it with
`gdb`. Using `gdb` to flash your part involves a helper script that is in
the script directory, something like

''`
arm-none-eabi-gdb --batch -ex 'target extended-remote /dev/ttyACM0' \
		-x ../../scripts/black_magic_probe_flash.scr progname.elf
```

