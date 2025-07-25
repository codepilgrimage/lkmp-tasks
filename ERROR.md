# Collection of faced errors and the working solution

> ⚠️ **Warning: Common Pitfalls When Building Linux Kernel with Out-of-Tree (`O=...`) Build Directory**
>
> When using `make O=build_dir` to separate your build files from source:
>
> - **Case-Sensitive Path Issues**: Ensure that the `O=...` directory path matches exactly in case with what's referenced in source scripts or configs. On case-insensitive filesystems (e.g., some macOS or Windows mounts), this can cause cryptic errors.
> - **Missing `.config` or inconsistent Kconfig**: Always run `make O=build_dir defconfig` or copy the `.config` into the `build_dir` before starting.
> - **Stale files or partial builds**: If switching between in-tree and out-of-tree builds, clean both source and build dirs (`make mrproper` and remove build_dir) to avoid subtle issues.
> - **Relative path assumptions**: Some scripts expect paths relative to the source tree. Always use absolute paths when possible for `O=...`.
> - **Missing symlinks or headers**: Some Makefiles assume headers or generated files exist in the source dir; make sure the output directory is fully initialized.
>
> 🧠 _Ask me how I know_: I spent almost **2 full days** debugging a case-sensitive directory mismatch. Great learning, but not the most fun I've had!
>
> ✅ Always double-check your command syntax and environment when setting `O=...`:
>
> ```bash
> make O=../builddir x86_64_defconfig
> make O=../builddir -j$(nproc)
> ```
>
> 💡 Tip: Add `V=1` for verbose output if errors seem unrelated to code but may be from path/config issues.


---
## 1. Error in executing the `make mrproper`
### Error 
```shell
    rakuram-lkmp@lkmp:~/kernel/linux-master$ make mrproper
    : not foundink-vmlinux.sh: 27: 
    ./scripts/link-vmlinux.sh: 29: set: Illegal option -
    make[1]: *** [/home/rakuram-lkmp/kernel/linux-master/Makefile:1596: vmlinuxclean] Error 2
    make: *** [Makefile:248: __sub-make] Error 2
```
### Solution
```shell
# You cloned git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git on Windows, then copied the source folder into VirtualBox Ubuntu.
# As a result:
  # Line endings are corrupted: Windows uses CRLF (\r\n), but Linux needs LF (\n)
  # Shell scripts lost +x executable permissions
  # Many scripts (like scripts/link-vmlinux.sh) break with /bin/sh: set: Illegal option -
```

#### 🛠️ Step-by-Step Repair
##### ✅ Step 1: Convert all line endings from CRLF → LF
- Install the conversion tool:
  ```bash
  sudo apt install dos2unix
  ```

- Then go to your kernel source directory:
  ```bash
  cd ~/kernel/linux-master
  ```

- Convert everything:
  ```bash
  find . -type f -exec dos2unix {} \;
  ```
  This removes Windows-style line endings (^M) from all files.

##### ✅ Step 2: Fix missing executable permissions
- Give execution rights to all shell scripts:
  ```bash
  find . -name "*.sh" -exec chmod +x {} \;
  chmod +x scripts/* scripts/kconfig/* -R
  ```

##### ✅ Step 3: Confirm the fix
- Check scripts/link-vmlinux.sh:
  ```bash
  head -n 5 scripts/link-vmlinux.sh
  file scripts/link-vmlinux.sh
  ```
  You should not see anything like CRLF or ASCII text, with CRLF line terminators

##### ✅ Step 4: Now run
  ```bash
  make mrproper
  ```
  This should now succeed without the Illegal option - error.

---

---
## 2. gawk module
### Error - `gawk` module not found
```shell
    /bin/sh: 1: gawk: not found
    make[2]: *** [scripts/Makefile.vmlinux:111: modules.builtin.ranges] Error 127
    make[2]: *** Deleting file 'modules.builtin.ranges'
    make[2]: *** Waiting for unfinished jobs....
    make[1]: *** [/home/rakuram-lkmp/kernel/linux-master/Makefile:1236: vmlinux] Error 2
    make: *** [Makefile:248: __sub-make] Error 2
```

### Solution
- Install the gawk module
    ```shell
    sudo apt update
    sudo apt install gawk
    ```
