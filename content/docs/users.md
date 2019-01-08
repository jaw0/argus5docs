---
title: "Users"
date: 2018-10-02T10:54:33-05:00
weight: 25
---


{{% notice warning %}}
The method of configuring users changed in version 5.0
{{% /notice %}}

To control access, argus wants to know who is permitted to access what. Associated with every valid user is:

*   username
*   password
*   home object (like a unix home directory)
*   list of access control groups

Users are managed via argusctl

*   **argusctl listusers** - list users
*   **argusctl getuser user=maggie** - display user config
*   **argusctl setuser user=maggie ...** - create or modify user
*   **argusctl deluser user=maggie** - delete a user

Create a user:
<pre>
argusctl setuser user=maggie pass=1234
</pre>

View a user:
<pre>
argusctl getuser user=maggie
<font color="#ccffff">200 OK
Name:     maggie
EPasswd:  $2a$10$hZjwJrHY1KrIcXTq1wiR.Zfk
Home:     Top
Groups:   user
</font></pre>

The home field specifies the name of the object for the user's default page. This is what the user will see when they first log in to argus. It is just like a unix home directory.

{{% notice info %}}
Also, just like a unix home directory, the user can do the equivalent of <tt>cd ..</tt> to visit other pages. To prevent the user from seeing other pages adjust the access control parameters appropriately.
{{% /notice %}}

The groups field is a white-space separated list of access control groups. Access control groups are documented further somewhere else.

