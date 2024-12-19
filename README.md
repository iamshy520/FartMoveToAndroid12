这里是一个将fart移植到android-12.0.0_r3的源码备份

# 系统优化目标
1、应用层隐藏root,adb可以使用root
2、集成charles证书到系统
3、信任用户证书
4、fart移植
5、frida检测(粗略)
6、隐藏系统为调试系统

# 涉及到的源码文件如下
//fart
art/dex2oat/dex2oat.cc 
art/runtime/art_method.cc 
art/runtime/interpreter/interpreter.cc 
art/runtime/native/dalvik_system_DexFile.cc 
art/runtime/native/java_lang_reflect_Method.cc 
art/runtime/oat_file_assistant.cc 
frameworks/base/core/java/android/app/ActivityThread.java 
frameworks/base/services/core/java/com/android/server/am/ActivityManagerShellCommand.java 
frameworks/base/services/core/java/com/android/server/pm/PackageDexOptimizer.java 
libcore/dalvik/src/main/java/dalvik/system/DexFile.java 
frameworks/native/cmds/installd/run_dex2oat.cpp



//信任用户证书//修改代理特征
frameworks/base/core/java/android/security/net/config/NetworkSecurityConfig.java
frameworks/base/core/java/android/security/net/config/XmlConfigSource.java
libcore/ojluni/src/main/java/java/lang/System.java
libcore/ojluni/src/main/java/java/net/NetworkInterface.java

//隐藏Bootloader锁
/bionic/libc/bionic/system_property_api.cpp


//root指纹抹除
https://mp.weixin.qq.com/s?__biz=MzI3MDQ1NDE2OA==&mid=2247484684&idx=1&sn=c5f0c9b525e820d198a4da3a7256bab9&chksm=ead199fbdda610ed1e26fc63bf80456c4b4646349e5d44206335d8ba38aeccc95b815d484a60&token=1324308020&lang=zh_CN#rd
build/make/tools/buildinfo.sh
build/make/core/sysprop.mk  //隐藏特征主要是修改这个文件

ROOT 检测通杀-改造优化篇
https://mp.weixin.qq.com/s/80cF7Is6yeaZrE-dyQNUiA
build/make/target/product/base_system.mk

备注：BUILD_ID位于build/make/core/build_id.mk
aosp_crosshatch-user 12.0.0 SP1A.210812.016.A1 eng.yougae.20240902.122508 release-keys

//root定制
system/extras/su/Android.mk
system/core/libcutils/fs_config.cpp
system/sepolicy/private/file_contexts
build/make/target/product/security/Android.mk
system/extra/su



//adb root
system/core/rootdir/init.rc

system/core/adb/daemon/main.cpp
12转移到了
packages/modules/adb/daemon/main.cpp

system/core/adb/adb.c
12转移到了

//证书集成
在此目录放入自己的证书即可，会自动编译至系统 证书的命名规则:
/system/ca-certificates/files




//codeitem
art/libdexfile/dex/dex_file_structs.h
