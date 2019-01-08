---
title: "Aberrant Behavior Detection"
date: 2018-10-02T10:54:33-05:00
---


Any service that has a numeric result can be checked for aberrant or unexpected values. Once enabled, argus uses the Holt-Winters algorithm to predict what the result should be, and anything outside the expected range will cause the service to be down.

to enable, configure a *maxdeviation* (or any severity qualified variant (eg. *maxdeviation.minor*))

{{< highlight conf >}}
    Service UDP/SNMP {
        oid:  ifInOctets[Gi3/0]
        calc: ave-rate-bits
        maxdeviation.minor:   2
        maxdeviation.warning: 4
    }
{{< / highlight >}}

by default, argus uses a seasonal period of one week, and alpha, beta, and gamma values reasonable for a week season. these can be changed using the parameters:

*   *hwab_period*
*   *hwab_alpha*
*   *hwab_beta*
*   *hwab_gamma*

See [this paper](http://www.it.iitb.ac.in/~praj/acads/seminar/04329008_ExponentialSmoothing.pdf) for details on the various parameters

