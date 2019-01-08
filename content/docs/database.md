---
title: "Database Testing"
date: 2018-10-02T10:54:33-05:00
---

{{% notice warning %}}
The syntax used for DSNs changed in version 5.0.
Prior versions used perl's syntax; while newer versions use go's syntax.
{{% /notice %}}


## Configuring

You will need to specify various parameters:

<pre>Service DB {
    dbtype:     mysql
    dbname:     mydb
    dbparam:    tls=true
    user:       argus
    pass:       secret
    hostname:   dbhost.example.com
    sql:        SELECT COUNT(*) FROM mytable
}
</pre>

*   **hostname** - the hostname of the database server
*   **dbtype**   - type of database. currently supported: postgres, mysql, sqlserver, hdb
*   **dbname**   - the name of the database
*   **dbparam**  - any required connection parameters.
*   **user** - the username to use for connecting to the database
*   **pass** - the password to use for connecting to the database, depending on your database, this may be optional.
*   **dsn** - optionally, specify the dsn used to connect to the database. consult the documentation for the go driver for your database for exact syntax.
*   **sql** - any arbitrary sql 'select' statement

The results of the select statement may be tested using any of the normal tests described in [extended testing](xtservices.html), such as 'expect', or 'minvalue'.

