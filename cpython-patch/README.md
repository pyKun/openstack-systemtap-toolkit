#Background

These patches are Jcea's work (see https://www.jcea.es/artic/python_dtrace.htm). Making a copy here is to avoid the lost of his work. I'm using python_dtrace-2_7_9-05d8fd4c57a1.patch in my cPython.

## Patch for OpenStack

If you need systemtap works in OpenStack, only "--with-dtrace" is not enough. For example, lib sqlite3.so will not be compiled by default, so you have to apply a patch to build sqlite3 library. Ubuntu and Redhat both have about 60 patches for each native python2.7 package. You'd better have a look at these links[1][2]

## My works

I'm working on making related marker to retrieve calling args, kwargs and return values, or hack calling data in runtime, dynamically. Based on Jcea's work, one test patch is [here](https://github.com/pyKun/openstack-systemtap-toolkit/blob/master/cpython-patch/python_dtrace-2_7_9-enhanced.patch). It fetches the args and kwargs during calling a python-defined function (not c function). And the related marker(function__entry) becomes to return: file name, function name, line number, calling arguments and key word arguments.

## Reference

1. http://bazaar.launchpad.net/~ubuntu-branches/ubuntu/trusty/python2.7/trusty/files/head:/debian/patches/
2. https://github.com/nmilford/rpm-python27/tree/master/rpmbuild/SOURCES
