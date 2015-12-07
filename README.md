This role enables the capture of kernel crash dumps.  Currently a reboot is needed to fully enable
crash dump capture, but this role does not reboot the target host(s) unless allow_reboot is set and
a reboot is actually needed.  So, for example, re-running against a box which already has crash
dumping enabled does not reboot the box unnecessarily.

Inputs
------

- allow_reboot: boolean, defaults to false.  If true, this role reboots the box if a reboot is
  necessary to enable kdump functionality.


Testing
-------

        cd it_ansible
        vagrant up it-ansible
        ansible-playbook -i inventory/vagrant.inv roles/kdump/test.yml


Monitoring
----------

This is good:

        root@it-ansible:~# if [ "$(kdump-config status)" == "current state   : ready to kdump" ]; then echo "status is GOOD"; else "echo status is BAD"; fi
        status is GOOD
        root@it-ansible:~# kdump-config status
        current state   : ready to kdump
        root@it-ansible:~# echo $?
        0

This is bad:

        root@it-ansible:~# kdump-config status
        current state   : Not ready to kdump
        root@it-ansible:~# echo $?
        0

Unfortunately the kdump-config exit code cannot always be used to distinguish good and bad.  If
kdump-config returns a non-zero exit code, however, or the kdump-config command is not found, this
is always bad.


References
----------

- [https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)
- [http://www.dedoimedo.com/computers/kdump.html](http://www.dedoimedo.com/computers/kdump.html)
