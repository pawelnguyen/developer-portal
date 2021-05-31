---
title: Run tests on emulated Android and iOS
subtitle:
tags: []
author: Guillaume Lethuillier
layout: doc_ci
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

<!-- 2021-03-30 based on 2548924630 in Confluence -->

# Coin Integration : Run tests on emulated Android and iOS

{% include alert.html style="primary" text="<b>Prerequisite</b><br>Your computer is expected to have been set up accordingly. Please follow the following guides for this purpose:<br>
<ul>
<li><a href='https://ledgerhq.atlassian.net/wiki/spaces/LCH/pages/1615200511' class='alert-link'>Mobile - Android and iOS</a></li>
<li><a href='https://ledgerhq.atlassian.net/wiki/spaces/LCH/pages/2177138906/iOS+build+on+macOS' class='alert-link'>iOS build on macOS</a></li>
</ul>
" %}

<!--  -->
{% include alert.html style="warning" text="<b>Warning</b><br>Different terminals have to be opened at the same time. Obviously, that could be terminal tabs. In this guide, <code>terminal X</code> refers indifferently to a terminal or one of its tabs." %}
<!--  -->


## Preliminary steps (common to Android and iOS)

1.  Build the relevant version of `live-common` and `yalc publish` it (to do it automatically, you can use the `setup_live.py` tool from [glethuillier/test-environments](https://github.com/glethuillier/test-environments) (_e.g._ `$ python3 setup_live.py -c polkadot`)

2.  Clone or pull [Ledger-Coin-Integration-team/ledger-live-mobile](https://github.com/Ledger-Coin-Integration-team/ledger-live-mobile) **and checkout the relevant branch**


| **Terminal 1** | **Terminal 2** |
| -------------- | -------------- |
| From `ledger-live-mobile` directory:<br>`$ yalc add @ledgerhq/live-common` <br> `$ yarn` <br> `$ yarn start` <br> Expected output: <br> <video controls muted preload='none' poster='../../../uploads/images/CI/2548924630/terminal1-poster.png'><source src="../../../uploads/images/CI/2548924630/Terminal1.mp4" type='video/mp4'></video>  | `$ ledger-live proxy` <br>  Expected output: <br> <video controls muted preload='none' poster='../../../uploads/images/CI/2548924630/terminal2-poster.png'><source src="../../../uploads/images/CI/2548924630/Terminal2.mp4" type="video/mp4"></video> <br> At this stage, an IP address is provided.<br> It will be used at a further stage to pair the Nano with the mobile app.|


## Android

| **Terminal 3** | **Terminal 4** |
| -------------- | -------------- |
| `$ adb start-server` <br> `$ emulator -avd <name of the device>` <br> `# e.g. emulator -avd Pixel_XL_API_30` <br> Expected output: <br>  <video controls muted preload='none' poster='../../../uploads/images/CI/2548924630/terminal3-poster.png'><source src="../../../uploads/images/CI/2548924630/Terminal3.mp4" type="video/mp4"></video>  <br> At this stage, the emulator starts. | `$ yarn run android` <br>  Expected output: <br>  <video controls muted preload='none' poster='../../../uploads/images/CI/2548924630/terminal4-poster.png'><source src="../../../uploads/images/CI/2548924630/Terminal4.mp4" type="video/mp4"></video>  <br> At this stage, the Mobile app starts. <br> _Note: if the app is blank, just relaunch it._|

Then, pair the Nano using the IP address provided in the preliminary steps


## iOS

| **Terminal 3** |
| -------------- |
| `$ yarn run ios` <br> Expected output: <br>  <video controls muted  preload='none' poster='../../../uploads/images/CI/2548924630/terminal3-IOS-poster.png'><source src="../../../uploads/images/CI/2548924630/Terminal3-IOS.mp4" type='video/mp4'></video>  <br> At this stage, both the emulator and the Mobile app start.|

Then, pair the Nano using the IP address provided in the preliminary steps
