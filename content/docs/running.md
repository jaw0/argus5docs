---
title: "Running Argus"
date: 2018-10-01T11:02:05-04:00
weight: 20
---

Typically, argus started by running (as root)

    argusd -c /path/to/config/file

Argus will read the specified config file, and run
in the background.

Typically, the config file will specify a user and group
for argus to switch to after starting.


# Command Line options

    -c    specify the config file

    -f    run in the foreground

    -d    enable all debugging

    -A    run in agent mode using the specified cert

    -p    use the specified tcp port in agent mode

    -s    use the specified control socket


