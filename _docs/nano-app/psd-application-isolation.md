---
title: Application Isolation
subtitle: Checking the apps on the Ledger Nano S
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

So how do I know that the apps I'm running on my Ledger devices are doing what I intend them to do? What protects me from getting a virus on my Ledger devices?

Ledger solves these problems by utilizing the application isolation technology available in the Secure Element - the ARM [Memory Protection Unit](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dai0179b/CHDFDFIG.html) and [Operating Modes](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.ddi0210c/Cihhcjia.html). The Memory Protection Unit is used to natively isolate each app to its own memory region, and apps run in User mode whereas the operating system runs in Supervisor mode. By restricting the device to a single-task model where only one app can run at a time, and each app is isolated from the rest of the device, apps are prevented from accessing your cryptographic secrets on the device unless you explicitly give them permission to.

The access that applications have to cryptographic secrets managed by the operating system can be configured when loading an application onto the device. Instead of accessing secrets like the device's master seed directly, applications instead have to request the operating system to derive a node from the master seed by providing the operating system with a path to the requested node. When the application is loaded, the BIP 32 paths that the application is permitted to derive nodes from are specified. If the application requests the operating system to derive a node on a path that it is not permitted to use, the request is denied. In this way, many different applications can be loaded onto the device, and each of them can be restricted to a specific subtree of the HD tree depending on the application's purpose. This process of requesting the operating system to perform an operation as Supervisor is called a syscall, it is discussed in [this section](../interaction-bolos-apps).

Still, the importance of only installing apps that you can trust should not be understated. In the next section, we'll talk about how Ledger's operating system provides attestation features that allow the device to verify the authenticity of the apps that you're installing by checking a digital signature that is sent along with apps when they're loaded onto the device. Additionally, we'll discuss how Ledger's platform is open-source friendly which enables you to review the apps that you're using to manage your assets.

