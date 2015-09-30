# openstack-systemtap-toolkit
SystemTap toolkits for OpenStack

## Background
Since nearly all of OpenStack projects are based on Python and official community CI runs Ubuntu as host, the toolkits here are known or unknown tricks of tracing Python via SystemTap.

## Installation
Keyword: Ubuntu 14.04, Python, SystemTap

Util I start this work, Ubuntu 14.04 is the latest LTS version. However the latest systemtap package from official repo is pretty old (2.3 in Trusty which built about two years ago[1][2]). So we MUST build systemtap from source and systemtap community recommend we build elfutils from source too (see [3] for reasons).

Another trouble is to enable dtrace functionalities in Python. Most patches of enabling dtrace/systemtap probes are not merged or not accepted, so we need apply a patch before real building.

Last obvious work is to download debuginfo image of kernel from ddebs.ubuntu.com which is anothor official repo.

Making a conclusion here: if you plan to use SystemTap in OpenStack, you MUST build cPython yourself, build SystemTap&elfutils yourself and install a matched kernel version.

#### Steps

## Reference
1. https://launchpad.net/ubuntu/+source/systemtap
2. https://sourceware.org/systemtap/ftp/releases/
3. https://sourceware.org/git/?p=systemtap.git;a=blob_plain;f=README;hb=HEAD
