---
title: Extracting Information from the Application Binary
platform: ios
---

You can use radare to get information about the binary, such as the architecture, the list of shared libraries, the list of classes and methods, strings and more.

Let's use the [Damn Vulnerable iOS App DVIA v1](https://github.com/prateek147/DVIA/) as an example. Open its main binary with radare2:

```bash
r2 DamnVulnerableIOSApp
```

## Binary Information

To get information about the binary, you can use the `i` command. This command will list information about the binary, such as the architecture, the list of shared libraries, the list of classes and methods, strings and more.

```bash
[0x1000180c8]> i
...
size     0x43d5f0
humansz  4.2M
mode     r-x
format   mach064
iorw     false
block    0x100
packet   xtr.fatmach0
...
lang     objc with blocks
linenum  false
lsyms    false
nx       false
os       ios
pic      true
relocs   true
sanitize false
static   false
stripped true
```

## Classes and Methods

And then we can proceed to extract information about the methods in the application's source code. To do this, we need to load the application binary into radare and then list the classes and methods in the binary.

```bash
[0x1000180c8]> icc

...

@interface SFAntiPiracy : NSObject
{
}
+ (int) isPirated
+ (int) isJailbroken
+ (void) killApplication
+ (bool) isTheDeviceJailbroken
+ (bool) isTheApplicationCracked
+ (bool) isTheApplicationTamperedWith
+ (int) urlCheck
...
@end
```

Note the plus sign, which means that this is a class method that returns a BOOL type.
A minus sign would mean that this is an instance method. Refer to later sections to understand the practical difference between these.

## Linked Libraries

The following command is listing shared libraries:

```bash
[0x1000180c8]> il
[Linked libraries]
/System/Library/Frameworks/SystemConfiguration.framework/SystemConfiguration
/System/Library/Frameworks/StoreKit.framework/StoreKit
/System/Library/Frameworks/Security.framework/Security
/System/Library/Frameworks/QuartzCore.framework/QuartzCore
/System/Library/Frameworks/MobileCoreServices.framework/MobileCoreServices
/usr/lib/libz.1.dylib
/System/Library/Frameworks/CoreLocation.framework/CoreLocation
/System/Library/Frameworks/CoreGraphics.framework/CoreGraphics
/System/Library/Frameworks/CFNetwork.framework/CFNetwork
/System/Library/Frameworks/AudioToolbox.framework/AudioToolbox
/System/Library/Frameworks/CoreData.framework/CoreData
/System/Library/Frameworks/UIKit.framework/UIKit
/System/Library/Frameworks/Foundation.framework/Foundation
/usr/lib/libobjc.A.dylib
/usr/lib/libSystem.B.dylib
/System/Library/Frameworks/CoreFoundation.framework/CoreFoundation

16 libraries
```

## Strings

Obtaining strings is very useful when reverse engineering an app because it can give you a lot of information about the app's functionality. For example, you can find URLs, API endpoints, encryption keys, and more. You can also find strings that will point you to interesting functions, such as the login function or a function that checks whether the device is jailbroken.

```bash
[0x1000180c8]> izz~cstring | less


29903 0x001d0b4c 0x1001d0b4c 5   6    5.__TEXT.__cstring        ascii   Admin
29904 0x001d0b52 0x1001d0b52 13  14   5.__TEXT.__cstring        ascii   This!sA5Ecret
29905 0x001d0b60 0x1001d0b60 15  16   5.__TEXT.__cstring        ascii   pushSuccessPage
29906 0x001d0b70 0x1001d0b70 4   5    5.__TEXT.__cstring        ascii   Oops
29907 0x001d0b75 0x1001d0b75 30  31   5.__TEXT.__cstring        ascii   Incorrect Username or Password
29908 0x001d0b94 0x1001d0b94 17  18   5.__TEXT.__cstring        ascii   usernameTextField
29909 0x001d0ba6 0x1001d0ba6 39  40   5.__TEXT.__cstring        ascii   T@"UITextField",&,N,V_usernameTextField
29910 0x001d0bce 0x1001d0bce 17  18   5.__TEXT.__cstring        ascii   passwordTextField
...
29915 0x001d0ca8 0x1001d0ca8 18  19   5.__TEXT.__cstring        ascii   http://google.com/
29926 0x001d0d73 0x1001d0d73 37  38   5.__TEXT.__cstring        ascii   Request Sent using pinning, lookout !
29927 0x001d0d99 0x1001d0d99 77  78   5.__TEXT.__cstring        ascii   Certificate validation failed. 
                                                                        You will have to do better than this, my boy!!
```
