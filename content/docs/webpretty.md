---
title: "Web Pages"
date: 2018-10-02T10:54:33-05:00
---


You can adjust the look of the web pages in numerous ways. The most commonly used parameters are:

You can add text or html to the top and bottom of web pages.
Typically, these are set once at the top of the monitoring config
to customize the look of pages site-wide.

*   **header** - text or html to be placed at the top of web pages
*   **footer** - text or html to be placed at the bottom of web pages
*   **header_branding** - text or html to be placed at the top of web pages, typically a company name or logo
*   **login_notice** - text or html placed on the login page, such as a link to a privacy policy

<pre>
header_branding:  YoyoDyne Inc - internal use only
login_notice:     We use &lt;a href=policy&gt;cookies&lt;a&gt;
</pre>

There are several fields that can be used for general informational purposes.
They can be text or html (such as a link to additional information).
Typically, these are configured on groups or services to provide
information about what is being monitored.

*   **note** - informational
*   **info** - informational
*   **comment** - informational
*   **details** - informational

<pre>
Host "server.example.com" {
    note:  This server is a Sun T1000

    Service Ping
</pre>

You can change the name that gets displayed for each object by setting:

*   **label** - name to display on web page

