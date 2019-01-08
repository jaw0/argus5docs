---
title: "Remote Agent"
date: 2018-10-02T10:54:33-05:00
---


Argus can monitor various systems and parameters on other servers via an agent


# Configuring Argus

The agent can gather all sorts of information on all sorts of subsystems on all sorts of Operating Systems. Except for yours. It works for most people, but for you, you might need to play around a bit. Just sayin'.

Somewhere at the top of your config, tell argus what port you configured the agent to use (164 in the example above):

<pre>        agent_port:     164
</pre>

In your argus config, you also need to specify the darp parameters
(the agent uses the same pki for security):

<pre>
<font color="#ffcccc"># what is the name of the local instance?</font>
darp_name       argus1

<font color="#ffcccc"># the root cerificate</font>
darp_root       /etc/ssl/cert/argus-root.crt

<font color="#ffcccc"># the keypair for this server</font>
darp_cert       /etc/ssl/cert/argus1.crt
darp_key        /etc/ssl/cert/argus1.key
</pre>


Configure things to monitor. You can use all of the usual minvalue, maxvalue, graphing, etc. parameters just as any other [service](/docs/services/).

<pre>
Host "server.example.com" {
    Service Agent/load {
        maxvalue:    0.5
    }

    Service Agent/disk {
        arg:        /home
        maxvalue:   80
    }

    Service Agent/netin {
        arg:        eth0
        maxvalue:   10000000
    }
}
</pre>

The particular things that can be remotely monitored can easily be extended.
See the 004_agent file in the examples directory.


# Running the Agent

* copy the appropriate argusd binary to the remote system

* start argusd in remote agent mode. you need to specify the location
of the argus root cert, and port number

<pre>
argusd -a 164 -A /etc/certs/argus_root.crt
</pre>

