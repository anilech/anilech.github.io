---
layout: post
title: Datapatch hangs on "Gathering database info..."
date: 2022-02-17
tags: oracle datapatch type
excerpt_separator: <!--more-->
---

![kdpv]({{ "/assets/calanques_de_piana.jpg" | relative_url }})
Hi.

Hit this issue on the one Windows server. The datapatch command freezes completely after displaying "Gathering database info..." prompt like this

```
D:\oracle\ora193\opatch> datapatch -verbose
SQL Patching tool version 19.12.0.0.0 Production on Tue Aug 31 14:10:50 2021
Copyright (c) 2012, 2021, Oracle. All rights reserved.

Log file for this invocation: D:\oracle\cfgtoollogs\...\sqlpatch_invocation.log

Connecting to database...OK
Gathering database info...
```

The problem is that the datapatch uses Windows internal `type` command to create temporary files. It is defined in the `%ORACLE_HOME%\rdbms\admin\catcon.pm` file in the `sub os_dependent_stuff`:

```perl
my $WindowsDoneCmd = "\nhost type nul > ";
```

It turns out that on the server there was another `type.exe` file from [UnxUtils](http://unxutils.sourceforge.net/) in the PATH. So the datapatch has used it instead of command processor's internal `type`.

That's why things went wrong: the datapatch couldn't create the file and waits forever on the following line (`sub exec_DB_script`):

```perl
select (undef, undef, undef, 0.01) until (-e $DoneFile);
```

After removing the third-party `type.exe` from the PATH, the datapatch works fine again.

I have no idea why Oracle decided to use external utilities to create files while sqlplus can create files with its own SPOOL command just fine...

Hope it helps!

###### UPD:

1. To debug datapatch you can use the -debug switch, but it doesn't help much. Setting the environment variable "CATCON_DEBUG" to "true" enables debug mode in the catcon.pm and it is much more helpful.
2. You can use [`where`](https://ss64.com/nt/where.html) command to find files in the PATH:

```
C:\>where type
c:\Program Files\bin\type.exe

C:\>del "c:\Program Files\bin\type.exe"

C:\>where type
INFO: Could not find files for the given pattern(s).
```
<!--more-->
