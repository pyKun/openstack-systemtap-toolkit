# openstack-systemtap-toolkit
SystemTap Toolkits for OpenStack

## Background
Since nearly all of OpenStack projects are based on Python and official community CI runs Ubuntu as host, the toolkits here are known or unknown tricks of tracing Python via SystemTap.

## Installation
Keyword: Ubuntu 14.04, Python, SystemTap

Util I start this work, Ubuntu 14.04 is the latest LTS version. However the latest systemtap package from official repo is pretty old (2.3 in Trusty which built about two years ago[1][2]). So we MUST build systemtap from source and systemtap community recommend we build elfutils from source too (see [3] for reasons).

Another trouble is to enable dtrace functionalities in Python. Most patches of enabling dtrace/systemtap probes are not merged or not accepted, so we need apply a patch before real building.

Last obvious work is to download debuginfo image of kernel from http://ddebs.ubuntu.com/ which is anothor official repo.

Making a conclusion here: if you plan to use SystemTap in OpenStack, you MUST build cPython yourself, build SystemTap&elfutils yourself and install a matched kernel version.

#### Steps
Upgrade Kernel and install its debuginfo package:

    sudo apt-search kernel-image # find a lastest one, 3.19.0-30-generic for example
    sudo apt-get install linux-image-3.19.0-30-generic linux-headers-3.19.0-30 linux-headers-3.19.0-30-generic
    # reboot your system to ensure you are running the new kernel 
    codename=$(lsb_release -c | awk  '{print $2}')
    sudo tee /etc/apt/sources.list.d/ddebs.list << EOF
    deb http://ddebs.ubuntu.com/ ${codename}      main restricted universe multiverse
    deb http://ddebs.ubuntu.com/ ${codename}-security main restricted universe multiverse
    deb http://ddebs.ubuntu.com/ ${codename}-updates  main restricted universe multiverse
    deb http://ddebs.ubuntu.com/ ${codename}-proposed main restricted universe multiverse
    EOF
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ECDCAD72428D7C01
    sudo apt-get update
    sudo apt-get install linux-image-$(uname -r)-dbgsym
    # reboot again
    
Download, patch and build cPython:

    cd ~
    git clone https://github.com/python/cpython
    wget https://www.jcea.es/artic/python_dtrace-2_7_9_05d8fd4c57a1.txt -O python-dtrace-2.7.9.patch
    cd cpython
    git checkout 2.7
    git checkout ad609d460a207bc12ca83b43ab764ea58bd013ab # the release of 2.7.9
    git apply ../python-dtrace-2.7.9.patch
    autoconf
    ./configure --with-dtrace
    make
    make install
    ln -s ~/cpython/python /usr/bin/python
    
Build SystemTap and elfutils:

    cd ~
    wget https://sourceware.org/systemtap/ftp/releases/systemtap-2.8.tar.gz
    wget https://fedorahosted.org/releases/e/l/elfutils/0.163/elfutils-0.163.tar.bz2
    tar jxf elfutils-0.163.tar.bz2
    tar zxf systemtap-2.8.tar.gz 
    cd systemtap-2.8/
    ./configure  '--with-elfutils=/home/hk/elfutils-0.163' --prefix=/home/hk/systemtap-2.8-21886
    make
    make install
    ln -s ~/systemtap-2.8-21886/bin/stap /usr/bin/stap

## Thanks
A lot of thanks to jcea's cpython patches[4].

## Reference
1. https://launchpad.net/ubuntu/+source/systemtap
2. https://sourceware.org/systemtap/ftp/releases/
3. https://sourceware.org/git/?p=systemtap.git;a=blob_plain;f=README;hb=HEAD
4. https://www.jcea.es/artic/python_dtrace.htm


