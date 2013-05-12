# vagrant-cluster

Creates a cluster using a YAML configuration file. Provides node defaults in the `default` stanza and reputable boxes for CentOS, Debian, and Ubuntu in the `boxes` stanza. Defaults can be overridden on a per-node basis.

After `vagrant up` you can confirm the configuration of the running VMs with:

    $ VBoxManage list runningvms -l | egrep 'Name:|Guest OS|Memory|CPUs'
    Name:            node01
    Guest OS:        Red Hat (64 bit)
    Memory size:     768MB
    Number of CPUs:  2
    Name:            node02
    Guest OS:        Debian (64 bit)
    Memory size:     512MB
    Number of CPUs:  1
    Name:            node03
    Guest OS:        Debian (64 bit)
    Memory size:     512MB
    Number of CPUs:  1
    Name:            node04
    Guest OS:        Debian (64 bit)
    Memory size:     512MB
    Number of CPUs:  1

    $ for i in `VBoxManage list runningvms | awk -F\" '{print $NF}'`; do VBoxManage guestproperty enumerate $i --patterns '*/1/V4/IP'; done
    Name: /VirtualBox/GuestInfo/Net/1/V4/IP, value: 10.100.100.11, timestamp: 1368371532459386000, flags:
    Name: /VirtualBox/GuestInfo/Net/1/V4/IP, value: 10.100.100.12, timestamp: 1368371607135977000, flags:
    Name: /VirtualBox/GuestInfo/Net/1/V4/IP, value: 10.100.100.13, timestamp: 1368371675567183000, flags:
    Name: /VirtualBox/GuestInfo/Net/1/V4/IP, value: 10.100.100.14, timestamp: 1368371741239213000, flags:

## Prerequisites
Uses the <a href="https://github.com/adrienthebo/vagrant-hosts">vagrant-hosts</a> plugin for local DNS resolution on the guest VMs. Install the plugin with:

    vagrant plugin install vagrant-hosts

## Supported Platforms
Vagrant version 2 (1.1.x or higher) is required.

## TODO
Update to use <a href="https://github.com/mosaicxm/vagrant-hostmaster">vagrant-hostmaster</a> once that plugin supports version 2. This will allow DNS resolution to work on the host OS as well.
