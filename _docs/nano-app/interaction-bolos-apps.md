---
title: Interaction Between BOLOS and Apps
subtitle: Getting smooth exchanges
tags: []
toc: true
toc_sticky: true
author:
layout: doc_na
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

## Introduction

Since BOLOS was designed based on a single-task model where only one app runs at any given time, an application is independently responsible for a lot of the things a typical OS would do. These things include managing hardware like the device screen, buttons, timer, etc. as well as handling all I/O with peripherals. However, there are many instances where a BOLOS application has to request the operating system to perform a certain operation. This is done using a syscall.

When an application performs a syscall, the Secure Element switches to Supervisor mode and the OS performs the requested task before giving control back to the application, in User mode. All syscalls have a wrapper function in the SDK that can be used to invoke them. A syscall may be used to access hardware accelerated cryptographic primitives (most of these functions are defined in `include/cx.h` in the SDKs), to perform low-level I/O operations (like receiving / transmitting to the MCU), or to access cryptographic secrets managed by BOLOS (for example, to derive a node from the BIP 32 master node).

## Error Model

If you are familiar with C programming, you will be used to error codes as the default error model. However, when programming in the embedded world, this traditional model reaches its limits, and can quickly overcomplicate large codebases. Therefore, we've implemented a try / catch system that supports nesting (direct or transitive) using the `setjmp` and `longjmp` API to facilitate writing robust code.

Here is an example of a typical try / catch / finally construct:

``` c
BEGIN_TRY {
    TRY {
        // Perform some operation that may throw an error using THROW(...)
    } CATCH_OTHER(e) {
        // Handle error
    } FINALLY {
        // Always executed before continuing control flow
    }
} END_TRY;
```

However there is a single constraint to be aware of with our try / catch system: a `TRY` clause must always be closed in the appropriate way. This means that if using a return, break, continue or goto statement that jumps out of the `TRY` clause you MUST manually close it, or it could lead to a crash of the application in a later `THROW`. A `TRY` clause can be manually closed using `CLOSE_TRY`. Using `CLOSE_TRY` is only necessary when jumping out of a `TRY` clause. Jumping out of a `CATCH` or `FINALLY` clause is allowed (but still, be careful you're not in a `CATCH` nested in a `TRY`).

You should use the error codes defined in the SDKs wherever possible (see `EXCEPTION`, `INVALID_PARAMETER`, etc. in `os.h`). If you decide to use custom error codes, never use an error code of `0`.

Developers should avoid creating a new try context wherever possible in order to reduce code size and stack usage. Preferably, an application should only have a single top-level try context at the application entry point (in `main()`).

## Syscall Requirements

BOLOS is based on an exception model for error reporting, therefore, it expects the application to call the BOLOS API using this mechanism. If an API function is called from outside a `TRY` context, then the BOLOS call is denied.

Here is a valid way to call a system entry point:

``` c
BEGIN_TRY {
    TRY {
        cx_hash_sha512(...);
    } FINALLY {}
} END_TRY;
```

However, as mentioned above, it is preferred to use as few try contexts as possible (not one per syscall). A single, top-level try context can be used to catch any exception thrown by any syscall performed by the application.

