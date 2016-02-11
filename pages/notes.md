---
layout: page
title: About
permalink: /notes/
---

# Portals:
{:.no_toc}

* TOC
{:toc}

---

# Jekyll

- [A good Jekyll tutorial](https://www.andrewmunsell.com/course/learning-jekyll-by-example/)


# Linux

## Yum

### Yum cheating sheet

[>>> pdf <<<](https://access.redhat.com/sites/default/files/attachments/rh_yum_cheatsheet_1214_jcs_print-1.pdf)

### Yum dependencies

Use following command to list all dependencies of the package:

~~~ bash
yum deplist <package>
~~~

Or use the command `repoquery` from `yum-utils` package to get a tree-like output:

~~~ bash
repoquery --tree-requires <package>
~~~

You also can find what packages require the given package

~~~ bash
repoquery --whatrequires --recursive <package>"
~~~


### Yum download plugin

Install the package including "downloadonly" plugin:

~~~ bash
yum install yum-plugin-downloadonly --nogpgcheck
~~~

Use following yum command with "--downloadonly" option as follows:

~~~ bash
yum install --downloadonly --downloaddir=<directory> <package>
~~~

Or use the following command to download all dependencies:

~~~ bash
sudo yum install --installroot=</path/to/tmp_dir> --downloadonly --downloaddir=<rpm_dir> <package>
~~~


Notes:

- Before using the plugin, check /etc/yum/pluginconf.d/downloadonly.conf to confirm that this plugin is "enabled=1"
- This is applicable for "yum install/yum update" and not for "yum groupinstall". Use "yum groupinfo" to identify packages within a specific group.
- If only the package name is specified, the latest available package is downloaded (such as sshd). Otherwise, you can specify the full package name and version (such as httpd-2.2.3-22.el5).
- If you do not use the --downloaddir option, files are saved by default in /var/cache/yum/ in rhel-{arch}-channel/packages
- If desired, you can download multiple packages on the same command.
- You still need to re-download the repodata if the repodata expires before you re-use the cache. By default it takes two hours to expire.


## RPM

RPM is a Package Manager for Red Hat, Suse and Fedora Linux.
It can be used to build, install, query, verify, update, and remove/erase individual software packages.

|-------------------------------+-------------
| Syntax						| Description
|-------------------------------|-------------
| rpm -ivh {rpm-file}	 		| Install the package
| rpm -Uvh {rpm-file}			| Upgrade package
| rpm -ev {package}			 	| Erase/remove/ an installed package
| rpm -ev --nodeps {package}	| Erase/remove/ an installed package without checking for dependencies
| rpm -qa 						| Display list all installed packages
| rpm -qi {package}				| Display installed information along with package version and short description
| rpm -qf {/path/to/file}		| Find out what package a file belongs to i.e. find what package owns the file
| rpm -qc {pacakge-name} 		| Display list of configuration file(s) for a package
| rpm -qcf {/path/to/file}		| Display list of configuration files for a command
| rpm -qa --last				| Display list of all recently installed RPMs
|-------------------------------+-------------------------------------------------
| rpm -qpR {.rpm-file}			|
| rpm -qR {package}				| 	Find out what dependencies a rpm file has
| --------------------------------------------------------------------------------