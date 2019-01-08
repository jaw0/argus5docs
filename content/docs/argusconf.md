---
title: "Argus Conf"
date: 2017-12-28T11:11:33-05:00
weight: 10
---

This file is passed to argus on the command line `argusd -c /path/argus.conf`
and controls various aspects of argus itself. In prior versions of argus,
many of these parameters were specified some other way.

The file consists of parameters and values, and may contain
comments by using '#': All parameters have default values and
may be ommited.


<pre>

<font color="#ffcccc"># send messages to syslog (highly recommended)</font>
syslog           local1

<font color="#ffcccc">################################################################
# where can argus find things?
################################################################</font>

<font color="#ffcccc"># control socket for argusctl
# NB. if you set this, you will also need to tell argusctl</font>
control_socket  /var/run/argus.ctl

<font color="#ffcccc"># location of the monitoring config (file or directory)</font>
monitor_config  /home/argus/config

<font color="#ffcccc"># data directory - argus will write data here</font>
datadir         /home/argus/data

<font color="#ffcccc"># location of installed files for the web interface</font>
htdir           /home/argus/htdir


<font color="#ffcccc">################################################################
# what ports should argus use?
################################################################</font>
port_http       8080
port_https      8443
port_test       8088

<font color="#ffcccc"># use https? (recommeneded)</font>
tls_cert            /etc/ssl/cert/example.crt
tls_key             /etc/ssl/cert/example.key


<font color="#ffcccc">################################################################
# configure the dns resolver
################################################################</font>
<font color="#ffcccc"># if these are not configured,
# argus will use /etc/resolv.conf
# you may include these multiple times</font>
dns_server      10.8.8.8
dns_search      example.com


<font color="#ffcccc">################################################################
# control argus maximum threads
# the defaults should be good for typical small
# to medium sized installations
################################################################</font>
<font color="#ffcccc"># how many dns resolver threads to run</font>
resolv_maxrun   2

<font color="#ffcccc"># how many ping threads</font>
ping_maxrun     10

<font color="#ffcccc"># how many monitoring threads</font>
mon_maxrun	100


<font color="#ffcccc">################################################################
# if we are going to run a distributed network of argus servers
# what is the name of the local instance?
# every server must have a unique name, the hostname will be
#  fine, but logical names may simplify the monitoring configs</font>
darp_name       argus1

<font color="#ffcccc"># all argus-to-argus communication (both darp + remote agent) is
# encrypted + authenticated using pki
# see the darp + remote agent docs for details

# the root cerificate</font>
darp_root       /etc/ssl/cert/argus-root.crt

<font color="#ffcccc"># the keypair for this server</font>
darp_cert       /etc/ssl/cert/argus1.crt
darp_key        /etc/ssl/cert/argus1.key


<font color="#ffcccc">################################################################
# if argus has runtime problems - errors can be emailed to
#  an admin. this is highly recommended</font>
errors_mailto    sysadmin@example.com
errors_mailfrom  argus@example.com

<font color="#ffcccc"># enable development mode - will run slower and crash more
# do not enable unless you are doing development on argus</font>
devmode          yes

<font color="#ffcccc"># enable various internal debugging
# - see source code for settings</font>
# debug resolv
# debug configure
# debug all

</pre>
