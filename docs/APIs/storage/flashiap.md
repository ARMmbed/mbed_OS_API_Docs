# Flash IAP

The flash in application programming provides an interface for access to MCU internal flash memory.

FlashIAP devices have different sized blocks for different operations. They allow to read and program in defined-sized pages, but must be erased in defined-sized sectors. The sector size must be multiple of the page size.

FlashIAP provides the following functionality:

- Read from a flash device
- Program data to flash device pages
- Erase flash device sectors
- Get sector/flash or page sizes
- Get flash start

There are some requirements or limitations for flash devices based on the operation. Please read the documentation for each operation.

This class is thread-safe.

## API

You can find the full C++ API [here](https://github.com/ARMmbed/mbed-os/blob/mbed-os-5.4/drivers/FlashIAP.h).

## Example 

Here is an example that uses FlashIAP driver:

[Bootloader example](https://github.com/ARMmbed/mbed-os-example-bootloader)
