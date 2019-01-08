---
title: "Notifications"
date: 2018-10-02T10:54:33-05:00
---


Whenever something changes state, Argus can notify someone about it.

Argus realizes that in the real world, sometimes people forget, they go out of range of their paging service, their batteries die, etc. So, by default, when Argus wants to be sure someone knows about something it will require that the message be acknowledged and will resend and/or escalate until someone does.

## Configuring

To specify where to send notifications:

<pre>
notify:         mail:support@example.com  qpage:joe
</pre>

you can specify more than one. Currently notifying by mail and qpage are supported.
See below to add other methods.

By default, Services generate notifications and Groups do not. This can be changed:

<pre>        Group "Foo" {
                sendnotify:     yes
                Service Ping {
                        sendnotify:     no
                }
        }
</pre>

You can change the message that gets sent:

<pre>        Service UDP/SNMP {
                # cisco inlet temperature
                oid:            .1.3.6.1.4.1.9.9.13.1.3.1.3.1
                maxvalue:       27
                messagedn:      server room is too hot
                messageup:      server room has cooled back down
        }
</pre>

If you only want one message sent, and never resent or escalated:

<pre>        autoack:        yes
</pre>

By default, notifications are resent every 5 minutes, to change this, specify a value in seconds:

<pre>        renotify:       120
</pre>

## Escalating

After attempting to notify someone of a problem repeatedly, you may want to try notifying someone else:

<pre>        escalate:    10 qpage:manager; 30 qpage:cio; 60 qpage:ceo
</pre>

which means:

*   after 10 minutes page the manager
*   after 30 minutes page the CIO
*   after 1 hour page the CEO

## Acknowledging

On the webpage, click "Un-Acked Notifies". Ack them one-by-one by clicking "Ack", or ack several by checking their checkboxes and clicking "Ack Checked", or ack all of them by clicking "Ack All".

The ability to ack is controlled by the "acl_ntfyack" access control list. The ability to "Ack All" is controlled by the "acl_ntfyackall" access control list.

In order to remain sane, the author **highly recommends** verifying that anyone who may be sent notifications is also permitted to acknowledge them.

## Adding Other Notification Methods

{{% notice warning %}}
The syntax used for message variables changed in version 5.0
Prior versions used % sequences; while newer versions use go's template syntax.
{{% /notice %}}


Argus comes with mail and qpage built in, to add other methods, pick a name for the method
and add to the top of the monitoring config:

<pre>        Method "annoy" {
                command:        winpopup {{.ADDR}}
                send:           {{.CONTENT}}
        }

        notify:         annoy:bob
</pre>

This will cause notifications to be sent to 'bob' using the 'winpopup' program.
Various parameters can be used:

*   **{{.ADDR}}** - the recipient ('bob' in the example)
*   **{{.SENDER}** - the from address (set with the 'mail_from' parameter)
*   **{{.CONTENT}** - the message
*   **{{.SUBJECT}}** - the message subject
*   **{{.IDNO}}** - the notification id number
*   **{{.OBJECT}}** - the object name - full name
*   **{{.FRIENDLY}}** - the object name in a friendly format
*   **{{.UNAME}}** - the object name - short version
*   **{{.STATUS}}** - the object state (up or down)
*   **{{.CREATED}}** - the notification creation time
*   **{{.SEVERITY}}** - the severity level (set with the 'severity' parameter)
*   **{{.REASON}}** - the reason the object went down
*   **{{.RESULT}}** - the value returned by the test

