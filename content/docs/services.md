---
title: "Services"
date: 2018-10-02T10:54:33-05:00
weight: 30
---


Services specify actual things to monitor. The very purpose of this software. So, you'll probably need to know about them

In the config file, Services are specified as:

<pre>        Service NAME {
                _data..._
        }
</pre>

Often, there will be no need to specify any data, and you may shorten the specification to just

<pre>        Service NAME
</pre>

<div class="TECHNOTE">If you are just starting out with argus, it is recommended that you stick to the short form when possible and let argus fill in the default values. In most cases, you will only say:  
<tt>    Service Ping</tt>  
<tt>    Service TCP/SMTP</tt>  
<tt>    Service TCP/IMAP</tt>  
in your config file, and argus will default the rest.</div>

NAME specifies the type of test. There are 4 main types of tests:

*   Ping - Pings a host
*   Prog - runs a program
*   TCP - tests a TCP port
*   UDP - tests a UDP port

Both TCP and UDP have a number of application tests built-in. Specifying an application does the same thing as setting the various bits of data to values appropriate for the protocol. But you could just as easily specify them directly.

The currently built-in application tests are:

*   TCP/SMTP TCP/FTP TCP/NNTP TCP/HTTP TCP/HTTPS TCP/Gopher TCP/Telnet TCP/SSH TCP/POP TCP/IMAP TCP/DNS TCP/LPD TCP/Whois TCP/Rwhois TCP/NFS TCP/NFSv3 TCP/POPS TCP/IMAPS TCP/SMTPS TCP/NNTPS TCP/SIP TCP/Slimserver
*   UDP/SNMP UDP/SNMPv2c UDP/SNMPv3 UDP/DNS UDP/DNSQ[[1]](#foot_1) UDP/NTP UDP/Portmap UDP/NFS UDP/NFSv3[[2]](#foot_2) UDP/RADIUS[[3]](#foot_3) UDP/SIP UDP/IAX2
*   several "special" tests: TCP/URL UDP/Domain TCP/RPC UDP/RPC

## Common Data that applies to all Services

<pre>        Service NAME {
                frequency:      60
                retries:        5
                timeout:        10
        }
</pre>

These do not need to be specified, they will happily default, or propagate from an enclosing group

*   <tt>frequency</tt> specifies how often the service is tested, in seconds.  
    The author is aware that technically, this should be called <tt>period</tt> -- deal with it.

*   <tt>retries</tt> if a test fails, we will retry it the specified number of times before declaring it down

*   <tt>timeout</tt> how long to wait for something (eg. a response from a mail server) before giving up and declaring the test a failure.

<div class="TECHNOTE">Note, specifying that things happen every 60 seconds (for example) does not mean that they will happen at the top of every minute (:00 seconds). Argus is smart and will load balance the tests to prevent the 59 seconds of quiet followed by an intense flurry of network congestion common in some other network monitoring applications. In argus-speak, things have a frequency and a phase, called <tt>phi</tt>. You cannot set this, but it is visible in the debugging web page.</div>

## Data for Prog Tests

<pre>        Service Prog {
                # make sure no one deleted root from the passwd file
                command:        cat /etc/passwd
                expect:         ^root:
        }
</pre>

These need to be specified. They will not default, or propagate from anywhere

*   <tt>command</tt> the command to run, along with any commandline arguments. specify it just as you would to your shell. you should either specify a complete path to the program, or verify that it is in argus's PATH

*   <tt>expect</tt> a regular expression that needs to match the output from the command. if not specified, the exit code from the program will be used to determine success or failure

## Data for Ping Tests

<pre>        Service Ping {
                hostname:       host1.example.com
        }
</pre>

This needs to be specified, but will propagate from an enclosing group

<tt>hostname</tt> the hostname or IP address to ping

## Data for TCP and UDP Tests

<pre>        Service TCP {
                hostname:       www.example.com
                port:           80
                send:           HEAD / HTTP/1.0\r\n\r\n
                expect:         HTTP
                readhow:        toeof
        }
</pre>

Both TCP and UDP have many of the same parameters

*   <tt>hostname</tt> the hostname or IP address to test. This needs to be specified, but will propagate from an enclosing group

*   <tt>port</tt> the port to test. This needs to be specified.

*   <tt>send</tt> data to send once connected. This needs to be specified. If nothing is specified, nothing will be sent to the remote server

*   <tt>expect</tt> a regular expression that needs to match the data received from the remote server. This needs to be specified. If nothing is specified, success or failure is determined by whether or not we received any data at all

*   <tt>readhow</tt> This needs to be specified. How much data should we try to read? <tt>toeof</tt> indicates read until the remote end closes the connection. <tt>banner</tt> indicates we only want to read a banner. <tt>once</tt> indicates to use whatever data is returned in the first read(). This parameter only applies to TCP not UDP

Specifying an application (eg. TCP/SMTP) will fill in port, send, expect, and readhow as appropriate for that protocol

See below for details on the special [TCP/URL](#TCP/URL), [UDP/Domain](#UDP/Domain), and [UDP/SNMP](#UDP/SNMP) tests

## <a name="UDP/SNMP">SNMP Tests</a>

<pre>        Service UDP/SNMP {
                hostname:       cisco-1.example.com
                community:      qwerty
                oid:            .1.3.6.1.4.1.9.9.13.1.3.1.3.1
                maxvalue:       27
        }
</pre>

*   <tt>hostname</tt> the hostname or IP address to test. This needs to be specified, but will propagate from an enclosing group

*   <tt>community</tt> the SNMP community. This needs to be specified, but will propagate from an enclosing group

*   <tt>oid</tt> the OID to query. It should be numeric, and the leading '.' is optional. This needs to be specified.

*   <tt>maxvalue</tt> the maximum permitted value, if the query returns something larger, we consider the service down. You can specify any of:

    *   maxvalue
    *   minvalue
    *   eqvalue
    *   nevalue

## <a name="UDP/SNMPv3">SNMPv3 Tests</a>[[5]](#foot_5)

<pre>        Service UDP/SNMPv3 {
                hostname:       cisco-1.example.com
                oid:            .1.3.6.1.4.1.9.9.13.1.3.1.3.1
                snmpuser:       joe
                snmppass:       secret
                snmpauth:       MD5
                snmpprivpass:   supersecret
                snmppriv:       DES
                contextname:    public
                contextengine:  80000000123456
                authengine:     80000000123456
        }
</pre>

*   <tt>snmpuser</tt> the snmp username This needs to be specified, but will propagate from an enclosing group

*   <tt>snmppass</tt> the snmp authentication password This needs to be specified if authentication is required, but will propagate from an enclosing group

*   <tt>snmpauth</tt> the authentication algorithm, MD5, SHA1, or none. This will default to MD5 if a password is provided, otherwise none.

*   <tt>snmpprivpass</tt> the privacy password This needs to be specified if privacy is required, but will propagate from an enclosing group

*   <tt>snmppriv</tt> the privacy encryption algorithm, DES or none. This will default to DES if a snmpprivpass is specified, otherwise none.

*   <tt>contextname</tt> the context name This needs to be specified, but will propagate from an enclosing group

*   <tt>contextengine</tt> the context engine-id This needs to be specified, but will propagate from an enclosing group, or will attempt to be auto-discovered.

*   <tt>authengine</tt> the authentication engine-id This needs to be specified, but will propagate from an enclosing group, or will attempt to be auto-discovered.

### SNMP rate calculations

Certain SNMP values are not useful themselves, but may be useful to watch after performing some manipulation. InOctets by itself is not useful, but using it to calculate a rate (or bandwidth use) in Bytes (or Bits) per second and comparing that value to a max or min is useful.

<pre>        Service UDP/SNMP {
                label:          Out
                hostname:       cisco-1.example.com
                # OutOctets
                oid:            .1.3.6.1.2.1.2.2.1.16.2
                calc:           ave-rate-bits
                maxvalue:       20000000        # 20M bits/sec
                minvalue:        1000000        #  1M bits/sec
        }
</pre>

This will convert the OutOctets value into a rate (Bytes per second), calculate the moving average, and convert it to Bits per second. If the resulting value falls outside the expected range of 1-20 Mbps, the test will fail.

<div class="TECHNOTE">The <tt>calc</tt> parameter was known as <tt>snmpcalc</tt> prior to version 3.2</div>

## <a name="TCP/URL">TCP/URL Content Test</a>

The TCP/HTTP tests that the HTTP service is up and running. The TCP/URL test will test the content of a web page.

<pre>        Service TCP/URL {
                url:     http://www.example.com/cgi-bin/shoppingcart.pl
                browser: Mozilla/5.0 (compatible; just testing)
                expect:  cart contains 1 item
        }
</pre>

*   <tt>url</tt> the url to fetch. it must begin with 'http://', and may contain an optional ':port'. It needs to be specified.

*   <tt>expect</tt> a regular expression that needs to match the data received from the remote server. This needs to be specified. If nothing is specified, success or failure is determined by whether or not we received any data at all

*   <tt>browser</tt> spoof the specified browser (send as the User-Agent). This is optional, and can be used if the server or application is serving different content based on browser.

## <a name="UDP/Domain">DNS Authoritativeness Test</a>

The UDP/DNS (and UDP/DNSQ) test that a DNS server is up and running. The UDP/Domain test will test that a server is answering authoritatively for a zone.

<pre>        Service UDP/Domain {
                hostname:       ns1.example.com
                zone:           example.com
        }
</pre>

*   <tt>hostname</tt> the hostname or IP address to query. This needs to be specified, but will propagate from an enclosing group

*   <tt>zone</tt> the DNS zone to check.

as a shortcut, an alternative syntax is supported:

<pre>        Group "nameserver" {
                hostname:       ns1.example.com
                Service UDP/Domain/example.com
                Service UDP/Domain/example.net
                Service UDP/Domain/example.org
        }
</pre>

## RPC tests

Both TCP and UDP RPC services can be tested. The RPC tests will query the RPC portmapper to learn the service's current port.

<pre>        Service UDP/RPC
                hostname:       ns1.example.com
                prognum:        100003
                version:        2
        }
</pre>

*   <tt>hostname</tt> the hostname or IP address to query. This needs to be specified, but will propagate from an enclosing group

*   <tt>prognum</tt> the RPC program number. this must be specified. many common RPC services can also be specified by name.

*   <tt>version</tt> the RPC program version. will default to 0.

as a shortcut, an alternative syntax is supported:

<pre>
        Group "server" {
                hostname:       server.example.com

                # Service UDP/RPC/program/version

                Service UDP/RPC/100003
                Service UDP/RPC/100003/2
                Service TCP/RPC/mountd
                Service TCP/RPC/mountd/2
        }
</pre>

* * *

<a name="foot_1">[1]</a> - UDP/DNS sends a 'status-query', UDP/DNSQ sends an 'IN ANY' query. some DNS servers (notably djbdns) do not handle 'status' queries.

<a name="foot_2">[2]</a> - UDP/NFS tests NFS version 2\. UDP/NFSv3 is NFS version 3.

<a name="foot_3">[3]</a> - This test uses the <tt>server-status</tt> query, which is not supported by all radius servers.
