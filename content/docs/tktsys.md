---
title: "Ticket System"
date: 2018-10-02T10:54:33-05:00
---


Argus can attach overrides to your trouble ticket system, in order to:

1.  make sure something isn't overridden without opening a trouble ticket
2.  remove an override once a ticket is closed

*   <span class="param">tkt_check_url</span> the url to check the status of a ticket. **TICKET** will be replaced with the ticket number.  
    eg. <span class="example">http://tkt.example.com/tktview?t=TICKET</span>
*   <span class="param">tkt_check_expect</span> a string that will be returned if the ticket is open.  
    eg. <span class="example">OPEN</span>
*   <span class="param">tkt_view_url</span> the url to view a ticket. **TICKET** will be replaced with the ticket number.  
    eg. <span class="example">http://tkt.example.com/tktview?t=TICKET</span>

    once configured, argus will ask for the ticket number when an override is installed.

