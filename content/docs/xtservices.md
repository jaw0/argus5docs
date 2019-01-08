---
title: "Extended Testing"
date: 2018-10-02T10:54:33-05:00
---


{{% notice warning %}}
Extended testing is an advanced feature. If you are just starting out with argus it is recommended that you stick to the standard built-in service tests as described in [services](/docs/services/)
{{% /notice %}}


Most services (those places where it makes sense) can do various comparisons on the
service's result:

*   **expect** - a regular expression to expect
*   **nexpect** - a regular expression to not expect
*   **eqvalue** - value must be equal to this
*   **nevalue** - value must not be equal to this
*   **minvalue** - value must be this or more
*   **maxvalue** - value must be this or less

There are also several pre-processing and calculations that can be performed before testing

*   **pluck** - a regular expression used to pull a value out of some content.
*   **unpack** - an unpack expression to pull a value from a binary packet
*   **jpath**  - a [jsonpath](https://goessner.net/articles/JsonPath/) expression to pull a value from json content
*   **xpath**  - a [XPath](https://en.wikipedia.org/wiki/XPath) expression to pull a value from xml content
*   **calc** - perform a calculation, such as averaging, or rate
*   **expr** - apply a mathematical expression
*   **scale** - apply a scale factor

for example

<pre>        Service TCP {
                uname:          disk-space
                messagedn:      /home is nearly full
                port:           6543
                send:           /home
                pluck:          \s(\d+)%
                maxvalue:       90
        }
        Service UDP/NTP {
                uname:          dispersion
                messagedn:      clock has drifted too far
                unpack:         x8 N
                scale:          65536
                maxvalue:       2
        }
        Service UDP/DNS/Serial/example.com {
                minvalue:       2002010100
                maxvalue:       2004123100
        }
        Service TCP/URL {
                url:            http://api.example.com/inventory.json
                jpath:          $.store.book[0].price
                expr:           x * 100
                minvalue:       100
                maxvalue:       200
        }
        Service TCP/URL {
                url:            http://api.example.com/inventory.xml
                xpath:          /store/book[1]/price
                expr:           x * 100
                minvalue:       100
                maxvalue:       200
        }

</pre>

## Aberrant Behavior Detection

Services can be checked for values outside of the predicted range using [Holt Winters](/docs/hwab/)

## Graphing

All services which can make use of the extended tests can have the resulting value graphed.
See the documentation on [graphing](/docs/graphing/).

