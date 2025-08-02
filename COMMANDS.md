### Shell Commands
- `uname -r` - to display the current kernel version
- `touch <filename>` - create a file
- `lsmod` - to list all modules/drivers are loaded
- `grep` - to search through the files
- `ls -h`
- `ls /boot/` - lists all files in `/boot/` folder

### GIT Setup
- git --version
- git config --global user.name <name>
- git config --global user.email <Mail ID>
- git config --list
- git config --global --edit

# GIT Commands for Generating the Formatted Patch
- git format-patch -1 <Commit\ ID>

### Shortcuts
- ctrl+h - to view the hidden files in the folder

### Copy the Kernel Config Files
- cp /boot/config-* kernel/

### Pre-requisite
- Also refer to this chapter [Online Chapter](https://static.packt-cdn.com/downloads/9781803232225_Online_Chapter.pdf)
- ```
  sudo apt-get install build-essential vim git cscope libncurses-dev libssl-dev bison flex gawk mokutil libdwarf-dev libelf-dev libdw-dev
  sudo apt-get install codespell
  ```

### Export the Kernel Source Folder
```bash
$ export LKP_KSRC=~/linux-kernel/linux-mainline-linus
$ cd "$LKP_KSRC"
  # enters the kernel folder
rakuram-lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ 

$ echo "$LKP_KSRC"
/home/rakuram-lkmp/linux-kernel/linux-mainline-linus
```

### Compile the Kernel
- Set up an environment variable to point to the location of the root of our shiny new kernel source tree
  `export LKP_KSRC=~/kernels/linux-6.1.25`
- `make mrproper` - Cleans the config file
- `make olddefconfig` - to generate the kernel configuration based on current configuration and assumes default for new additions
- `make oldconfig` - to generate the kernel configuration based on current configuration
  - Need to run this command inside of the kernel we're going to build
- ```shell
  lsmod > /tmp/my-lsmod
  make LSMOD=/tmp/my-lsmod localmodconfig/
  ```
  - Trim down the kernel and tailor it to your system is by using make localmodconfig. This option creates a configuration file based on the list of modules currently loaded on your system.
- `make -j$(nproc) all` - time to compile the kernel. Using the -j option helps the compiles go faster. The -j option specifies the number of jobs (`make commands`) to run simultaneously
- To measure how long it took to execute, prefix the `time` command:
  `time make -j8 2>&1 | tee out.txt`

#### Compile the Kernel in 'build' directory
```bash
# Disable the certificate
scripts/config --file build/.config --disable SYSTEM_TRUSTED_KEYS
scripts/config --file build/.config --disable SYSTEM_REVOCATION_KEYS
scripts/config --file build/.config --disable SYSTEM_REVOCATION_LIST
scripts/config --file build/.config --disable CONFIG_NETFILTER_XT_TARGET_DSCP # not necessary
# Build Step
make mrproper
make O=build olddefconfig
grep CONFIG_NETFILTER_XT_TARGET_DSCP build/.config
grep CONFIG_SYSTEM_REVOCATION_KEYS build/.config
grep CONFIG_SYSTEM_TRUSTED_KEYS build/.config
make O=build -j4 all 2>&1 | tee out.txt
```

### After Completion of Compilation
- In the root of the kernel source tree, we will now have the following files:
  - The uncompressed kernel image file, vmlinux (used for debugging purposes)
  - The symbol-address mapping file, System.map
  - The compressed bootable kernel image file, bzImage (see the following output)

- Let’s check them out! We make the output (specifically the file size) more human-readable by passing the -h option to ls:
  ```bash
  $ ls -lh vmlinux System.map
  -rw-rw-r-- 1 c2kp c2kp 4.8M May 16 16:12 System.map
  -rwxrwxr-x 1 c2kp c2kp 704M May 16 16:12 vmlinux
  ```
  
  ```bash
  $ file vmlinux
  vmlinux: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically
  linked, BuildID[sha1]=e4<...>, with debug_info, not stripped
  ```

- The actual kernel image file that the bootloader loads up and boots into will always be in the generic location `arch/<arch>/boot/`; hence, for the x86 architecture, we have the following:
  ```bash
  $ ls -lh arch/x86/boot/bzImage
  -rw-rw-r-- 1 c2kp c2kp 12M May 16 16:12 arch/x86/boot/bzImage
  
  $ file arch/x86/boot/bzImage
  arch/x86/boot/bzImage: Linux kernel x86 boot executable bzImage, version
  6.1.25-lkp-kernel (c2kp@osboxes) #3 SMP PREEMPT_DYNAMIC Tue [...], RO-rootFS,
  swap_dev 0XB, Normal VGA
  ```
- So, here, our compressed kernel image version `6.1.25-lkp-kernel` for the x86_64 is approximately 12 MB in size. The file utility again clearly reveals that indeed it is a Linux kernel boot image for the x86 architecture.

### Kernel Booting Process
- ChatGPT Link - https://chatgpt.com/c/68811bdb-d578-800c-927a-4e6e9198383c

#### 🛡️ Safe Flow for Kernel Installation
```bash
# 1. Install modules
sudo make modules_install

# 2. Copy kernel files
sudo cp arch/x86/boot/bzImage /boot/vmlinuz-6.16-rc7
sudo cp System.map /boot/System.map-6.16-rc7
sudo cp .config /boot/config-6.16-rc7

# 3. Generate initramfs
sudo mkinitramfs -o /boot/initrd.img-6.16-rc7 6.16-rc7

# 4. Update boot menu
sudo update-grub

# 5. Reboot and select 6.16-rc7 in GRUB menu
sudo reboot
```

### Set system to boot in console mode (non-GUI) - Helpful for capturing the kernel panics
- Step 1: Boot the system in console mode using below commands
  ```
  sudo systemctl set-default multi-user.target
  sudo reboot
  ```
- Step 2: Login your your credentials
- Step 3: Restore GUI mode as default
  ```
  sudo systemctl set-default graphical.target
  sudo reboot
  ```
- Refer to this chat: https://chatgpt.com/c/688bac3b-29e4-800c-857b-a2126f32c73f



### Important Files 
- Makefile
- MAINTAINERS
- scripts/get_maintainer.pl
- scripts/checkpatch.pl
