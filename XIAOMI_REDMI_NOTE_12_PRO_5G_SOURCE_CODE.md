# Xiaomi Redmi Note 12 Pro 5G - Android Source Code Research

## Device Information

### Model Identification
- **Device Name**: Xiaomi Redmi Note 12 Pro 5G
- **Codename**: `ruby`
- **Model Number (Sweden/Europe)**: `22101316G`
- **European Variant Code**: `MZB0D37EU` (with NFC)
- **Chipset**: MediaTek Dimensity 1080

### Regional Variants
- **Global**: 22101316G, 22101316UG
- **China**: 22101316C, 22101316UCP
- **India**: 22101316I, 22101316UP
- **Europe** (including Sweden): 22101316G

## Official Kernel Source Code

### Primary Official Repository
**Repository**: [MiCode/Xiaomi_Kernel_OpenSource](https://github.com/MiCode/Xiaomi_Kernel_OpenSource)

**Available Branches for Redmi Note 12 Pro**:
- `ruby-s-oss` - Android S (Android 12) kernel source

### Clone Command
```bash
# Clone the entire repository
git clone https://github.com/MiCode/Xiaomi_Kernel_OpenSource.git

# Clone specific ruby-s-oss branch
git clone -b ruby-s-oss https://github.com/MiCode/Xiaomi_Kernel_OpenSource.git
```

### MediaTek Kernel Modules
Since the Redmi Note 12 Pro 5G uses a MediaTek chipset, you'll also need:

**Repository**: [MiCode/MTK_kernel_modules](https://github.com/MiCode/MTK_kernel_modules)

**Branch**: `ruby-s-oss`

```bash
# Clone MediaTek kernel modules
git clone -b ruby-s-oss https://github.com/MiCode/MTK_kernel_modules.git
```

## Community Kernel Sources

### Mayuri-Chan's Repository (Archived)
**Repository**: [Mayuri-Chan/android_kernel_xiaomi_ruby](https://github.com/Mayuri-Chan/android_kernel_xiaomi_ruby)

**Details**:
- **Status**: Archived (Read-only as of May 23, 2025)
- **Primary Branch**: `12.1` (Android 12.1)
- **Language**: C (98.1%), Assembly (1.1%)
- **Commits**: 812,000+
- **Stars**: 7
- **Forks**: 7

**Clone Command**:
```bash
git clone https://github.com/Mayuri-Chan/android_kernel_xiaomi_ruby.git
```

### Coconutat's Experimental Kernel
**Repository**: [Coconutat/android_kernel_xiaomi_ruby_exp](https://github.com/Coconutat/android_kernel_xiaomi_ruby_exp)

**Details**:
- Experimental kernel with KernelSU support
- **Branch**: `MUI14-KernelSU` (for MIUI 14)
- Includes custom modifications and optimizations

**Clone Command**:
```bash
git clone https://github.com/Coconutat/android_kernel_xiaomi_ruby_exp.git
```

## Device Trees (AOSP)

### AOSP Device Tree
**Repository**: [aosp-trees/android_device_xiaomi_ruby](https://github.com/aosp-trees/android_device_xiaomi_ruby)

Used for building AOSP/LineageOS-based ROMs.

```bash
git clone https://github.com/aosp-trees/android_device_xiaomi_ruby.git
```

### crDroid Device Tree
**Repository**: [crdroidandroid/android_device_xiaomi_rubyx](https://github.com/crdroidandroid/android_device_xiaomi_rubyx)

```bash
git clone https://github.com/crdroidandroid/android_device_xiaomi_rubyx.git
```

### PQEnablers Device Tree
**Repository**: [PQEnablers-Devices/android_device_xiaomi_ruby](https://github.com/PQEnablers-Devices/android_device_xiaomi_ruby)

```bash
git clone https://github.com/PQEnablers-Devices/android_device_xiaomi_ruby.git
```

## Building the Kernel

### Prerequisites
- Linux build environment (Ubuntu 20.04+ recommended)
- Android NDK or kernel toolchain
- Build dependencies:
  ```bash
  sudo apt-get install git ccache automake flex lzop bison \
  gperf build-essential zip curl zlib1g-dev g++-multilib \
  libxml2-utils bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev \
  squashfs-tools pngcrush schedtool dpkg-dev liblz4-tool \
  make optipng maven libssl-dev pwgen libswitch-perl \
  policycoreutils minicom libxml-sax-base-perl \
  libxml-simple-perl bc libc6-dev-i386 lib32ncurses5-dev \
  x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev \
  xsltproc unzip device-tree-compiler python2 python3
  ```

### Build Configuration Files
The kernel repositories include multiple configuration files:
- `ruby_defconfig` - Standard configuration
- `ruby_gki_defconfig` - Generic Kernel Image configuration
- Various debug and sanitizer configurations

### Basic Build Steps
```bash
# 1. Clone the kernel source
git clone -b ruby-s-oss https://github.com/MiCode/Xiaomi_Kernel_OpenSource.git kernel

# 2. Navigate to kernel directory
cd kernel

# 3. Set up build environment
export ARCH=arm64
export SUBARCH=arm64
export CROSS_COMPILE=aarch64-linux-android-
export CROSS_COMPILE_ARM32=arm-linux-androideabi-

# 4. Load configuration
make ruby_defconfig

# 5. Build kernel
make -j$(nproc)
```

**Note**: Building the kernel requires a proper Android toolchain. See [Android Kernel Build Documentation](https://source.android.com/docs/setup/build/building-kernels) for detailed instructions.

## GPL Compliance History

### Timeline
- **Late 2022**: Redmi Note 12 Pro 5G launched
- **April 2023**: GPL violation reported - kernel sources not released
- **2023-2024**: Source code eventually released to `ruby-s-oss` branch

### Xiaomi's GPL Policy
Xiaomi aims to release kernel source code within 3 months of device launch, though this timeline has not always been met consistently.

## Additional Resources

### Custom ROMs
- **LineageOS 21**: Available (Android 14)
- **crDroid**: Available
- **HyperOS 2**: Official MIUI/HyperOS updates available

### Firmware
- Firmware downloads available at [mifirm.net/model/ruby.ttt](https://mifirm.net/model/ruby.ttt)
- Stock ROM downloads at [miuirom.org](https://miuirom.org/phones/redmi-note-12-pro)

### Developer Communities
- **XDA Forums**: Active development community
- **Xiaomi.eu**: European Xiaomi community
- **GitHub**: Multiple developers maintaining custom kernels and ROMs

## Sweden-Specific Notes

The Swedish variant (22101316G) is identical to other European/Global variants. There are no Sweden-specific source code differences. The kernel source from the `ruby-s-oss` branch works for all regional variants including:
- European Union markets
- UK
- Global markets (excluding China and India)

## Summary

For building Android for the Xiaomi Redmi Note 12 Pro 5G sold in Sweden, you need:

1. **Kernel Source**: `ruby-s-oss` branch from MiCode/Xiaomi_Kernel_OpenSource
2. **MediaTek Modules**: `ruby-s-oss` branch from MiCode/MTK_kernel_modules
3. **Device Tree**: Any of the community device trees (aosp-trees, crdroidandroid, etc.)
4. **Vendor Blobs**: Extract from stock firmware or use LineageOS vendor tree

All source code is publicly available and free to use under GPL v2 license.

---
**Last Updated**: November 4, 2025
**Research Status**: Complete
