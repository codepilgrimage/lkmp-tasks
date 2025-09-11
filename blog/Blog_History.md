### Watched Mentorship Sessions
1. [Mentorship Session: Dynamic Program Analysis for Fun and Profit](https://www.youtube.com/watch?v=ufcyOkgFZ2Q)
  - Date: 09-Sep-2025
  - [Slides](https://events.linuxfoundation.org/wp-content/uploads/2022/10/Dmitry-Vyukov-Dynamic-program-analysis_-LF-Mentorship.pdf?ajs_aid=f47394ae-0144-4f51-b526-b29581694089)
2. [Mentorship Session: Kernel Validation With Kselftest](https://www.youtube.com/watch?v=mpO_iDEMqWQ)
  - Date: 09-Sep-2025
  - [Slides](https://events.linuxfoundation.org/wp-content/uploads/2022/10/Shuah-Khan-Kernel-Validation-With-Kselftest.pdf?ajs_aid=f47394ae-0144-4f51-b526-b29581694089)
3. [Mentorship Session: New Ideas for Smatch (Static Analysis)](https://www.youtube.com/watch?v=zZGvKcPYhe0)
  - Date: 11-Sep-2025
  - [Slides](https://events.linuxfoundation.org/wp-content/uploads/2022/10/Dan-Carpenter-Smatch-Mentorship-Series-Presentation-Template.pdf?ajs_aid=b526b18b-c670-4f0d-9269-45cb9edd572d)
4. []()
  - Date:
  - [Slides]()
5. []()
  - Date:
  - [Slides]()

### Smatch Usage
- ```
  # to build the database
  lkmp@lkmp:~/linux-kernel/linux-mainline-linus$ ../smatch/smatch_scripts/build_kernel_data.sh 
  
  Usage:  /usr/local/bin/kchecker [--sparse][--valgrind][--debug] path/to/file.c
  rakuram-lkmp@lkmp:~/linux-kernel/smatch$ /usr/local/bin/kchecker ../linux-mainline-linus/tools/testing/selftests/net/can/test_raw_filter.c
```

### Analysed Syzkaller Issues List
- 

### Supplement Resources
- [KASAN - Kernel Address Sanitizer](https://naveenaidu.dev/kasan-kernel-address-sanitizer)
- [KernelAddressSanitizer (KASan) - LinuxCon North America 2015](https://events.static.linuxfound.org/sites/events/files/slides/LinuxCon%20North%20America%202015%20KernelAddressSanitizer.pdf)
- [Kernel Documentation - KASAN](https://www.kernel.org/doc/html/latest/dev-tools/kasan.html)
