---
title: "Overrides"
date: 2018-10-02T10:54:33-05:00
---


Everyone knows that **Red = Bad**. Two people working to fix the same thing is also bad. An override is a way of saying "I know it is down" or "I'm working on it".

Overrides are typically used in the 2 following ways:

*   When someone discovers something is down, they will place an override on the item letting their co-workers know that someone is fixing the problem, and also to make it not red (so that management doesn't storm on over wondering why there is red). In many companies a note may also be placed in a trouble-ticket system

*   If someone is going to perform maintenance on something, an override can be set on the item prior to doing the work, preventing it from turning red and notifying someone. This will prevent the person who would have been notified about the outage from having to wake up and scream at you.

## Setting an Override

1.  Click on the "Override" button on the webpage.
2.  Fill in the fields.
    *   **Comment** - a comment, such as "why is it down", or "how is it being fixed", perhaps a reference to a trouble-ticket number.
    *   **Expires** - how long should the object stay overridden. it should be set to roughly "how long will it take to fix". this prevents things from being placed in override and forgotten.
    *   **Mode** - if set to manual, the override will stay in place until it is manually removed or it expires.  
        if set to auto, it will stay in place until the object comes up, or it is removed manually, or it expires.
3.  Click "Submit"
4.  Depending on company policy, create a trouble-ticket.
5.  Fix problem.

In order to set an override on an object that is currently "up", manual mode must be used. Otherwise, immediately after being set, it will notice the object is up, and automatically disengage.

## Preventing Overriding

Often, there are things that should be able to be overridden (such as a router), and things that should not be (such as "our entire east coast network"). This can be specified in the config file:

<pre>        Group "East Coast Network" {
                <font color="blue">overridable:   no</font>
                Host "gw-phila1" {
                        Service Ping
                }
                Host "gw-nyc1" {
                        Service Ping
                }
                ...
        }
</pre>

Additionally, you can control who can and cannot override what on a per user, per object basis using access control lists. Overrides are protected by "acl_override" (or, in simple mode, "acl_staff").