---

---
## 3. Sharedrive
- Refer to this chat -> [configure VirtualBox Sharedrive](https://chatgpt.com/share/6880c011-1644-800c-a5c7-81adeb3309fe)
---

---
## 4. Certificate Error
### Error Description
```shell
make[3]: *** No rule to make target 'debian/canonical-certs.pem', needed by 'certs/x509_certificate_list'.  Stop.
make[3]: *** Waiting for unfinished jobs....
  CC      certs/system_keyring.o
make[2]: *** [scripts/Makefile.build:555: certs] Error 2
make[2]: *** Waiting for unfinished jobs....
```
### Solution
- Run the below command in terminal
```shell
# Apply fix
scripts/config --disable SYSTEM_REVOCATION_LIST
scripts/config --disable SYSTEM_REVOCATION_KEYS

# Regenerate .config accordingly
make olddefconfig

# Rebuild
make -j$(nproc)
```
---

---
## 5. Different folder for Build & Source - Linux Kernel Build Error Fix

### Error Description

```
make[1]: *** .../Makefile:2003: .] Error 2
make: *** [Makefile:248: __sub-make] Error 2
```

This error is **100% caused by using the same directory for both the source and the build output**, which the Linux kernel explicitly forbids.

### Solution Steps

#### Step 1: Go to your kernel source directory

```bash
cd ~/rakuram-lkmp/linux-kernel/linux-mainline-linus
```

#### Step 2: Create a separate output build directory

```bash
mkdir -p build
```

#### Step 3: Run kernel configuration

```bash
make O=build defconfig
```

You can also use `menuconfig` if you want:

```bash
make O=build menuconfig
```

#### Step 4: Start the actual build

```bash
make O=build -j$(nproc)
```

This tells the kernel to use the `build/` folder for all temporary files and outputs.

### Why This Works

The Linux kernel build system requires a clean separation between source code and build artifacts. Using the `O=build` parameter ensures that:

- Source files remain untouched
- All compiled objects go to the separate build directory
- The build system can properly track dependencies
- You avoid conflicts between source and generated files
---


---
## 5. Out of Tree - folder Structure of Build

### Error

- In the 1st command, I need to specify the folder build to grep the data. If I want to modifiy that means how to do that?
    ```shell
    rakuram-lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ grep CONFIG_SYSTEM_TRUSTED_KEYS build/.config
    CONFIG_SYSTEM_TRUSTED_KEYS="debian/canonical-certs.pem"
    rakuram-lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ scripts/config --disable SYSTEM_REVOCATION_KEYS
    grep: .config: No such file or directory
    ```

### Solution
- ✅ To Modify the .config inside build/, you must do:
    ```bash
    scripts/config --file build/.config --disable SYSTEM_TRUSTED_KEYS
    scripts/config --file build/.config --disable SYSTEM_REVOCATION_KEYS

    # Then regenerate dependencies using below command
    make O=build olddefconfig
    ```
---

---
## 6. Out of Tree - folder Structure of Build - Disable DSCP 

### Error

- In the 1st command, I need to specify the folder build to grep the data. If I want to modifiy that means how to do that?
    ```shell
    make[5]: *** No rule to make target 'net/netfilter/xt_DSCP.o', needed by 'net/netfilter/'.  Stop.
    make[5]: *** Waiting for unfinished jobs....
    make[4]: *** [../scripts/Makefile.build:555: net/netfilter] Error 2
    make[3]: *** [../scripts/Makefile.build:555: net] Error 2
    make[3]: *** Waiting for unfinished jobs....
    ```

### Solution
- Disable the CONFIG_NETFILTER_XT_TARGET_DSCP from `.config`
    ```bash
    rakuram-lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ grep CONFIG_NETFILTER_XT_TARGET_DSCP build/.config
    > CONFIG_NETFILTER_XT_TARGET_DSCP=m
    rakuram-lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ scripts/config --file build/.config --disable CONFIG_NETFILTER_XT_TARGET_DSCP
    rakuram-lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ grep CONFIG_NETFILTER_XT_TARGET_DSCP build/.config
    > # CONFIG_NETFILTER_XT_TARGET_DSCP is not set

    # Then regenerate dependencies using below command
    make O=build olddefconfig
    ```
---

