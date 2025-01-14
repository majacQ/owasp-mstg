---
title: Native Code Tracing
platform: android
---

Native methods tracing can be performed with relative ease compared to Java method tracing. `frida-trace` is a CLI tool for dynamically tracing function calls. It makes tracing native functions trivial and can be very useful for collecting information about an application.

In order to use `frida-trace`, a Frida server should be running on the device. An example for tracing libc's `open` function using `frida-trace` is demonstrated below, where `-U` connects to the USB device and `-i` specifies the function to be included in the trace.

```bash
frida-trace -U -i "open" com.android.chrome
```

<img src="Images/Chapters/0x05c/frida_trace_native_functions.png" width="100%" />

Note how, by default, only the arguments passed to the function are shown, but not the return values. Under the hood, `frida-trace` generates one little JavaScript handler file per matched function in the auto-generated `__handlers__` folder, which Frida then injects into the process. You can edit these files for more advanced usage such as obtaining the return value of the functions, their input parameters, accessing the memory, etc. Check Frida's [JavaScript API](https://www.frida.re/docs/javascript-api/ "JavaScript API") for more details.

In this case, the generated script which traces all calls to the `open` function in `libc.so` is located in `__handlers__/libc.so/open.js`, it looks as follows:

```javascript
{
  onEnter: function (log, args, state) {
    log('open(' +
      'path="' + args[0].readUtf8String() + '"' +
      ', oflag=' + args[1] +
    ')');
  },


  onLeave: function (log, retval, state) {
      log('\t return: ' + retval);      \\ edited
  }
}
```

In the above script, `onEnter` takes care of logging the calls to this function and its two input parameters in the right format. You can edit the `onLeave` event to print the return values as shown above.

> Note that libc is a well-known library, Frida is able to derive the input parameters of its `open` function and automatically log them correctly. But this won't be the case for other libraries or for Android Kotlin/Java code. In that case, you may want to obtain the signatures of the functions you're interested in by referring to Android Developers documentation or by reverse engineer the app first.

Another thing to notice in the output above is that it's colorized. An application can have multiple threads running, and each thread can call the `open` function independently. By using such a color scheme, the output can be easily visually segregated for each thread.

`frida-trace` is a very versatile tool and there are multiple configuration options available such as:

- Including `-I` and excluding `-X` entire modules.
- Tracing all JNI functions in an Android application using `-i "Java_*"` (note the use of a glob `*` to match all possible functions starting with "Java_").
- Tracing functions by address when no function name symbols are available (stripped binaries), e.g. `-a "libjpeg.so!0x4793c"`.

```bash
frida-trace -U -i "Java_*" com.android.chrome
```

Many binaries are stripped and don't have function name symbols available with them. In such cases, a function can be traced using its address as well.

```bash
frida-trace -p 1372 -a "libjpeg.so!0x4793c"
```
