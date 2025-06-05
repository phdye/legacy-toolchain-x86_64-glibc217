# 🛠️ Legacy Cross Toolchain: GCC 5.x + glibc 2.17 for x86_64

This repository provides a prebuilt **Crosstool-NG**-based cross-compilation toolchain targeting **x86_64 with glibc 2.17** and **GCC 5.x**. It is intended for developers needing to build legacy-compatible binaries (e.g. for older Linux distros, containerized environments, or specific legacy APIs).

---

## 📦 Binary Release

👉 [Download the latest toolchain release](https://github.com/YOUR_USERNAME/legacy-toolchain-x86_64-glibc217/releases/latest)

Contents:
- GCC 5.x (cross-compiler)
- glibc 2.17
- Binutils, libstdc++, headers, and sysroot

---

## ✅ Target System

| Property         | Value                            |
|------------------|----------------------------------|
| Architecture      | x86_64                          |
| ABI               | LP64                            |
| libc              | glibc 2.17                      |
| Compiler          | GCC 5.x                         |
| Binutils          | Matching 2.26 or newer          |
| Compatible With   | Ubuntu 14.04–16.04, CentOS 7,   |
|                   | RHEL 7, legacy container images |

---

## 🚀 Quickstart

### 1. Extract

```bash
tar -xvf x86_64-glibc217-toolchain.tar.xz -C $HOME/x-tools/
````

### 2. Setup Environment

```bash
export TOOLCHAIN_ROOT=$HOME/x-tools/x86_64-pc-linux-gnu
export PATH="$TOOLCHAIN_ROOT/bin:$PATH"
export CC="x86_64-pc-linux-gnu-gcc"
export CXX="x86_64-pc-linux-gnu-g++"
```

### 3. Compile a C/C++ Program

```bash
$CC -static-libgcc -static-libstdc++ -O2 -o hello hello.c
```

Or configure an autotools/cmake-based project with:

```bash
./configure --host=x86_64-pc-linux-gnu --prefix=/opt/myapp
```

---

## 🔧 Build Provenance

This toolchain was built using:

* **[Crosstool-NG](https://github.com/crosstool-ng/crosstool-ng)**
* Host system: Ubuntu 20.04 (x86\_64)
* Configuration file: [.config](./.config)

The configuration uses:

* `glibc version`: 2.17
* `gcc version`: 5.x
* `binutils`: compatible
* Optimized for `x86_64` (no `-mcpu` tuning for broad compatibility)

---

## 📁 Directory Structure

```text
x86_64-pc-linux-gnu/
├── bin/                  # Compiler and toolchain binaries
├── lib/                  # glibc runtime and development libs
├── include/              # glibc headers
├── sysroot/              # Target root filesystem
└── share/
```

---

## 🧪 Testing

After setup:

```bash
x86_64-pc-linux-gnu-gcc -v
x86_64-pc-linux-gnu-gcc -static-libgcc -static-libstdc++ -o test test.c
file test
ldd ./test  # should show glibc 2.17 or no dynamic deps if statically linked
```

---

## 🧰 Notes

* Do **not** use `LD_LIBRARY_PATH` to inject glibc 2.17 into host programs.
* This toolchain is for *building* legacy-targeted binaries, not running them on newer systems.

---

## 📜 License

This repository contains only configuration files and build metadata.
Toolchain binaries include components under:

* GPLv2 (gcc, binutils)
* LGPL (glibc)
* Other compatible FLOSS licenses

Consult upstream toolchain sources for license details.

---

## 📬 Feedback & Contributions

Pull requests and issues welcome!
This repo is intended for reproducible, portable toolchain builds for legacy systems.
