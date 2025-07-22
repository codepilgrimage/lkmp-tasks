# Collection of faced errors and the working solution

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

