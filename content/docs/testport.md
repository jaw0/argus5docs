---
title: "Argus Test Port"
date: 2018-10-02T10:54:33-05:00
---


In order to remotely check that Argus is properly running, you can enable the test server on a TCP port.
in your argus.conf, add:

<pre>        test_port:      3074
</pre>

You should pick a port number that is not currently in use on the server argus is running on.

You can then check on it by telneting to the test port:

<pre>        telnet argus.example.com 3074

        Connected to argus.example.com.
        Escape character is '^]'.
        <font color="blue">Argus running</font>
        Connection closed by foreign host.
</pre>

Or, you can configure a 'Service' monitor on another Argus:

<pre>        Service TCP/Argus {
                hostname:       argus.example.com
                port:           3074
        }
</pre>

