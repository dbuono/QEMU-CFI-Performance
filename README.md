# QEMU-CFI-Performance
Performance Data of QEMU with Control-Flow Integrity enabled

| Test Node  |  | 
| ---- | ----- | 
| Distribution | Ubuntu 18.04 |
| CPU | 2x Intel(R) Xeon(R) CPU E5−2683 v4 |
| Cores | 2x 16 |
| CPU Speed |  2.10GHz |
| Memory | 512GB |
| Host Kernel |  4.15.0−64−generic |

## Check-Acceptance tests

Full VM boot with TCG and KVM, using QEMU's `make acceptance` test

Compilation options, CFI:
```
AR=llvm-ar-9 RANLIB=llvm-ranlib-9 NM=llvm-nm-9 ../qemu/configure --disable-virglrenderer --extra-cflags="-O2" --enable-cfi --cc=clang-9 --cxx=clang++-9
```
Compilation options, Default:
```
../qemu/configure --disable-virglrenderer --extra-cflags="-O2" --cc=clang-9 --cxx=clang++-9
```

| Test | LLVM CFI | LLVM Default | Performance Difference |
| ---- | ----- | ------ | ---- |
| BootLinuxX8664.test_pc_i440fx_tcg | 186.67 s | 186.68 s | 0.01% |
| BootLinuxX8664.test_pc_i440fx_kvm | 29.67 s | 30.49 s | 2.69% |
| BootLinuxX8664.test_pc_q35_tcg | 176.08 s | 186.14 s | 5.40% |
| BootLinuxX8664.test_pc_q35_kvm | 14.88 s | 14.90 s | 0.13% |
| BootLinuxAarch64.test_virt_tcg | 251.60 s| 258.42 s | 2.64% |
| BootLinuxPPC64.test_pseries_tcg | 193.01 s | 195.63 s | 1.34% |
| BootLinuxS390X.test_s390_ccw_virtio_tcg | 341.06 s | 346.33 s | 1.52% |

## Kata Metrics Report

A mix of benchmarks to compare startup times, storage and network performance between containers in independent KATA Virtual Machines.

### Full lifecycle startup time
| llvm-cfi-1 | llvm-cfi-2 | llvm-nocfi-1 | llvm-nocfi-2 |
| - | - | - | - | 
| 2.12s | 2.12s | 2.11s | 2.11s |

### Storage Bandwidth

Read:
| I/O Size | llvm-cfi-1 | llvm-cfi-2 | llvm-nocfi-1 | llvm-nocfi-2 |
| - | - | - | - | - | 
| 4k | 232.5 MB/s | 238.2 MB/s | 232.4 MB/s | 224 MB/s |
| 4k | 256.4 MB/s | 255 MB/s   | 241 MB/s   | 253.1 MB/s |
| 4k | 268.7 MB/s | 255.1 MB/s | 249.1 MB/s | 254.9 MB/s |
| 4k | 262.1 MB/s | 261.3 MB/s | 263.1 MB/s | 247.9 MB/s |
| 4k | 272.4 MB/s | 253 MB/s   | 273.6 MB/s | 271.5 MB/s |

Write:
| I/O Size | llvm-cfi-1 | llvm-cfi-2 | llvm-nocfi-1 | llvm-nocfi-2 |
| - | - | - | - | - | 
| 4k | 229.9 MB/s | 228.2 MB/s | 234.3 MB/s | 234.7 MB/s |
| 4k | 264.7 MB/s | 252.5 MB/s | 238.8 MB/s | 247.6 MB/s |
| 4k | 255.8 MB/s | 258.8 MB/s | 246.3 MB/s | 246 MB/s |
| 4k | 254 MB/s   | 244.9 MB/s | 249.4 MB/s | 250.9 MB/s |
| 4k | 260 MB/s   | 248.2 MB/s | 260.9 MB/s | 252.9 MB/s |

### Network: Instructions per Cycle
| llvm-cfi-1 | llvm-cfi-2 | llvm-nocfi-1 | llvm-nocfi-2 |
| - | - | - | - | 
| 1.37 Ins/Cyc | 1.41 Ins/Cyc | 1.4 Ins/Cyc | 1.38 Ins/Cyc |

Full report [here](KATA_Full_report.pdf)