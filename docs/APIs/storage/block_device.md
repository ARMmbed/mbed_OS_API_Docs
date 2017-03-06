# Block Devices

The block device API provides a simple interface for providing access to block-based storage. A block device can be used to back a full [filesystem](filesystem.md) or can be written to
directly.

The full C++ API can be found [here](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/bd/BlockDevice.h)

## Example

Here is a quick example of a simple user of a block device:

[SD block device example](https://github.com/ARMmbed/sd-driver/blob/master/README.md)

## Block device operations

A block device can perform three operations on block in a device:

- Read a block from storage
- Erase a block in storage
- Program a block that has previously been erased

Note: The state of an erased block is undefined. For some block devices, erase is a noop. If a deterministic value is required after an erase, this must be verified by the consumer of the block device.

## Block sizes

Some storage technologies have different sized blocks for different operations. For example, NAND flash can be read and programmed in 256 byte pages, but must be erased in 4Kbyte sectors.

Block devices indicate their block sizes through the three `get_read_size`, `get_program_size`, and `get_erase_size` functions. The erase size must be a multiple of the program size, and the program size must be a multiple of the read size. Some devices may even have a read/program size of a single byte.

As a rule of thumb, the erase size can be used for applications that use a single block size (for example, the FAT filesystem).

## Example block devices

- [SDBlockDevice](https://github.com/armmbed/sd-driver) - Block device for SD cards
- [SPIFBlockDevice](https://github.com/armmbed/spiflash-driver) - Block device for NOR based SPI flash devices
- [I2CEEBlockDevice](https://github.com/armmbed/i2ceeprom-driver) - Block device for EEPROM based I2C devices
- [HeapBlockDevice](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/bd/HeapBlockDevice.h) - Block device backed by the heap for quick testing

