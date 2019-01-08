---
title: "Advanced Features"
date: 2018-10-02T10:54:33-05:00
---

## Aliases

Often, when deciding how to arrange things, it may make sense to have something in more than one place. You may have a group of "Unix Servers" containing all of your unix servers, and also a group "Web Servers" containing all of your web servers. While you could just monitor the service twice, that just seems silly.

An Alias acts much like a "symlink", allowing the same object to appear in more than one place, but still only be monitored once.

<pre>Group "Web Servers" {
        Alias "threemile"  "Top:Unix_Servers:threemile"
        Alias "tarawa"     "Top:NT_Servers:tarawa"
        Alias "matagi"     "Top:NT_Servers:matagi"
}
</pre>

## uname

Sometimes you want to monitor 2 similar things, but with some minor difference. The way argus tells things apart is by is name. If the name argus wants to use is the same for both items, argus will complain. You can specify this name yourself, if you need to:

<pre>Host "jeremy-03.example.com" {
        Service TCP/HTTP {
                uname: HTTP-PRIMARY
        }
        Service TCP/HTTP {
                port:  8080
                uname: HTTP-SECONDARY
        }
}
</pre>

Without the <tt>uname</tt> specified, argus will not be able to tell these items apart (both would have the name <tt>HTTP_jeremy-03.example.com</tt>.

## Inheritance Bypass

The argus config file is hierarchical. Many parameters can be specified at a higher level, and the value will be inherited by objects below it. e.g.

<pre>Group "foo" {
    hostname:           server.example.com
    overridable:        no
    Service TCP/HTTP
    Service TCP/SSH
}
</pre>

the hostname and overridable parameters are specified on group-foo, and are inherited by the 2 services.

Sometimes, you don't want a parameter which is normally inherited to be inherited. In the above example, you may want the group to not be overridable, but the services to be overridable. Adding a <tt>!</tt> after the parameter name, will prevent the value from being inherited. e.g:

<pre>Group "foo" {
    hostname:           server.example.com
    overridable:        yes     # will be inherited by the services
    overridable!:       no      # will be used by group-foo only
    Service TCP/HTTP
    Service TCP/SSH
}
</pre>


