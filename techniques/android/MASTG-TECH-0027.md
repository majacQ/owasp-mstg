---
title: Get Open Files
platform: android
---

You can use `lsof` with the flag `-p <pid>` to return the list of open files for the specified process. See the [man page](http://man7.org/linux/man-pages/man8/lsof.8.html "Man Page of lsof") for more options.

```bash
# lsof -p 6233
COMMAND     PID       USER   FD      TYPE             DEVICE  SIZE/OFF       NODE NAME
.foobar.c  6233     u0_a97  cwd       DIR                0,1         0          1 /
.foobar.c  6233     u0_a97  rtd       DIR                0,1         0          1 /
.foobar.c  6233     u0_a97  txt       REG             259,11     23968        399 /system/bin/app_process64
.foobar.c  6233     u0_a97  mem   unknown                                         /dev/ashmem/dalvik-main space (region space) (deleted)
.foobar.c  6233     u0_a97  mem       REG              253,0   2797568    1146914 /data/dalvik-cache/arm64/system@framework@boot.art
.foobar.c  6233     u0_a97  mem       REG              253,0   1081344    1146915 /data/dalvik-cache/arm64/system@framework@boot-core-libart.art
...
```

In the above output, the most relevant fields for us are:

- `NAME`: path of the file.
- `TYPE`: type of the file, for example, file is a directory or a regular file.

This can be extremely useful to spot unusual files when monitoring applications using obfuscation or other anti-reverse engineering techniques, without having to reverse the code. For instance, an application might be performing encryption-decryption of data and storing it in a file temporarily.
