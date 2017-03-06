# File System

The filesystem API provides a common interface for implementing a [filesystem](https://en.wikipedia.org/wiki/File_system) on a [block-based storage device](block_device.md). The filesystem API is presented as a class-based interface, but implementing the filesystem API provides the standard POSIX file API familiar to C users.

## Example

Here is what it looks like to use the filesystem in mbed OS:

[FAT filesystem example](https://github.com/armmbed/mbed-os-example-fat-filesystem)

## Declaring a filesystem

The [FileSystem](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h) class provides the core API for filesystem operations. A user must provide a BlockDevice to back the filesystem. When a filesystem is declared with a name, files can be opened on the filesystem through standard POSIX functions (see [open](http://pubs.opengroup.org/onlinepubs/009695399/functions/open.html) or [fopen](http://pubs.opengroup.org/onlinepubs/9699919799/functions/fopen.html).

Existing filesystems:

- [FATFileSystem](https://github.com/ARMmbed/mbed-os/tree/master/features/filesystem/fat) - Standard filesystem that has stood the test of time

The [BlockDevice](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/bd/BlockDevice.h) class provides the underlying API for representing block-based storage that can be used to back a filesystem. mbed OS provides standard interfaces for the more common storage mediums, and the BlockDevice class can be extended to provide support for storage that is not supported.

Existing block devices:

- [SDBlockDevice](https://github.com/armmbed/sd-driver) - Block device for SD cards
- [SPIFBlockDevice](https://github.com/armmbed/spiflash-driver) - Block device for NOR based SPI flash devices
- [I2CEEBlockDevice](https://github.com/armmbed/i2ceeprom-driver) - Block device for EEPROM based I2C devices
- [HeapBlockDevice](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/bd/HeapBlockDevice.h) - Block device backed by the heap for quick testing

Additionally, two utility block devices are provided for better control over how storage is allocated. The [SlicingBlockDevice](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/bd/SlicingBlockDevice.h) allows a user to partition storage into smaller block devices that can be used independentally, and the [ChainingBlockDevice](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/bd/ChainingBlockDevice.h) allows a user to chain multiple block devices together, extending the usable amount of storage.

Note: Some filesystems may provide a format function for cleanly initializing a filesystem on an underlying block device. However, this is optional and a filesystem may require an external tool to set up the filesystem before the first use.

## C++ classes

The [FileSystem](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h) class provides the core user interface, with general functions that map directly to their global POSIX counterparts. mbed OS provides [File](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/File.h) and [Dir](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/Dir.h) classes that represent files and directories in a C++ API that takes advantage of object oriented features in C++.

To implement a new filesystem in mbed OS, an implementor just needs to provide the abstract functions in the FileSystem interface. The [FATFileSystem](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/fat/FATFileSystem.cpp) provides an excellent example, and tests of the POSIX API can be found [here](https://github.com/ARMmbed/sd-driver/tree/master/features/TESTS/filesystem).

Minimal filesystem operations required:

- rename - [POSIX](http://pubs.opengroup.org/onlinepubs/009695399/functions/rename.html), [user/implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L75)
- remove - [POSIX](http://pubs.opengroup.org/onlinepubs/009695399/functions/remove.html), [user/implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L67)
- stat - [POSIX](http://pubs.opengroup.org/onlinepubs/009695399/functions/stat.html), [user/implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L83)
- mkdir - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/mkdir.html), [user/implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L91)

Minimal file operations required:

- file open - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/open.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/File.h#L63), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L105)
- file close - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/close.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/File.h#L69), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L112)
- file read - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/read.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/File.h#L78), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L121)
- file write - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/write.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/File.h#L86), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L130)
- file seek - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/lseek.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/File.h#L109), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L157)

Minimal directory operations required:

- dir open - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/opendir.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/Dir.h#L56), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L186)
- dir close - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/closedir.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/Dir.h#L62), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L193)
- dir read - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/readdir.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/Dir.h#L70), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L201)
- dir seek - [POSIX](http://pubs.opengroup.org/onlinepubs/9699919799/functions/seekdir.html), [user API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/Dir.h#L77), [implementation API](https://github.com/ARMmbed/mbed-os/blob/master/features/filesystem/FileSystem.h#L209)

