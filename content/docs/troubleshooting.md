---
title: "Troubleshooting"
date: 2018-10-02T10:54:33-05:00
---


At some point you may wonder why Argus thinks something is down or up (perhaps you tested it manually and you disagree), or why argus is doing such-and-such instead of other-such.

The <tt>debugging</tt> page contains all sorts of information about the object in question, some of which may be useful for a tech to troubleshoot issues (but beware, this is no place for a PHB).

The meaning of some of the information will be obvious from its name or from reading the configuration documentation, some won't be obvious. In particular, you may find such things as: time of the last test, time of the next test, the reason a test failed, and the values argus is using for every configurable parameter.

Access to this information is controlled by 'acl_about'. To minimize confusion of management and/or end-users, it is recommended that access only be given to technical staff capable of understanding this information.

The same information is also available via argusctl
<pre>argusctl dump obj=Top:Servers:Ping_10.0.0.4
</pre>

## Logging

Argus will log various errors to syslog, if so configured.

## Service Debugging

For detailed, real-time 'what is happening' info, you can set the debugging flag on a service, this will send all sorts of data to syslog:

<pre>
Service UDP/SNMP {
    hostname:       cisco-1.example.com
    community:      qwerty
    oid:            .1.3.6.1.4.1.9.9.13.1.3.1.3.1
    maxvalue:       27
    <font color="red">debug:          yes</font>
}
</pre>

You can also toggle the debugging flag at run-time using argusctl:

<pre>argusctl setdebug obj=Top:Servers:Ping_10.0.0.4
argusctl clrdebug obj=Top:Servers:Ping_10.0.0.4
</pre>

