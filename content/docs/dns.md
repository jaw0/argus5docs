---
title: "Testing DNS"
date: 2018-10-02T10:54:33-05:00
---


{{% notice warning %}}
Extended DNS testing is an advanced feature. If you are just starting out with argus, or are not familiar with the inner workings of DNS [[RFC 1035]](http://www.ietf.org/rfc/rfc1035.txt), it is recommended that you stick to the standard built-in DNS tests as described in [services](/docs/services/)
{{% /notice %}}

It seems that there is no end to the creativity of people mis-configuring DNS servers, or the number of failure modes that exist in DNS servers.

The extended DNS testing facility attempts to stay one step ahead.

You can specify any arbitrary DNS query, and perform any number of tests on the response

In addition to all of the parameters for a typical UDP test, the following can also be specified:

## Specifying a Query

*   **zone** - the DNS zone to query about
*   **class** - the DNS class to query about. (typically <tt>IN</tt>)
*   **recurse** - should the query be recursive
*   **query** - the type of query. most standard queries are supported, including:
    *   **A** - ask for an address
    *   **TXT** - ask for text
    *   **MX** - ask for MX server
    *   **NS** - ask for name server
    *   **SOA** - ask for the start-of-authority data
    *   **CNAME** - ask for canonical name
    *   **PTR** - ask for ptr data
    *   **STAT** - perform a status query

for example:

<pre>        Service UDP/DNS {
                zone:   example.com
                class:  IN
                query:  A
        }
</pre>

## Specifying a Test

There are several different ways to test the response

*   **none** - up if we receive a response
*   **noerror** - up if the response is error free
*   **authok** - up if the response has the <tt>authoratative</tt> flag set
*   **serial** - perform an [extended test](/docs/xtservices/) on the serial number. _this only makes sense for SOA queries_
*   **nanswers** - perform an [extended test](/docs/xtservices/) on the number of answers
*   **answer** - perform an [extended test](/docs/xtservices/) on the answer itself

for example:

<pre>        Service UDP/DNS {
                zone:   example.com
                class:  IN
                query:  MX
                test:   answer
                expect: mail.example.com
        }
        Service UDP/DNS {
                zone:     example.com
                class:    IN
                query:    SOA
                test:     serial
                minvalue: 2002010100
                maxvalue: 2004123100
        }
        Service UDP/DNS {
                zone:     example.com
                class:    IN
                query:    NS
                test:     nanswers
                minvalue: 2
        }
</pre>

## Compatibility with Old DNS queries

The syntax is backwards compatible with the DNS tests in previous versions. So you can still say:

<pre>        Service UDP/Domain/example.com
        Service UDP/DNSQ
        Service UDP/DNS
</pre>

The backwards compatible syntax is also extended slightly, so you can say things like:

<pre>        Service UDP/DNS/NS/example.com {
                expect:         ns1.example.com
        }
        Service UDP/DNS/Serial/example.com {
                minvalue:       2002112000
        }
</pre>

Using the backwards compatible shorthand syntax will set things to reasonable default values (such as <tt>class: IN</tt> and <tt>test: answer</tt> or <tt>test: soa</tt>)

## Answer Format

When testing the answer, the answer section of the response is decoded into a textual format similar to the format of a DNS zone file or to the output of <tt>dig</tt>, and it may be multi-line if there is more than one answer. For example:

<pre>
www.example.com.  23h5m  IN   A    10.0.1.2
or
example.com.      1d     IN   MX   10 mail1.example.com.
example.com.      1d     IN   MX   20 mail2.example.com.
</pre>
