---
title: "Install"
date: 2017-10-22T16:12:54-04:00
weight: 2
---

### Verify that you have the prerequisites installed

1. sendmail and qpage are recommended. either or both can be used to send notifications.
    + Find sendmail at www.sendmail.org
    + Find qpage at www.qpage.org
    + Note: this does not need to be the real sendmail, as long as it looks and smells like sendmail.
      ie. qmail's sendmail compatible sendmail program will be just fine.
2. fping is used by the Ping Monitoring module for ping tests. You almost certainly want to run ping tests.
    * Find fping at www.fping.com
3. if you are installing from source, a go compiler

## Installing from binary

1. download the correct tarball
2. unbundle the tarball
3. as root, run make install

## Installing from source

1. download the source code
2. unbundle the tarball
3. optionally, read and modify the Makefile
4. run make
5. as root, run make install

