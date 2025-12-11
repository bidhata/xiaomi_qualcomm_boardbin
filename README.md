# 📡 bidhatawrt-boardbin-qcn9074

> Custom board-2.bin firmware package for Qualcomm QCN9074 WiFi chipset in OpenWrt

[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
[![OpenWrt](https://img.shields.io/badge/OpenWrt-Compatible-00B5E2?logo=openwrt)](https://openwrt.org/)
[![Platform](https://img.shields.io/badge/Platform-All%20Targets-green)](https://github.com/bidhata/xiaomi_qualcomm_boardbin)

---

## 📖 Overview

This OpenWrt package provides a **vendor-specific board-2.bin firmware file** for the Qualcomm QCN9074 WiFi chipset. It seamlessly replaces the default board-2.bin with a custom version optimized for improved compatibility and performance.

### ✨ Key Features

- 🔧 **Easy Integration** - Simple drop-in replacement for default firmware
- 🚀 **No Compilation Required** - Pure firmware file installation
- 🔄 **Non-Destructive** - Works alongside existing ath11k packages
- 🌐 **Universal Compatibility** - Available for all OpenWrt target platforms

---

## 👤 Maintainer

**Krishnendu Paul**

- 📧 Email: [me@krishnendu.com](mailto:me@krishnendu.com)
- 🌐 Website: [krishnendu.com](https://krishnendu.com)
- 💻 GitHub: [@bidhata](https://github.com/bidhata)
- 📦 Repository: [xiaomi_qualcomm_boardbin](https://github.com/bidhata/xiaomi_qualcomm_boardbin)

---

## 🚀 Quick Start

### Prerequisites

- OpenWrt build environment set up
- Vendor-provided `board-2.bin` file for QCN9074

### Installation Steps

#### 1️⃣ Copy Package to OpenWrt Buildroot

```bash
cp -r bidhatawrt-boardbin-qcn9074 /path/to/openwrt/package/firmware/
```

#### 2️⃣ Add Your Vendor board-2.bin File

> [!IMPORTANT]
> **Critical Step:** Replace the placeholder with your actual vendor-provided firmware file!

```bash
cd /path/to/openwrt/package/firmware/bidhatawrt-boardbin-qcn9074

# Remove placeholder
rm files/lib/firmware/ath11k/QCN9074/hw1.0/board-2.bin

# Copy your vendor file
cp /path/to/your/vendor/board-2.bin files/lib/firmware/ath11k/QCN9074/hw1.0/
```

#### 3️⃣ Configure Package in menuconfig

```bash
cd /path/to/openwrt
make menuconfig
```

**Navigation:**
1. Go to **Firmware** section
2. ✅ **Keep** existing `ath11k-firmware-*` packages selected
3. ✅ **Select** `<*> bidhatawrt-boardbin-qcn9074`
4. Save and exit

#### 4️⃣ Build Firmware

```bash
# Build only this package
make package/bidhatawrt-boardbin-qcn9074/compile V=s

# Or build complete firmware
make -j$(nproc)
```

---

## ⚠️ Important Notes

### 🔴 Do NOT Disable ath11k Packages!

> [!WARNING]
> **Keep the existing ath11k firmware and driver packages selected!**

This package is designed to work **alongside** the standard ath11k packages, not replace them entirely.

**Required packages to keep selected:**
- ✅ `kmod-ath11k-*` (driver packages)
- ✅ `ath11k-firmware-qcn9074` (other firmware files)
- ✅ `bidhatawrt-boardbin-qcn9074` (this package)

**What this package does:**
- ✔️ Replaces **only** the `board-2.bin` file
- ✔️ Keeps all other firmware files intact
- ✔️ Preserves WiFi driver functionality

### 📁 Installation Path

The custom firmware file is installed to:

```
/lib/firmware/ath11k/QCN9074/hw1.0/board-2.bin
```

This will override the default board-2.bin from the standard ath11k-firmware package.

---

## ✅ Verification

After flashing your compiled firmware to the router:

### Check File Installation

```bash
# SSH into your router
ssh root@192.168.1.1

# Verify file exists
ls -lh /lib/firmware/ath11k/QCN9074/hw1.0/board-2.bin

# Check file size/checksum
md5sum /lib/firmware/ath11k/QCN9074/hw1.0/board-2.bin
```

### Verify Firmware Loading

```bash
# Check kernel messages for ath11k
dmesg | grep ath11k

# Expected output should show firmware loading successfully
# Look for: "ath11k ... board-2.bin found"
```

---

## 🔧 Troubleshooting

### 📶 WiFi Not Working After Installation

**Possible causes:**

1. **Missing ath11k packages**
   - ✅ Solution: Ensure you kept `ath11k-firmware-*` and `kmod-ath11k-*` selected

2. **Incompatible board-2.bin**
   - ✅ Solution: Verify your vendor firmware file matches your hardware revision

3. **Firmware loading errors**
   - ✅ Solution: Check `dmesg | grep -i error` for specific error messages

### 📦 Package Not Appearing in menuconfig

**Try these steps:**

```bash
# Update package feeds
./scripts/feeds update -a && ./scripts/feeds install -a

# Refresh package symlinks
make package/symlinks

# Clean and retry
make clean
make menuconfig
```

**Verify package location:**
```bash
ls -la package/firmware/bidhatawrt-boardbin-qcn9074/Makefile
```

### 🛠️ Build Errors

Run with **verbose output** to see detailed error messages:

```bash
make package/bidhatawrt-boardbin-qcn9074/compile V=s
```

**Common issues:**

| Issue | Solution |
|-------|----------|
| `board-2.bin not found` | Ensure you copied your vendor file to `files/lib/firmware/...` |
| `No rule to make target` | Check Makefile syntax and indentation (use tabs, not spaces) |
| `Permission denied` | Verify file permissions: `chmod 644 files/lib/firmware/.../board-2.bin` |

---

## 📂 Package Structure

```
bidhatawrt-boardbin-qcn9074/
├── 📄 Makefile                      # OpenWrt package build instructions
├── 📖 README.md                     # This file
└── 📁 files/
    └── 📁 lib/
        └── 📁 firmware/
            └── 📁 ath11k/
                └── 📁 QCN9074/
                    └── 📁 hw1.0/
                        └── 🔧 board-2.bin    # Your vendor firmware file
```

---

## 📄 License

This package is licensed under **GNU General Public License v2.0**.

See the [LICENSE](LICENSE) file for full details.

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

### How to Contribute

1. 🍴 Fork the repository
2. 🌿 Create a feature branch (`git checkout -b feature/amazing-feature`)
3. 💾 Commit your changes (`git commit -m 'Add amazing feature'`)
4. 📤 Push to the branch (`git push origin feature/amazing-feature`)
5. 🔀 Open a Pull Request

---

## 📞 Support

### Need Help?

- 🐛 **Bug Reports:** [GitHub Issues](https://github.com/bidhata/xiaomi_qualcomm_boardbin/issues)
- 📧 **Email:** [me@krishnendu.com](mailto:me@krishnendu.com)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/bidhata/xiaomi_qualcomm_boardbin/discussions)

---

## 🔗 Related Links

- [OpenWrt Official Website](https://openwrt.org/)
- [OpenWrt Package Development Guide](https://openwrt.org/docs/guide-developer/packages)
- [QCN9074 Chipset Information](https://www.qualcomm.com/)
- [ath11k Driver Documentation](https://wireless.wiki.kernel.org/en/users/drivers/ath11k)

---

<div align="center">

**Made with ❤️ by [Krishnendu Paul](https://krishnendu.com)**

⭐ Star this repository if you find it helpful!

</div>
