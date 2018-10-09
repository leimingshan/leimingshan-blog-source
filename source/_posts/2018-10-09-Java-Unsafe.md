---
title: Java Unsafe的应用与发展
category: Java
tags: Java
abbrlink: 7d6ac8a1
date: 2018-10-09 14:01:29
---
# 历史
sun.misc.Unsafe至少从2004年Java1.4开始就存在于Java中了。
在Java9发布之前，有传闻说Oracle会在Java9正式发布时移除sun.misc.Unsafe，引起了激烈的争论。
因为有不少重要的Java开发库都在底层使用了sum.misc.Unsafe，例如Netty，Neo4J，Spring Framework，Apache Kafka，Apache Storm等。
新的替代API成熟之前，直接移除Unsafe是很冒险的一项做法。

[JEP 260](http://openjdk.java.net/jeps/260)是Java9中一项重要内容，意在封装那些JDK内部使用的API，而不再提供给外部应用使用。
鉴于类似Unsafe这类非常关键而广泛使用的API，目前也没有非常有效的替代方案，暂时得到了保留，因此在JDK9中，我们仍然可以使用Unsafe类，目前没有被内部封装。
在JDK9中jdk.internal.misc中也可以找到Unsafe类。

JDK9中没有被封装的关键内部类有：
- sun.misc.{Signal,SignalHandler}
- sun.misc.Unsafe （许多功能可以通过variable handles实现，后面介绍）
- sun.reflect.ReflectionFactory
- com.sun.nio.file.{ExtendedCopyOption,ExtendedOpenOption, ExtendedWatchEventModifier,SensitivityWatchEventModifier}

这些类在以后的版本中可能被移除或封装。而非关键类例如sun.misc.BASE64Encoder和sun.misc.BASE64Decoder则直接被移除了。
<!--more-->

# 说明
Unsafe类名的意思就是“不安全的”，警告开发者们一定要慎重使用该类。

我们知道，Java语言是不能直接访问底层硬件的，但为了解决某些问题，还不得不访问底层硬件，Java提供了JNI技术（JNI是Java Native Interface的缩写，它提供了若干的API实现了Java和其他语言的通信）使我们可以通过C、C++这类语言去访问底层硬件。

Unsafe类通过JNI封装了一些较为底层的方法，但就如它的类名表达的含义一样，警告使用者使用它里面的方法是不安全的、是很危险的。使用Unsafe直接操作内存，是绕过了JVM的内存分配机制的，需要自己手动分配回收内存（熟悉C语言的同学一定很熟悉），以及内存屏障（store load barrier）和CAS这些操作，都是非常危险的操作。所以默认情况下，Unsafe只提供给可信任代码（被BootstrapClassLoader加载的类，也就是说只能被rt.jar包里面的类）使用。

# 应用
开发者自己编写的类是由AppClassLoader加载的，如果尝试调用Unsafe.getUnsafe()来获得Unsafe的实例的话，你会遇到一个SecurityException的异常，因为前面提到，只有可信任代码（例如JDK中的Atomic相关类）才能这样直接获取Unsafe实例。
JDK8之前只能通过反射获取实例：
```
Field f = Unsafe.class.getDeclaredField("theUnsafe");
f.setAccessible(true);
Unsafe unsafe = (Unsafe) f.get(null);
```

JDK9中其实已经没有了上述限制，参考Unsafe源码对比。
```
package jdk.internal.misc;

import jdk.internal.HotSpotIntrinsicCandidate;
import jdk.internal.vm.annotation.ForceInline;

import java.lang.reflect.Field;
import java.security.ProtectionDomain;

/**
 */
public final class Unsafe {

    private static native void registerNatives();
    static {
        registerNatives();
    }

    private Unsafe() {}

    private static final Unsafe theUnsafe = new Unsafe();

    /**
     * Provides the caller with the capability of performing unsafe
     * operations.
     *
     * <p>The returned {@code Unsafe} object should be carefully guarded
     * by the caller, since it can be used to read and write data at arbitrary
     * memory addresses.  It must never be passed to untrusted code.
     *
     * <p>Most methods in this class are very low-level, and correspond to a
     * small number of hardware instructions (on typical machines).  Compilers
     * are encouraged to optimize these methods accordingly.
     *
     * <p>Here is a suggested idiom for using unsafe operations:
     *
     * <pre> {@code
     * class MyTrustedClass {
     *   private static final Unsafe unsafe = Unsafe.getUnsafe();
     *   ...
     *   private long myCountAddress = ...;
     *   public int getCount() { return unsafe.getByte(myCountAddress); }
     * }}</pre>
     *
     * (It may assist compilers to make the local variable {@code final}.)
     */
    public static Unsafe getUnsafe() {
        return theUnsafe;
    }
```

JDK1.9之前的sun.misc.Unsafe
```
package sun.misc;

import jdk.internal.vm.annotation.ForceInline;
import jdk.internal.misc.VM;
import jdk.internal.ref.Cleaner;
import jdk.internal.reflect.CallerSensitive;
import jdk.internal.reflect.Reflection;
import sun.nio.ch.DirectBuffer;

import java.lang.reflect.Field;


/**
 */

public final class Unsafe {

    static {
        Reflection.registerMethodsToFilter(Unsafe.class, "getUnsafe");
    }

    private Unsafe() {}

    private static final Unsafe theUnsafe = new Unsafe();
    private static final jdk.internal.misc.Unsafe theInternalUnsafe = jdk.internal.misc.Unsafe.getUnsafe();

    /**
     * Provides the caller with the capability of performing unsafe
     * operations.
     *
     * <p>The returned {@code Unsafe} object should be carefully guarded
     * by the caller, since it can be used to read and write data at arbitrary
     * memory addresses.  It must never be passed to untrusted code.
     *
     * <p>Most methods in this class are very low-level, and correspond to a
     * small number of hardware instructions (on typical machines).  Compilers
     * are encouraged to optimize these methods accordingly.
     *
     * <p>Here is a suggested idiom for using unsafe operations:
     *
     * <pre> {@code
     * class MyTrustedClass {
     *   private static final Unsafe unsafe = Unsafe.getUnsafe();
     *   ...
     *   private long myCountAddress = ...;
     *   public int getCount() { return unsafe.getByte(myCountAddress); }
     * }}</pre>
     *
     * (It may assist compilers to make the local variable {@code final}.)
     *
     * @throws  SecurityException if the class loader of the caller
     *          class is not in the system domain in which all permissions
     *          are granted.
     */
    @CallerSensitive
    public static Unsafe getUnsafe() {
        Class<?> caller = Reflection.getCallerClass();
        if (!VM.isSystemDomainLoader(caller.getClassLoader()))
            throw new SecurityException("Unsafe");
        return theUnsafe;
    }
```

# 未来的替代者Variable Handles
https://www.voxxed.com/2016/11/java-9-series-variable-handles/
java.lang.invoke.VarHandle


# 参考文档
https://www.jianshu.com/p/54cc20a87502
https://blog.csdn.net/luzheqi/article/details/79097682
http://ifeve.com/java-9-sun-misc-unsafe/comment-page-1/
https://www.zybuluo.com/kiraSally/note/867462
https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-F7696E02-A1FB-4D5A-B1F2-89E7007D4096