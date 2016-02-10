---
layout: post
title: "Install R and R packages on RHEL 6 (x86_64)"
date: 2016-02-10 10:30
categories: [Tech]
tags: [jekyll, linux, R]
description: Introduction of installing R and R packages in RedHat 6
---

## Install R

In order to get R running on RHEL 6, we need to add an additional repository that allows us to install the new packages.
The [EPEL (Extra Packages for Enterprise Linux)](https://fedoraproject.org/wiki/EPEL) is a Fedora Special Interest Group
that creates, maintains, and manages a high quality set of additional packages for Enterprise Linux,
including, but not limited to, Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Linux (OL).

### EPEL Repository

First, login as `root` user, and add EPEL repository:

~~~ bash
rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
yum update
yum install R --nogpgcheck
~~~

You can also search for additional R packages using `yum` command:

~~~ bash
yum list R-\*
~~~

When the installation is finished successfully, you can run R:

~~~ bash
R
# In the R console, type 'q()' to exit
~~~

To verify is EPEL repository is enabled, run following command:

~~~ bash
yum repolist
~~~

And the output could be like:

~~~
repo id                                                                   repo name                                                                                     status
*epel                                                                     Extra Packages for Enterprise Linux 6 - x86_64                                                11,992
mirror.centos.org_centos_6_os_x86_64_                                     added from: http://mirror.centos.org/centos/6/os/x86_64/                                       6,575
rpmforge                                                                  RHEL 6Server - RPMforge.net - dag                                                              4,718
repolist: 23,285
~~~

### [Optional] CentOS Repository

Sometimes, there could be some library missing while installing R using yum.
In this case, you may need add CentOS repository by creating a new repository file:

~~~ bash
vi /etc/yum.repos.d/centos.repo
~~~

And add following contents (note that you need to change the version number depending on your RHEL version):

~~~
# Repoitory
[centos]
name=org_centos_6_os_x86_64
baseurl=http://mirror.centos.org/centos/6/os/x86_64/
enabled=1
gpgcheck=0
~~~

## Install Rserve Package

### Download and Install

Find the Rserve package file in our R archive, or download the latest version from
[CRAN](https://cran.r-project.org/web/packages/Rserve/index.html).

Then use `R CMD` to install the package (the file name could be different):

~~~ bash
R CMD INSTALL Rserve_1.8-3.tar.gz
~~~

### Configuration

Rserve is configured by the configuration file /etc/Rserv.conf.
If no configuration file is supplied, Rserve accepts no remote connections,
requires no authentication and file transfer is enabled.
To enable remote access, we need to create a configuration file and enable remote:

~~~ bash
echo "remote enable" > /etc/Rserv.conf
~~~

The other possible configuration items are as follows
(all entries are optional, default values are in angled brackets):

~~~
workdir <path> [/tmp/Rserv]
pwdfile <file> [none=disabled]
remote enable|disable [disable]
auth required|disable [disable]
plaintext enable|disable [disable]
fileio enable|disable [enable]
interactive yes|no [yes] (since 0.6-2)

(since version 0.1-9):
socket <socket> [none=disabled]
port <port> [6311]
maxinbuf <size in kb> [262144]

(since version 0.3):
maxsendbuf <size in kb> [0=unlimited]
uid <uid> [none]
gid <gid> [none]
su now|server|client [none] (since 0.6-1)

(since version 0.3-16):
source <file>
eval <expressions>

(since version 0.5 and unix only):
chroot <directory> [none]
sockmod <mode> [0=default]
umask <mask> [0]

(since version 0.5-3):
encoding native|utf8|latin1 [native]
(since version 0.6-2):
~~~

### Start Rserve

Login as `rudolph` user and start the Rserve by:

~~~ bash
R CMD Rserve
~~~

Then check if the Rserve can be remotely accessed from other machine:

~~~ bash
telnet <host> <port(6311)>
~~~

If you have an accessible Rserve, you should see a response like:

~~~
Trying x.x.x.x...
Connected to x.x.x.x.
Escape character is '^]'.
Rsrv0103QAP1
~~~

## Install ROracle Package

Find the R package files (DBI and ROracle) in our R archive, or download the latest version from
[DBI](https://cran.r-project.org/web/packages/DBI/index.html)
and [ROracle](https://cran.r-project.org/web/packages/ROracle/index.html).

### Install DBI

~~~ bash
R CMD INSTALL DBI_0.3.1.tar.gz
~~~

### Install ROracle

If you have Oracle Client installed, you need to set `LD_LIBRARY_PATH` and `ORACLE_HOME` variables.
For example, if the Oracle was installed in `/apps/oracle/product/11g`, then you need to do following:

~~~ bash
# Set variables
export ORACLE_HOME=/apps/oracle/product/11g/:$ORACLE_HOME
export LD_LIBRARY_PATH=/apps/oracle/product/11g/lib/:$LD_LIBRARY_PATH

# Install pacakge
R CMD INSTALL ROracle_1.2-1.tar.gz
~~~

If you have Oracle Instant Client installed, you need to set `OCI_LIB` and `LD_LIBRARY_PATH` variables.
For example, if the Instant Client was installed in '/apps/oracle/instantclient_11_2', then you need to do following:

~~~ bash
# Set variables
export OCI_LIB=/apps/oracle/instantclient_11_2/
export LD_LIBRARY_PATH=/apps/oracle/instantclient_11_2/:$LD_LIBRARY_PATH

# Install pacakge
R CMD INSTALL ROracle_1.2-1.tar.gz
~~~

To verify if ROracle is installed, you can launch R console by running `R` and load the library:

~~~ R
library(ROracle)
~~~

For more details about installing ROracle, please refer to <https://cran.r-project.org/web/packages/ROracle/INSTALL>.

## Reference

- [R RPMS - CRAN](https://cran.r-project.org/bin/linux/redhat/README)
- [How to Enable EPEL for RHEL/CentOS](http://www.tecmint.com/how-to-enable-epel-repository-for-rhel-centos-6-5/)
- [Install R in Linux](http://www.jason-french.com/blog/2013/03/11/installing-r-in-linux/)
- [Rserve Webpage](https://rforge.net/Rserve/)
- [ROracle CRAN](https://cran.r-project.org/web/packages/ROracle/index.html)
