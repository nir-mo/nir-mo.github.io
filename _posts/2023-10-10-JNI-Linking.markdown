---
layout: post
title: "Linking Tips and Insights for Android Native Libraries"
date:   2023-10-10 14:07:00 +0300
categories: Android
tags: android jni linking lightricks
image: /assets/images/android_in_library.jpeg
---

![Android mascot standing in a library]({{ page.image }})

## Introduction
Reflecting on our journey at [Lightricks](https://www.lightricks.com/), it's clear that the world of [Android development with native (C++) libraries](https://developer.android.com/ndk/guides/concepts) presents a multitude of complexity that demand careful consideration. 

When we initially ventured into this domain, linking considerations were, admittedly, not at the forefront of our minds. We found ourselves in possession of several C++ libraries, vital components shared among various [Lightricks](https://www.lightricks.com/) applications. 
Some were shared via [shared objects](https://en.wikipedia.org/wiki/Shared_library) (`*.so`), while others relied on [static archives](https://en.wikipedia.org/wiki/Static_library) (`*.a`). 
Moreover, our libraries had different levels of complexity. Some had Java interfaces, while others were only in C++.
As our applications size grew, the need to optimize binary sizes and understanding the different linking choices became apparent. 

In this blog post, I share my insights and the knowledge I gained while discovering the right architectures and considerations for harnessing the full potential of Android native (C++) libraries, while providing essential guidelines, especially for applications featuring multiple native packages.
As we delve into the world of Android development with native libraries, we assume that you, the reader, already have a foundation in Android app development and a basic understanding of compilation processes.

## Exploring the World of Native Libraries
Before we delve into the intricacies of different linking types and considerations in Android development, let's begin with a brief overview of native libraries significance in this context.

Native libraries serve as external building blocks, enriching the capabilities of Android applications. These libraries are typically developed in C/C++ or other native languages, introducing an additional layer of functionality to Android apps. While Java and Kotlin are often sufficient for app development, there are scenarios where these languages alone may not meet the application's requirements.

Consider the following situations where native libraries come into play:
1. Integrating an existing library written in another language to make it accessible within your application—this is particularly common for legacy code or shared libraries used across different projects and platforms.
2. Implementing time-critical code, such as low-level operations in assembly, to enhance performance.

Incorporating native libraries into your app usually doesn't demand advanced engineering skills. However, when dealing with multiple native libraries that interact with each other, a deeper understanding becomes essential to optimize the application according to your specific needs. At [Lightricks](https://www.lightricks.com/), we work with several native libraries, ranging from pure C++ libraries that serve as infrastructure for other internal native libraries to those with [JNI](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/spec/jniTOC.html) functions (which we'll explain later). Additionally, we integrate third-party libraries with prebuilt binaries. This diverse mix brings benefits but also entails considerations, including compilation time and [APK](https://en.wikipedia.org/wiki/Apk_(file_format))/AAB size, which we'll explore further.

In the upcoming sections, we will discuss the nuances of archive files and shared objects, shedding light on their roles in Android development and the critical considerations surrounding them.

## Archive Files and Shared Objects - An Overview
Understanding the underpinnings of native libraries requires delving into the world of archive files (`*.a`) and shared objects (`*.so`). These file formats serve as the foundation upon which native libraries are built. 

Archive files, denoted by the `*.a` extension, are essentially collections of [object files](https://en.wikipedia.org/wiki/Object_file) (`*.o`) bundled together. They excel in facilitating [static linking](https://en.wikipedia.org/wiki/Static_library), resulting in executables with self-contained code. Static linking means that all the code necessary to run the program is included within the executable itself. While static linking provides performance advantages and allows for certain linker optimizations, it may lead to larger executables due to the inclusion of the entire library during compilation. However, it's essential to note that static archives are an integral part of the compilation process, and they, too, can benefit from linker optimizations such as [dead code elimination](https://en.wikipedia.org/wiki/Dead-code_elimination) and [Link-Time Optimization (LTO)](https://gcc.gnu.org/wiki/LinkTimeOptimization).

On the other hand, shared objects, marked by the `*.so` extension, are [dynamic libraries](https://en.wikipedia.org/wiki/Dynamic-link_library) that export functions. They champion dynamic linking, which trims down executable sizes, promoting efficient memory use. [Dynamic linking](https://en.wikipedia.org/wiki/Dynamic-link_library) means that the code from the shared object is loaded into memory only when the program needs it. This not only reduces executable size but also allows for efficient memory usage. The beauty of shared objects lies in their replaceability. If you need to update a specific library, you can simply swap out the corresponding `*.so` file without having to update the entire application. This level of modularity ensures that updates are precise and efficient. 

It's worth noting that while shared object files are generally considered smaller, and your application's shared object may indeed be smaller (because it is split among several files), __in Android, we calculate the APK size as a whole__. In the following sections, we will delve into the details of two fundamental linking techniques: static and dynamic linking, shedding light on their roles in Android development and the critical considerations surrounding them.

## Android Development Considerations
![Android mascot asking questions](/assets/2023-10-10-JNI-Linking/android_ask_questions.jpeg)

Android app development presents its unique set of challenges and considerations, distinguishing it from other frameworks. Among these considerations are:

### The Role of JNI
Every Android app built in Java/Kotlin can tap into the power of C++ through the [Java Native Interface (JNI)](https://en.wikipedia.org/wiki/Java_Native_Interface). This standard defines the bridge between the Java Virtual Machine (JVM) and native code. It covers everything from defining Java functions that map to C/C++ implementations to loading native code into an app's memory. In the Android world, loading native libraries requires a special method known as [System.LoadLibrary](https://developer.android.com/reference/java/lang/System#loadLibrary(java.lang.String)). This method expects a shared object file using the JNI naming convention. Shared objects are the way to go when you have native libraries with implementations for Java functions. __In practical terms, this means your app will always include at least one shared object file that loads dynamically during runtime.__

### Replaceability?
One compelling aspect of using shared libraries is modularity. The promise is enticing: when you need to update a specific native library, just swap out the corresponding `*.so` file with a new version, right? Well, here's where Android adds a twist. In Android, this degree of modularity isn't a reality. If you wish to update a single `*.so` file, you'll find yourself needing to update the entire application. It's a quirk worth noting as it impacts how you manage native code updates.

### APK Size and ABI Replicas
Adding to these considerations is the matter of APK size. Android applications often need to support multiple [ABIs](https://developer.android.com/ndk/guides/abis) to ensure compatibility with various devices. This means that each native library included in your app will have several replicas—one for each ABI your app targets. Consequently, as the number of supported ABIs increases, so does the size of your APK. It's a crucial aspect to keep in mind, especially if you're aiming for an optimized application. 

## Architectural Principles: Static Linking vs. Shared Objects

Now, let's talk about architectural choices when integrating native libraries. Here's what not to do:

### Don't Mix Linking Types
Imagine you have several shared object files (let's call them `libA.so`, `libB.so`, etc.), and all of them rely on common functions from a different library (`libcore.a`) linked statically. In this scenario, each shared object file carries a redundant copy of `libcore.a`. It's an unnecessary waste of space.

![bad design: libcore.a is duplicated in every library](/assets/2023-10-10-JNI-Linking/bad_mix_dup_core.jpg)

The same principle applies in reverse. Suppose you have a large shared object file (we'll call it `liblarge.so`) used across multiple statically linked libraries (`libA.a`, `libB.a`, etc.). In that case, your APK will contain the entire `liblarge.so`, even if you're using just a single function from it. The linker won't optimize it away.

![bad design: liblarge.so is included in the APK which is redundant](/assets/2023-10-10-JNI-Linking/bad_mix_big_so.jpg)

### Static Linking with One Shared Library
This architectural approach shines when you have multiple __native libraries that don't export JNI functions__. In this setup, you'll have a single shared object file, loaded by the app, housing all relevant functions from other libraries. Irrelevant functions simply get dropped during compilation.

![All libraries are statically linked against libApp.so](/assets/2023-10-10-JNI-Linking/good_all_static.jpg)

The drawbacks include longer compilation times—each change in any native library triggers a full rebuild. There's also the issue of naming collisions; two functions can't share the same name, leading to ambiguity and compilation errors. Plus, __once you introduce a single JNI function, [it's at risk of being stripped away as "dead code" by the compiler](#archive-files-and-shared-objects---an-overview)__. There are ways to handle this, such as the [`--whole-archive` flag](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_node/ld_3.html), but it sacrifices compiler optimizations. Alternatively, you can use the [-u](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_node/ld_3.html) flag to retain a specific function, but this reveals implementation details to your CMAKE/Makefile, which can be fragile, especially when libraries tend to change.

### Using Only Shared Objects
Opting for shared objects eliminates code duplication and ensures the compiler won't prematurely prune your code. However, there's a trade-off. If you have a large shared object file but only use a small portion of it, your app still carries the entire file, missing out on compiler optimizations.

![Using only shared libraries](/assets/2023-10-10-JNI-Linking/good_all_dynamic.jpg)

## Guidelines for Choosing the Right Linkage Type

Selecting the right architecture can be a challenge, varying based on use cases and evolving project needs. Here are some guidelines to consider:

### Does the Native Code Contain JNI?
If your library implements C++ functions for a Java/Kotlin interface and offers Java classes as part of the package, shared objects are the way forward. You won't have to worry about the linker removing critical functions.

### Is It a Large Library/Framework?
If you're only utilizing a fraction of its capabilities, you likely don't want to include the entire code in your app's binary. Consider using static linking for optimization. For example, [OpenCV](https://opencv.org/) is a substantial library, and you might not need all its features. If you can use it solely through the native interface, linking it as a static archive can significantly reduce your APK size.

### Is the Library Shared Among Multiple Libraries with Shared Objects?
If yes, consider publishing it as a shared object to avoid code replication in each `*.so` file. But, again, if it's way too big then read the [previous section](#is-it-a-large-libraryframework).

### Other Considerations
This blog post covers essential aspects of linking in Android development with native libraries. However, there are additional considerations not explored here, such as:
#### Singletons and other unique objects
If you have a variable which is a [singleton](https://en.wikipedia.org/wiki/Singleton_pattern) (or just a global variable), exercise caution when placing it in a static library. [Recall that static libraries are copied into shared object files](#dont-mix-linking-types). So if you have multiple shared objects linked against a specific static library with a singleton implementation then each component can have its own instance of a singleton. This can lead to unexpected behavior. Therefore, it's crucial to carefully design and manage singletons within your app to ensure they function as intended across different parts of the application.

## Conclusion

In the realm of Android development with native (C++) libraries, our journey has unveiled a labyrinth of considerations. Whether it's navigating the intricacies of the Java Native Interface (JNI) or carefully weighing the benefits of shared objects (`*.so`) against the self-contained nature of archive files (`*.a`), every decision shapes the outcome of your application.

There's no universal rule of thumb that fits all scenarios. Android development with native libraries is a realm where flexibility and adaptability take center stage. What works seamlessly for one project may not be the best choice for another. Rather than seeking a one-size-fits-all solution, the key is to deeply understand your project's unique needs.

As you embark on your own journey into Android development with native libraries, remember that the optimal solution lies not in blind adherence to specific guidelines but in your ability to adapt and make informed choices. It's a landscape where the needs of your application serve as the compass, and your understanding of the available tools and techniques empowers you to navigate confidently.

If you have any questions or need further assistance, don’t hesitate to reach out to [me](https://www.linkedin.com/in/nir-moshe-87a44622b/).
