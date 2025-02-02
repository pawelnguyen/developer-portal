---
title: Developer mode
subtitle:
tags: []
layout: doc_pa
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

To activate the Developer mode in Ledger Live, go to the **Settings/About** section, and click ten times on the Ledger Live version.

This will make the Developer tab appear.

![screenshot of the Developer tab](../images/developer_mode_tab.png "Developer tab")

Note : this feature is available only for Ledger Live version 2.32 and above.

This new section will give you access to the following content :

- **Allow debug apps** : Display and allow opening debug tagged platform apps.
- **Allow experimental apps** : Display and allow opening experimental tagged platform apps.
- **Set the catalog provider** : Switch between multiple platform apps sources (Production of Staging).
- **Enable platform dev tools** : Enable opening platform apps dev tools window.
- **Add a local app** : Browse local files and add a local app using a local manifest.

We’ll dive into each section to describe how they work.

## Allow debug apps

This setting, when enabled, will display and allow opening “debug” tagged platform apps.
The first debug tagged platform app you will now see appear in the Discover section will be the Debugger. This app has been designed by Ledger to specifically allow you to test the API and understand how it works.

## Allow experimental apps

This setting, when enabled, will display and allow opening “experimental” tagged platform apps.
As long as an application hasn’t been reviewed and approved by Ledger, it can only remain experimental.

## Set the catalog provider

The catalog provider is a variable used to define the list of applications being displayed in the discover section. Ledger uses two lists of applications, those in staging (currently being tested), and those in production (validated already, and usually available to all our users).

## Enable Platform dev tools

This setting will enable opening the platform apps dev tools window, which can become handy when debugging your application.

![screenshot of the Platform dev tools](../images/platform_dev_tools.png "Developer tools window")

## Add a local app

This option will allow you to browse for local files and add a local app using a local manifest.
With it you can be autonomous in testing your application.
You’ll have to load your own manifest to make your application appear in the Discover section.
