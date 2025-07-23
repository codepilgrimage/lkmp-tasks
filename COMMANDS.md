### Shell Commands
- `uname -r` - to display the current kernel version
- touch <filename> - create a file
- ls -h

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

### Compile the Kernel
- `make oldconfig` - to generate the kernel configuration based on current configuration
  - Need to run this command inside of the kernel we're going to build
- ```shell
  lsmod > /tmp/my-lsmod
  make LSMOD=/tmp/my-lsmod localmodconfig/
  ```
  - Trim down the kernel and tailor it to your system is by using make localmodconfig. This option creates a configuration file based on the list of modules currently loaded on your system.
- `make -j3 all` - time to compile the kernel. Using the -j option helps the compiles go faster. The -j option specifies the number of jobs (`make commands`) to run simultaneously

### Important Files 
- Makefile
- MAINTAINERS
- scripts/get_maintainer.pl
- scripts/checkpatch.pl
