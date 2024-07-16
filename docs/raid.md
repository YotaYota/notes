# RAID

**RAID**: *Reduntant Array of Inexpensive Disks* or *Reduntant Array of Independent Disks*.

Storage virtualization technology. Combines multiple physical disk drive components into one or more logical units.

Provides data redundancy and/or performance improvement.

Data is distributed across drives in one of several ways: RAID levels.

- **RAID 0**: block-level striping, without mirroring or parity. The failure of any drive causes the entire RAID 0 volume and all files to be lost. Reads and writes are done concurrently.
- **RAID 1**: data mirroring, without parity or striping. Write throughput is limited by the slowest drive's write performance.
- **RAID 2**: bit-level striping with dedicated Hamming-code parity. As of 2014 it is not used by any commercially available system.
- **RAID 3**: byte-level striping with dedicated parity. Not commonly used in practice.
- **RAID 4**: block-level striping with dedicated parity. The main advantage of RAID 4 over RAID 2 and 3 is I/O parallelism. One I/O read operation does not have to spread across all data drives.
- **RAID 5**: block-level striping with distributed parity. Parity information is distributed among the drives, requiring all drives but one to be present to operate. Requires at least 3 disks.
- **RAID 6**: block-level striping with double distributed parity. Double parity provides fault tolerance up to two failed drives. Requires at least 4 disks. The larger the drive capacities and the larger the array size, the more important it becomes to choose RAID 6 instead of RAID 5.

**Parity**: evenness or oddness of integer. It ensures that the total number of 1-bits in the string is even or odd. If the sum is odd, parity bit is 1. Parity bit can only detect errors, it cannot correct it.

**Data strinping**: sgmenting logically sequential data so that consequtive segments are stored on different physical storage devices. It is useful if data is requested more quickly than a single device can provide it. It also balances I/O across disks.
