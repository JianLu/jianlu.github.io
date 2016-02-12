---
layout: post
title: "Install R and R packages on Windows"
date: 2016-02-10 10:30
categories: [Tech]
tags: [jekyll, linux, R]
description: Introduction of installing R and R packages on Windows 7 64bit
---

## Contents
{:.no_toc}

* TOC
{:toc}

---

## Install R and RTools

Find and download the executable installer from [R website](https://www.r-project.org/),
you mey need both **base** and **Rtools** installers.

> Remember to check the box to add Rtools into system path,
> It is neccessary when you need compile R package from source.
> Or you may need to manually add Rtools to PATH variable.

![Install Rtools](/images/rtools_install.JPG){:.center-image #img-rtools}
*Rtools Installation*

## Install ROracle

### Install from binary

If you only use 64-bit R, you can download ROracle for Windows directly
from [Oracle website](http://www.oracle.com/technetwork/database/database-technologies/r/roracle/downloads/index.html).

The zip file includes the binary files and run following command to install ROracle into your R library folder:

~~~ bash
R CMD INSTALL ROracle_1.2-1.zip
~~~

### Install from source

If you want ROracle available for both 32-bit and 64-bit R, you need to compile ROracle from source.
You can download the package source from [ROracle CRAN page](https://cran.r-project.org/web/packages/ROracle/).

ROracle is based on the OCI, therefore for compiling on Windows,
ROracle needs either the Oracle Instant Client or only the Oracle Database Client that is part of Oracle Database distribution.

For details, please read the [INSTALL Doc](https://cran.r-project.org/web/packages/ROracle/INSTALL) carefully.
In this article, we only provide the example of compiling ROralce using Oracle Instant Client.

#### Instant Client

Firstly, download Instant CLient for Windows (x64) from
[Oracle page](http://www.oracle.com/technetwork/topics/winx64soft-089540.html).
You may need **basic** (or basic lite) package and **SDK** package.

For example, I download version 11 for both x64 and win32, and extract with following directories.
I also extract sdk packages and copy the `sdk` folder to the corresponding instant client directory.

~~~
C:/instantclient
    \_ x64
        \_ all files of instantclient_11_2 for windows.x64
        \_ sdk folder for windows.x64
    \_ i386
        \_ all files of instantclient_11_2 for win32
        \_ sdk folder for win32
~~~

#### Set PATH

The add `c:\instantclient\i386` and `C:\instantclient\x64` into `PATH` system variable.
(Please refer to [here](https://java.com/en/download/help/path.xml) about how to change PATH variable)

#### Compile ROracle

Use following commands to build ROracle for both 32-bit and 64-bit versions of R:

~~~ bash
set OCI_LIB32=c:/instantclient/i386
set OCI_LIB64=c:/instantclient/x64
R CMD INSTALL --build --merge-multiarch ROracle_1.2-1.tar.gz
~~~

#### Troubleshooting:

Please search "Troubleshooting" in the [INSTALL Doc](https://cran.r-project.org/web/packages/ROracle/INSTALL).


## Install Rserve

### Download and Install

Download Windows binary package from [Rserve CRAN page](https://cran.r-project.org/web/packages/Rserve/index.html),
then install the package using:

~~~ bash
R CMD INSTALL Rserve_1.7-3.zip
~~~

### Configuration

Suppose we only use 64-bit version.
After the installation, you can find the `Rserve.exe` file in Rserve folder in the R library directory.
For my case, it is in:

~~~
C:\Users\JianLu\Documents\R\win-library\3.2\Rserve\libs\x64
~~~

Copy the all the Rserve files to the directory containing R.dll file.
For may case, it is

~~~
C:\Program Files\R\R-3.2.3\bin\x64
~~~

### Limitation

Make sure that you use the one most recently installed,
because Rserve detects the location of `$RHOME` from the registry.

Using Rserve in Windows is not recommended because the Windows version of Rserve is slightly limited compared to its unix counterpart.
Quick overview of the differences: (for technical details see bottom of this page)

- the config file is Rserv.cfg in the working directory (unless changed at compile-time)
- initialization of `R_HOME` is performed automatically by fetching the information from the registry,
i.e. you should use R installed by the official Windows installer and
leave the DCOM option checked (this is the default) unless you know what you're doing.
You should copy Rserve.exe in the bin directory of such installed R.
- no parallel connections are supported, subsequent connections share the same namespace
- sessions are not supported - this is a consequence of the fact that parallel connections are not supported
- local unix sockets are not supported (by design, as the name implies)


## Reference

- [ROracle CRAN page](https://cran.r-project.org/web/packages/ROracle/)
- [Rserve main page](https://rforge.net/Rserve/)
- [Rserve CRAN page](https://cran.r-project.org/web/packages/Rserve/index.html)