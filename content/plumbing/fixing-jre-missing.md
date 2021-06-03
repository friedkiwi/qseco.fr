---
title: "Fixing ‘JRE libraries are missing or not compatible….’ on Traveler installation"
date: 2019-03-26
draft: false
---

On modern Ubuntu Linux (18.04 LTS in my case), the InstallAnywhere installer fails with the following error:

    JRE libraries are missing or not compatible.... 

IBM provides the following article to troubleshoot this issue:

https://www-01.ibm.com/support/docview.wss?uid=ibm10737993

Sadly enough, the information and work around in this article is not entirely accurate. The reason why the installer fails is because it relies on an included JRE, which is 32-bit only (even though the applications are 64-bit). As a consequence, you need to have multilib enabled and installed. Refrain from carrying out the fix in the article since it will lead to stability issues on your server and won’t fix the installation process.

The installation process can be fixed using:

    $ sudo dpkg --add-architecture i386
    $ sudo apt-get update
    $ sudo apt-get install zlib1g:i386

This should allow you to continue the installation. This fix will work on most InstallAnywhere self-extracting installers from that time period, including non-IBM supplied packages.
