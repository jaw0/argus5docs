---
title: "Access Control"
date: 2018-10-02T10:54:33-05:00
---


In the users section we saw how to make a user a member of a set of groups. In order to control access to things, we specify which groups have access to a particular thing. If the user is a member of any of the specified groups, access is permitted.

say we have a users file entry:

<pre>Name:     bob
EPasswd:  $2a$10$sck5WIOLGsthgyX
Home:     Top
Groups:   gibber jabber
</pre>

bob now belongs to both the 'gibber' and 'jabber' groups.

<pre>        Group "Foo" {
                acl_foobar:     jabber gizzle
                ...
        }
</pre>

this gives all members of the groups 'jibber' and 'gizzle' foobar permission. Thus, the user bob, being a member of 'jabber', has foobar access.

<div class="TECHNOTE">In most installations, the default access control lists are sufficient, and you will not need to specify anything further.</div>

## Specifying Access Control Lists

You can specify access control parameters on an object in either of 2 modes: simple or extended.

In simple mode, there are 3 access control lists which control all permissions:

1.  acl_user
2.  acl_staff
3.  acl_root

acl_user controls basic read access to an objects webpage. acl_staff controls some read-write access, such as setting or removing overrides, and acknowledging notifications. acl_root controls everything else, such as accessing debug info, and the configuration data.

Note, these are separate acls, controlling access to different things. Argus will happily permit a user access to debugging info (acl_root) and deny them access to view the webpage (acl_user). If you want to permit a user access to everything, they will need to be in all 3 of the acls.

## Simple Mode Defaults

By default, if no acls are specified in the config file, argus uses 3 groups named 'user', 'staff', and 'root' and creates acls:

<pre>        acl_root:       root
        acl_staff:      staff root
        acl_user:       user staff root
</pre>

allowing you to assign one of these 3 groups to users in the users file

## Extended Mode

In extended mode, each separate function has its own acl.

<pre>        Group "Foo" {
                acl_mode:       extended
                acl_override:   staff
                acl_getconf:    sr_staff
                ...
        }
</pre>

The acls are:

*   acl_page
*   acl_getconf
*   acl_about
*   acl_flush
*   acl_override
*   acl_annotate
*   acl_logfile
*   acl_ntfylist
*   acl_ntfyack
*   acl_ntfyackall
*   acl_ntfydetail

<div class="TECHNOTE">In versions 3.5 and later, the acl_mode no longer needs to be specified, and simple mode and extended mode parameters may be mixed and matched.</div>

## Cumulative Inheritance

ACLs are cumulative from the top level down.

<pre>        Group "Foo" {
                acl_override:   foo

                Group "Bar" {
                        acl_override:   bar
                        ...
                }
        }
</pre>

Members of both groups 'foo' and 'bar' will have override access on 'Bar'.

The syntax '-group' can be used to remove groups. The special '-ALL' will remove all groups:

<pre>        Group "Foo" {
                acl_override:   foo bar
                Group "Bar" {
                        acl_override:   -foo baz
                        ...
                }
                Group "Baz" {
                        acl_override:   -ALL baz
                        ...
                }
        }
</pre>
