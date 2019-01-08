---
title: "Distributed Monitoring"
date: 2018-10-02T10:54:33-05:00
---


{{% notice warning %}}
DARP is an advanced feature. If you are just starting out with argus, it is recommended that you skip this section.
{{% /notice %}}

{{% notice warning %}}
The syntax for configuring DARP changed in version 5.0
{{% /notice %}}

For increased reliability, some people may want to have multiple Argus servers monitoring their network.
There are several reasons you may want to do this, and Argus DARP has several modes of operation:


* **none**  - no redundancy, this is what you do now.
* **failover** - the master Argus server monitors things, a slave server sits idle and monitors the master. when the slave detects the master has failed, the slave takes over and monitors things
* **distributed** - a number of slaves each monitor the same or different things. the master gathers the results from the slaves, and determines the overall status of the things being monitored.
* **redundant** - both the master and slave monitor things.


Mix-and-match, combinations, and variations on the above are also possible

## Configuring

Each Argus server participating in DARP must be given a unique name (or tag) to identify it to the other servers. You can use the hostname of the server, or you can pick other names. In the examples below we will use 'master1', 'slave1', and 'slave2'.

Each server must configure itself in its argus.conf:

<pre>
darp_name       master1
darp_root       /etc/argus/cert/Argus_Root.crt
darp_key        /etc/argus/cert/argus1.key
darp_cert       /etc/argus/cert/argus1.crt
</pre>

All servers must use the same root cert, and should have its own private key + cert.
The private keys should be kept private.


Each server must configure the other servers in its monitoring config:

<pre>
Darp "master1" {
        type:           master
        hostname:       server1.example.com
        port:           8090
}

Darp "slave1" {
        type:           slave
        hostname:       server2.example.com
        port:           8091
        fetch_config:   master1
}

Darp "slave2" {
        type:           slave
        hostname:       server3.example.com
        port:           8091
        fetch_config:   master1
}
</pre>

{{% notice info %}}
In a typical configuration, identical darp configs can be used on all of servers.
If placed in a seperate file (eg. 003_darp) it can simply be copied.
{{% /notice %}}


The master can also monitor the status of its slave connections as normal Service monitors:

<pre>
    Group "Darp Slaves" {
        Service DARP/WATCH/slave1
        Service DARP/WATCH/slave2
    }
</pre>

