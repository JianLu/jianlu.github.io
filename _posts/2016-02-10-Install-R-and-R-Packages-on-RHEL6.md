Install R and R packages on RHEL 6 (x86_64)
===========================================

## Install R software

In order to get R running on RHEL 6, we need to add an additional repository that allows us to install the new packages.
The [EPEL (Extra Packages for Enterprise Linux)](https://fedoraproject.org/wiki/EPEL) is a Fedora Special Interest Group
that creates, maintains, and manages a high quality set of additional packages for Enterprise Linux,
including, but not limited to, Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Linux (OL).

First, login as `root` user, and add EPEL repository:

``` bash
rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
yum update
yum install R --nogpgcheck
```

You can also search for additional R packages using `yum` command:

``` bash
yum list R-\*
```

When the installation is finished successfully, you can run R:

``` bash
R
# In the R console, type 'q()' to exit
```

## Install Rserve Package

### Download and Install

Find the Rserve package file in our R archive, or download the latest version from
[CRAN](https://cran.r-project.org/web/packages/Rserve/index.html).

Then use `R CMD` to install the package (the file name could be different):

``` bash
R CMD INSTALL Rserve_1.8-3.tar.gz
```

### Configuration

Rserve is configured by the configuration file /etc/Rserv.conf.
If no configuration file is supplied, Rserve accepts no remote connections,
requires no authentication and file transfer is enabled.
To enable remote access, we need to create a configuration file and enable remote:

``` bash
echo "remote enable" > /etc/Rserv.conf
```

The other possible configuration items are as follows
(all entries are optional, default values are in angled brackets):

``` bash
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
```

### Start Rserve

Login as `rudolph` user and start the Rserve by:

``` bash
R CMD Rserve
```

Then check if the Rserve can be remotely accessed from other machine:

``` bash
# telnet <host> <port(6311)>
```

If you have an accesible Rserve, you should see a response like:

``` bash
Trying x.x.x.x...
Connected to x.x.x.x.
Escape character is '^]'.
Rsrv0103QAP1
```



## Reference

- [Install R in Linux](http://www.jason-french.com/blog/2013/03/11/installing-r-in-linux/)
- [Rserve Webpage](https://rforge.net/Rserve/)
- [ROracle CRAN](https://cran.r-project.org/web/packages/ROracle/index.html)
