---
title: Build the application
subtitle:
tags: [ledger app builder, app builder, container, compile, make, setup]
category: Nano Application
author:
toc: true
layout: doc_na
---




[Ledger App Builder](https://github.com/LedgerHQ/ledger-app-builder) is a container image which contains all dependencies to compile an application for Nano S/X.

<!--  -->
{% include alert.html style="warning" text="When you submit an app, using Ledger App Builder to build the application is a Security Requirement." %}
<!--  -->

To use the container image, you need to either install [Docker](https://docs.docker.com/get-docker/), [Podman](https://podman.io/), or [Buildah](https://buildah.io/) and apply the following steps.

<!-- ------------- Image ------------- -->
<div style="text-align:center">
<img src="../images/folder-organization.png" ></div>
<!-- --------------------------------- -->

## 1. Build the container (standard way)...

The container can be build using standard tools:

```bash
# Docker
sudo docker build -t ledger-app-builder:latest .
# Podman (from https://podman.io/)
podman build -t ledger-app-builder:latest .
# Buildah (from https://buildah.io/)
buildah bud -t ledger-app-builder:latest .
```

## ... Or Build with App Scanner

Images can embed the [Coverity Scan](https://scan.coverity.com/) build tool. It is an excellent static analysis tool, and it can be very useful to find bugs in Nano apps.

The build tool must be downloaded before building the image. The archive can be downloaded from <https://scan.coverity.com/download>. Download is available to everyone, but it requires to create an account. After having registered, download Coverity Build Tool 2020.09 for Linux64 and place the downloaded archive in the `coverity` directory.

Then, build the container with:

```bash
# Docker
sudo docker build -t ledger-app-scanner:latest .
# Podman (from https://podman.io/)
podman build -t ledger-app-scanner:latest .
# Buildah (from https://buildah.io/)
buildah bud -t ledger-app-scanner:latest .
```

## 2. Compile your app in the container

### 2.1 Get the Ledger Boilerplate Application

The Boilerplate Application was specifically designed for developers to play around with and read the code. You can clone it in your working folder to start a new project.
Applications that support multiple BOLOS devices are typically contained within a single repository, so you can use the same repository to build an app for different Ledger devices.

```bash
git clone https://github.com/LedgerHQ/app-boilerplate.git
```

### 2.2 Build the Application

#### For the Nano S

In the source folder of the application:

```bash
$ # docker can be replaced with podman or buildah without sudo
$ sudo docker run --rm -ti -v "$(realpath .):/app" ledger-app-builder:latest
root@656be163fe84:/app# make
```

#### For the Nano X

For Nano X, specify the `BOLOS_SDK` environment variable before building your app, in the source folder of the app:

```bash
$ # docker can be replaced with podman or buildah without sudo
$ sudo docker run --rm -ti -v "$(realpath .):/app" ledger-app-builder:latest
root@656be163fe84:/app# BOLOS_SDK=$NANOX_SDK make
```

<!--  -->
{% include alert.html style="primary" text="If you change the <code>BOLOS_SDK</code> variable betwin two builds, you can first use <code>make clean</code> to avoid errors." %}
<!--  -->


#### Using the Clang Static Analyzer

The Docker image includes the [Clang Static Analyzer](https://clang-analyzer.llvm.org/), which can be invoked with:

```bash
$ # docker can be replaced with podman or buildah without sudo
$ sudo docker run --rm -ti -v "$(realpath .):/app" ledger-app-builder:latest
root@656be163fe84:/app# make scan-build
```

### 2.3 Exit the image

The build generates several files in your application folder and especially the `app.elf` that can be loaded to a [physical Nano S](../load) or an [emulated Nano X or S with Speculos](../../speculos/start-here).

You can exit the image, with the command `exit`.

