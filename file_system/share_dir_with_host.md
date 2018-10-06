
how to share files and directories between host machine and unix-like vm instances;

sources:
```
# install VM tools
https://kb.vmware.com/s/article/1022525

# use vmhgfs and vmware-hgfsclient
https://communities.vmware.com/thread/579226

# unmount:
https://tecadmin.net/mount-and-unmount-filesystem-in-linux/
```


create a shared directory entry
-------------------------------

on host (gunship), the directory is ```/home/wein/dude```; on vm the directory is ~/host_dude

vm has to explicitly mount this location


install VM tools on vm
----------------------

the goal is to have vmware-hgfsclient and vmhgfs-fuse running in the vm instance

RMB click on vm item -> install VM tools

switch to root user

for Lubuntu 17.10.1
```
mount /dev/sr0 /mnt/cdrom
```

for tiny core linux
```
mount /dev/cdrom /mnt/cdrom
```

unpack and execute the installer

this requires perl and ejection tool (tiny core does not have this tool therefore can not complete the installation step.
```
cd /mnt/cdrom
tar xzvf /mnt/cdrom/VMwareTools-x.x.x-xxxx.tar.gz -C /tmp/
cd /tmp/vmware...
./vmware-install.pl
```

use default options

unmount cdrom:
```
umount /mnt/cdrom
```


mount shared directory
----------------------

I created an alias for that:

alias mountstuff="sudo vmhgfs-fuse -o nonempty -o allow_other .host:/dude /home/wein/host_dude"





