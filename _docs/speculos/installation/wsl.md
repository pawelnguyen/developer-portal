---
title: Windows (with WSL 2)
subtitle:
tags: [install speculos, emulate, emulator, Nano X emulator, speculos]
category: Speculos
author:
layout: doc_sp
sort: 2
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

Building in WSL 2 (Windows Subsystem for Linux 2) is identical to the procedure in [building](../build).

Using Speculos with display features requires correctly exporting the X display.

Rough steps to do so below, detailed procedure available on [this blogpost](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/running-wsl-gui-apps-on-windows-10/ba-p/1493242).

- Add the following to your `.bashrc` within WSL2:

```bash
export DISPLAY="`sed -n 's/nameserver //p' /etc/resolv.conf`:0"
```

- Install an X server on your Windows host, like [VcXSrv](https://sourceforge.net/projects/vcxsrv/), and disable access control when prompted to do so.
- Simply launch the X server prior to launching an app through Speculos from WSL.
