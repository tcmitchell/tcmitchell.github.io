---
title: RPM
layout: index
---
# As root
Install stuff:

```
# get up to date
yum update -y

# Some convenience packages
yum install -y emacs-nox wget

# RPM related packages
yum install -y rpm-build rpmdevtools rpmlint
yum groupinstall -y "Development Tools"

# Reboot so the new changes take hold
reboot
```

Create a non-root user if you don't already have one:

```
/usr/sbin/useradd makerpm
usermod -a -G mock makerpm
passwd makerpm
```

# As a regular (non-root) user
Configure for building RPMs:

```
rpmdev-setuptree
```

Get the hello tarball and the my version of the spec file

```
cd "${HOME}"/rpmbuild/SOURCES
wget http://ftp.gnu.org/gnu/hello/hello-2.9.tar.gz

cd "${HOME}"/rpmbuild/SPECS
wget http://www.gpolab.bbn.com/experiment-support/images/tom/hello.spec
```

Make the package:

```
cd "${HOME}"/rpmbuild/SPECS
rpmbuild -ba hello.spec > rpmbuild.log 2>&1

 # Now view the log file rpmbuild.log
```

Check the package:

```
cd "${HOME}"/rpmbuild/SPECS

# rpmlint <Spec File> <Source RPM> <Binary RPM>
rpmlint hello.spec ../SRPMS/hello-2.9-1.el7.centos.src.rpm ../RPMS/x86_64/hello-2.9-1.el7.centos.x86_64.rpm

# Or use -i for extra info on warnings:
rpmlint -i hello.spec <Source RPM> <Binary RPM>
```

# Other useful command
## rpmdev-wipetree 
Erase all files within dirs created by `rpmdev-setuptree`:

```
$ rpmdev-wipetree
```
