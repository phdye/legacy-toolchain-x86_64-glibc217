# 🛠️ Legacy Toolchain: GCC 5.x + glibc 2.17 for x86_64-pc-linux-gnu (Runtime-Enabled)

This repository provides a cross-build toolchain for targeting **glibc 2.17** on **x86_64** using **GCC 5.x**. It is designed for building and *running* binaries on modern systems **with full runtime support via sysroot redirection**.

---

## 📦 Binary Download

➡️ [Download toolchain tarball](https://github.com/YOUR_USERNAME/legacy-toolchain-x86_64-glibc217/releases/latest)

Contents:
- GCC 5.x cross toolchain
- glibc 2.17 runtime and development headers
- libstdc++, libgcc, and sysroot layout
- Example wrapper script for runtime execution

---

## ✅ Use Case

Build and run legacy-compatible binaries **on the same host**, using glibc 2.17 and older ABI rules — without touching or downgrading the host system’s glibc.

---

## 🧱 Toolchain Specs

| Item               | Value                        |
|--------------------|------------------------------|
| Architecture       | `x86_64`                     |
| Compiler           | `GCC 5.x`                    |
| C library          | `glibc 2.17`                 |
| Sysroot layout     | Fully relocatable            |
| Host Compatibility | Ubuntu ≥ 20.04, WSL2, etc.   |

---

## 🚀 Quickstart

### 1. Extract the toolchain

```bash
mkdir -p $HOME/x-tools
tar -xJf x86_64-pc-linux-gnu-toolchain.tar.xz -C $HOME/x-tools/
export TOOLCHAIN_ROOT=$HOME/x-tools/x86_64-pc-linux-gnu
````

### 2. Build a program

```bash
export PATH="$TOOLCHAIN_ROOT/bin:$PATH"
x86_64-pc-linux-gnu-gcc -O2 -static-libgcc -static-libstdc++ -o hello hello.c
```

---

## 🧪 Run the program (glibc 2.17 runtime)

To run binaries using glibc 2.17 instead of your host glibc:

### Method 1: With LD\_LIBRARY\_PATH

```bash
export GLIBC217_LIB="$TOOLCHAIN_ROOT/x86_64-pc-linux-gnu/lib"
LD_LIBRARY_PATH="$GLIBC217_LIB:$LD_LIBRARY_PATH" ./hello
```

### Method 2: Patchelf rpath override

```bash
patchelf --set-interpreter "$GLIBC217_LIB/ld-2.17.so" --set-rpath "$GLIBC217_LIB" ./hello
./hello
```

---

## 🧰 Wrapper script (recommended)

Create a `run-with-sysroot.sh`:

```bash
#!/bin/bash
TOOLCHAIN="$HOME/x-tools/x86_64-pc-linux-gnu"
export LD_LIBRARY_PATH="$TOOLCHAIN/x86_64-pc-linux-gnu/lib:$LD_LIBRARY_PATH"
exec "$@"
```

Usage:

```bash
chmod +x run-with-sysroot.sh
./run-with-sysroot.sh ./hello
```

---

## 🔧 Cross Configuration

This toolchain was built with **Crosstool-NG** using the config: [.config](./.config)

Key settings:

* `GCC version`: 5.5.0
* `glibc version`: 2.17
* `Binutils`: 2.26
* `Enable C++ support`: yes
* `Enable threads`: yes
* `Sysroot`: enabled and isolated

---

## 📜 License

Toolchain components are covered by:

* GPLv2+/LGPL for GCC, glibc
* FLOSS-compatible licenses

---

## 📬 Feedback

Issues and pull requests welcome!
If you build variants (e.g. musl, static-only, ARM targets), feel free to contribute configs.
