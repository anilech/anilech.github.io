---
layout: post
title: "How to transfer files to or from oracle directory"
date: 2018-10-04
tags: oracle directory
excerpt_separator: <!--more-->
---

![kdpv]({{ "/assets/rosenbauer.jpg" | relative_url }})

Sometimes you may need to copy files from or to the Oracle directory.
It is easy when you have direct access to the database server's file system.
It is a little bit tricky when you don't (AWS RDS instance for example).
One way to accomplish this is to create database link between the existing database (the one you have access to) and the target db 
 and use [DBMS_FILE_TRANSFER](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/DBMS_FILE_TRANSFER.html) package to copy files between instances.

#### I. powershell+odac: ora_dir_transfer.ps1
[ora_dir_transfer.ps1](https://github.com/anilech/oracle_directory_transfer) is another solution which doesn't require the second database.
It is a powershell script which uses [ODAC](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html) to access the database and [UTL_FILE](https://docs.oracle.com/en/database/oracle/oracle-database/19/arpls/UTL_FILE.html) package to read/write files on the database server.
It is influenced by [this perl script](https://stackoverflow.com/questions/29431398/perl-script-to-download-raw-files-from-amazon-oracle-rds).
You may need to fix Oracle dll path on the "[Reflection.Assembly]::LoadFile" line.

#### II. sqlcl+javascript (download only): sqlcl_ora_dir_download.js
The [sqlcl](https://www.oracle.com/database/technologies/appdev/sqlcl.html) is the Oracle's sqlplus written in java, therefore it supports a bunch of platforms.
It also runs javascript natively, so it is possible to create quite powerfull scripts.
The [sqlcl_ora_dir_download.js](https://github.com/anilech/oracle_directory_transfer/) script works in a download-mode only,
but it doesn't require the UTL_FILE's grant.
<!--more-->
