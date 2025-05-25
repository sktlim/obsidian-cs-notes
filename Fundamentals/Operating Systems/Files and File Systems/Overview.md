Long term info storage has 3 requirements:
1) A large amount of info must be able to be stored
2) Info must be **persistent**; it should survive termination of process using it
3) Multiple processes must be able to access info at once

In general, long term storage is implemented in SSDs and hard drives which support disk-like operations of treating data as a linear sequence of fixed-size blocks and supporting read and write operations

Data in storage are abstracted into **files**, logical units of information created by processes

| File System | Key Feature                  | Best Use Case | Limitation                                         |
| ----------- | ---------------------------- | ------------- | -------------------------------------------------- |
| FAT32       | Simplicity, Compatibility    | USB drives    | 4 GB file size limit                               |
| NTFS        | Journaling, Security         | Windows PCs   | Limited cross-platform                             |
| HFS+        | macOS Legacy                 | Legacy macOS  | Performance on SSDs                                |
| APFS        | SSD Optimization             | Modern macOS  | Limited compatibility with non-Apple devices       |
| ext4        | Performance, Stability       | Linux         | Lacks advanced features (snapshots, deduplication) |
| XFS         | Scalability, I/O speed       | Enterprises   | Metadata bottlenecks                               |
| ZFS         | Data Integrity, Snapshots    | Data Centers  | High memory use                                    |
| Btrfs       | Snapshots, CoW               | Linux         | Maturity issues                                    |
| exFAT       | Large Volume Support (128PB) | Flash Drives  | No journaling                                      |
- **Journaling**: mechanism used by file systems to record changes in a log (journal) before applying them to the main file system, ensuring data consistency and facilitating disaster recovery
- **Heavy metadata operations**: Frequent creation and deletion of small files, accessing or modifying file attributes