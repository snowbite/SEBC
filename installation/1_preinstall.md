
```
$ sudo bash -c "echo 'vm.swappiness = 1' >> /etc/sysctl.conf"
$ sudo sysctl -p
vm.swappiness = 1


$ cat /proc/mounts
rootfs / rootfs rw 0 0
sysfs /sys sysfs rw,seclabel,nosuid,nodev,noexec,relatime 0 0
proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
devtmpfs /dev devtmpfs rw,seclabel,nosuid,size=7599028k,nr_inodes=1899757,mode=755 0 0
securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
tmpfs /dev/shm tmpfs rw,seclabel,nosuid,nodev 0 0
devpts /dev/pts devpts rw,seclabel,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
tmpfs /run tmpfs rw,seclabel,nosuid,nodev,mode=755 0 0
tmpfs /sys/fs/cgroup tmpfs ro,seclabel,nosuid,nodev,noexec,mode=755 0 0
cgroup /sys/fs/cgroup/systemd cgroup rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd 0 0
pstore /sys/fs/pstore pstore rw,nosuid,nodev,noexec,relatime 0 0
cgroup /sys/fs/cgroup/blkio cgroup rw,nosuid,nodev,noexec,relatime,blkio 0 0
cgroup /sys/fs/cgroup/hugetlb cgroup rw,nosuid,nodev,noexec,relatime,hugetlb 0 0
cgroup /sys/fs/cgroup/cpu,cpuacct cgroup rw,nosuid,nodev,noexec,relatime,cpuacct,cpu 0 0
cgroup /sys/fs/cgroup/freezer cgroup rw,nosuid,nodev,noexec,relatime,freezer 0 0
cgroup /sys/fs/cgroup/memory cgroup rw,nosuid,nodev,noexec,relatime,memory 0 0
cgroup /sys/fs/cgroup/net_cls cgroup rw,nosuid,nodev,noexec,relatime,net_cls 0 0
cgroup /sys/fs/cgroup/cpuset cgroup rw,nosuid,nodev,noexec,relatime,cpuset 0 0
cgroup /sys/fs/cgroup/perf_event cgroup rw,nosuid,nodev,noexec,relatime,perf_event 0 0
cgroup /sys/fs/cgroup/devices cgroup rw,nosuid,nodev,noexec,relatime,devices 0 0
configfs /sys/kernel/config configfs rw,relatime 0 0
/dev/xvda1 / xfs rw,seclabel,relatime,attr2,inode64,noquota 0 0
rpc_pipefs /var/lib/nfs/rpc_pipefs rpc_pipefs rw,relatime 0 0
selinuxfs /sys/fs/selinux selinuxfs rw,relatime 0 0
systemd-1 /proc/sys/fs/binfmt_misc autofs rw,relatime,fd=30,pgrp=1,timeout=300,minproto=5,maxproto=5,direct 0 0
debugfs /sys/kernel/debug debugfs rw,relatime 0 0
mqueue /dev/mqueue mqueue rw,seclabel,relatime 0 0
hugetlbfs /dev/hugepages hugetlbfs rw,seclabel,relatime 0 0
nfsd /proc/fs/nfsd nfsd rw,relatime 0 0
tmpfs /run/user/1000 tmpfs rw,seclabel,nosuid,nodev,relatime,size=1497312k,mode=700,uid=1000,gid=1000 0 0
tmpfs /run/user/0 tmpfs rw,seclabel,nosuid,nodev,relatime,size=1497312k,mode=700 0 0


$ df
Filesystem     1K-blocks   Used Available Use% Mounted on
/dev/xvda1      31444004 897948  30546056   3% /
devtmpfs         7599028      0   7599028   0% /dev
tmpfs            7486540      0   7486540   0% /dev/shm
tmpfs            7486540  16652   7469888   1% /run
tmpfs            7486540      0   7486540   0% /sys/fs/cgroup
tmpfs            1497312      0   1497312   0% /run/user/1000
tmpfs            1497312      0   1497312   0% /run/user/0



$ cat /sys/kernel/mm/transparent_hugepage/enabled
[always] madvise never
$ cat /sys/kernel/mm/transparent_hugepage/defrag
[always] madvise never

Add the following to the bottom of /etc/rc.d/rc.local
------------------------------------------------------------------------
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
echo never > /sys/kernel/mm/transparent_hugepage/defrag
fi


$ chmod u+x /etc/rc.d/rc.local

$ cat /sys/kernel/mm/transparent_hugepage/defrag
always madvise [never]
$ cat /sys/kernel/mm/transparent_hugepage/enabled
always madvise [never]







$ ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 02:86:af:0e:f3:69 brd ff:ff:ff:ff:ff:ff
	

$ /sbin/ifconfig -a
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.13.101  netmask 255.255.240.0  broadcast 172.31.15.255
        inet6 fe80::86:afff:fe0e:f369  prefixlen 64  scopeid 0x20<link>
        ether 02:86:af:0e:f3:69  txqueuelen 1000  (Ethernet)
        RX packets 307  bytes 33617 (32.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 373  bytes 40428 (39.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 0  (Local Loopback)
        RX packets 6  bytes 416 (416.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 6  bytes 416 (416.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
		
		
$ sudo yum install bind-utils
		
$ nslookup ip-172-31-13-101.ap-southeast-1.compute.internal
Server:         172.31.0.2
Address:        172.31.0.2#53

Non-authoritative answer:
Name:   ip-172-31-13-101.ap-southeast-1.compute.internal
Address: 172.31.13.101


$ getent hosts ip-172-31-13-101.ap-southeast-1.compute.internal
172.31.13.101   ip-172-31-13-101.ap-southeast-1.compute.internal

$ getent hosts 172.31.13.101
172.31.13.101   ip-172-31-13-101.ap-southeast-1.compute.internal

$ getent hosts 54.251.144.156
54.251.144.156  ec2-54-251-144-156.ap-southeast-1.compute.amazonaws.com


$ sudo service nscd status
Redirecting to /bin/systemctl status  nscd.service
? nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2016-12-05 09:28:55 UTC; 1min 7s ago
  Process: 2496 ExecStart=/usr/sbin/nscd $NSCD_OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 2497 (nscd)
   CGroup: /system.slice/nscd.service
           +-2497 /usr/sbin/nscd

Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring file `/etc/hosts` (4)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring directory `/etc` (2)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring file `/etc/resolv.conf` (5)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring directory `/etc` (2)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring file `/etc/services` (6)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring directory `/etc` (2)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring file `/etc/netgroup` (7)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 monitoring directory `/etc` (2)
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal nscd[2497]: 2497 Access Vector Cache (AVC) started
Dec 05 09:28:55 ip-172-31-13-101.ap-southeast-1.compute.internal systemd[1]: Started Name Service Cache Daemon.





$ sudo yum install ntp ntpdate ntp-doc

$ sudo chkconfig ntpd on

$ sudo service ntpd start

$ $ sudo service ntpd status
Redirecting to /bin/systemctl status  ntpd.service
? ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2016-12-05 09:43:07 UTC; 5s ago
  Process: 2698 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 2699 (ntpd)
   CGroup: /system.slice/ntpd.service
           +-2699 /usr/sbin/ntpd -u ntp:ntp -g

Dec 05 09:43:07 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: Listen normally on 2 lo 127.0.0.1 UDP 123
Dec 05 09:43:07 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: Listen normally on 3 eth0 172.31.13.101 UDP 123
Dec 05 09:43:07 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: Listen normally on 4 lo ::1 UDP 123
Dec 05 09:43:07 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: Listen normally on 5 eth0 fe80::86:afff:fe0e:f369 UDP 123
Dec 05 09:43:07 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: Listening on routing socket on fd #22 for interface updates
Dec 05 09:43:07 ip-172-31-13-101.ap-southeast-1.compute.internal systemd[1]: Started Network Time Service.
Dec 05 09:43:08 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: 0.0.0.0 c016 06 restart
Dec 05 09:43:08 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: 0.0.0.0 c012 02 freq_set kernel 0.000 PPM
Dec 05 09:43:08 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: 0.0.0.0 c011 01 freq_not_set
Dec 05 09:43:09 ip-172-31-13-101.ap-southeast-1.compute.internal ntpd[2699]: 0.0.0.0 c614 04 freq_mode
```