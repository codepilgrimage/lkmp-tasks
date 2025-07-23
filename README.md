# lkmp-tasks
Linux kernel Fall Unpaid 2025 mentorship program

## Important Links to refer
- [LFD103 Course](https://trainingportal.linuxfoundation.org/learn/course/a-beginners-guide-to-linux-kernel-development-lfd103)
- [Mailing List - Archives](https://lore.kernel.org/)
- [Linux Kernel - Mailing List - All Subsystems](https://subspace.kernel.org/vger.kernel.org.html)
- [Kernel GIT Repository](https://git.kernel.org/)
- [The Linux Kernel Archives](https://www.kernel.org/category/releases.html)
- [A guide to the Kernel Development Process](https://www.kernel.org/doc/html/latest/process/development-process.html)
- [Code Of Conduct](https://www.kernel.org/doc/html/latest/process/code-of-conduct.html)
- [Code Of Conduct Interpretation](https://www.kernel.org/doc/html/latest/process/code-of-conduct-interpretation.html)
- [Git Email Configuration](https://git-send-email.io/)
- [Email clients info for Linux](https://www.kernel.org/doc/html/latest/process/email-clients.html)
- [Git-scm - Send-email](https://git-scm.com/docs/git-send-email)

## Important Kernel Documents
- [Document - Kernel Tags](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#using-reported-by-tested-by-reviewed-by-suggested-by-and-fixes)
- [Document - Submitting Patches](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#sign-your-work-the-developer-s-certificate-of-origin)
- [Patch Version History](https://patchwork.kernel.org/patch/11163415/)
- [Minimal requirements to compile the Kernel](https://www.kernel.org/doc/html/latest/process/changes.html)

## Brief Helpful Guide For Starters
- [Kernel Newbies - First Kernel Patch](https://kernelnewbies.org/FirstKernelPatch)
- [ARCHLINUX - Kernel/Traditional compilation](https://wiki.archlinux.org/title/Kernel/Traditional_compilation)
- [ARCHLINUX - mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio)

## Other Online Articles
- [KConfig-Language](https://www.kernel.org/doc/html/v6.1/kbuild/kconfig-language.html#kconfig-language)
- [A Dive into Kbuild By Cao Jin, Fujitsu](https://events19.linuxfoundation.org/wp-content/uploads/2017/11/A-Dive-into-Kbuild-Cao-Jin-Fujitsu.pdf)
- [Exploring the Linux kernel: The secrets of Kconfig/kbuild](https://opensource.com/article/18/10/kbuild-and-kconfig)
- [Kbuild: the Linux Kernel Build System](https://www.linuxjournal.com/content/kbuild-linux-kernel-build-system)


## Misc
- [cregit Tool - Explore the source history](https://github.com/cregit/cregit)
- [The Linux Kernel Archives](https://www.kernel.org/)
- [Kernel Newbies](https://kernelnewbies.org/)

## Kernel Links
- [Linux-Mainline](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/)
- ```shell
  git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux_stable
  cd linux_stable
  git branch -a | grep linux-5
    remotes/origin/linux-5.12.y
    remotes/origin/linux-5.11.y
    remotes/origin/linux-5.10.y

​git checkout linux-5.12.y
```

## Important Books to consider
- Linux Kernel Programming (2nd Edition) by Kaiwan N. Billimoria
- Operating Systems: Three Easy Pieces by Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau

## Important Commands
- `git format-patch -1 --pretty=<Commit-HASH>`

### Patch Components
- [LFD103 - Patches Section](https://trainingportal.linuxfoundation.org/learn/course/a-beginners-guide-to-linux-kernel-development-lfd103/patches/patches?page=2)
- Commit ID
- Commit Header
  - major subsystem: minor area: short description of what is being changed
- Commit Log
- Author
- AuthorDate
- Commit
- CommitDate
- Signed-off-by

### Tags
- [Document - Kernel Tags](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#using-reported-by-tested-by-reviewed-by-suggested-by-and-fixes)
- Acked-by
  - This tag is often used by the maintainer of the affected code when that maintainer neither contributed to nor forwarded the patch. For example, Shuah maintains the usbip driver and she uses the Acked-by tag to ask the USB maintainer to pick patches sent by other developers.
- Reviewed-by
  - This tag indicates that the patch has been reviewed by the person named in the tag.
-  Reported-by
  - This tag gives credit to people who find bugs and report them.
- Tested-by
  - This tag indicates that the patch has been tested by the person named in the tag.
- Suggested-by
  - This tag is used to give credit for the patch idea to the person named in the tag.
- Fixes
  - This tag indicates that the patch fixes an issue in a previous commit referenced by its Commit ID. This tag allows us to track where the bug originated.

### Patch Email Subject Line Conventions
- [PATCH] - used to indicate that the email consists of a patch
- [RFC PATCH] or [PATCH RFC] - indicates the author is requesting comments on the patch. RFC stands for "Request For Comments"
- [PATCH v4] - used to indicate that the patch is the 4th version of this specific change that is being submitted. It is not unusual for a patch to go through a few revisions before it gets accepted

### Email Client Configuration
- Bottom post - Never top post in your response. Top posting is writing a message above the original text while responding to an email. Add your response at the bottom of the original text. Bottom posting makes reading and following review comments on a patch easier.
- Inline post - When reviewing or responding to a patch, deleting or stripping parts of the message you are not replying to is a good practice, and makes it easier to follow the responses in the thread.
- No HTML format - Disable compose messages in HTML format. Patches sent using HTML format will be automatically rejected by the development mailing lists
- No signatures - Do not include private information in your signature. This is important for privacy reasons, as you will be posting to mailing lists.
- No attachments - Do not send patches as attachments. In general, avoid attachments. Some exceptions are kernel logs or configuration files when reporting bugs.


### Install Edge & VS Code
- Refer to this chat -> https://chatgpt.com/share/6880bf94-13f8-800c-907f-d6e237098e43
