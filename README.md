conntrack_table_memory
======================

Requires: python >= 2.7, libnetfilter-conntrack-dev

    usage: conntrack_table_memory [-h] [--output {conntrack_max,hashsize}]
                                  mebibytes

    Given a particular memory size, examine the size of the nf_conntrack struct
    and determine how to set nf_conntrack_max appropriately. Also outputs the
    recommended hashsize parameter for the nf_conntrack module. Michael Fincham
    <michael.fincham@catalyst.net.nz>

    positional arguments:
      mebibytes             desired maximum kernel memory consumption in MiB

    optional arguments:
      -h, --help            show this help message and exit
      --output {conntrack_max,hashsize}
                            output just the requested value and nothing else

Example
-------

    fincham:~/Working/sysadmin-tools/conntrack$ ./conntrack_table_memory 256
    On this machine, each conntrack entry requires 328 bytes of kernel memory, and each hash table entry requires 16.

    Therefore to consume a maximum of 256 MiB of kernel memory:
     - conntrack_max should be set to 813440
     - Using the kernel's default ratio, the nf_conntrack module's `hashsize' parameter should be set to 101680

    root:~/Working/sysadmin-tools/conntrack# ./conntrack_table_memory --output conntrack_max 256 > /proc/sys/net/netfilter/nf_conntrack_max
    root:~/Working/sysadmin-tools/conntrack# ./conntrack_table_memory --output hashsize 256 > /sys/module/nf_conntrack/parameters/hashsize
    root:~/Working/sysadmin-tools/conntrack# cat /proc/sys/net/netfilter/nf_conntrack_max
    813440
    root:~/Working/sysadmin-tools/conntrack# cat /sys/module/nf_conntrack/parameters/hashsize
    101888

