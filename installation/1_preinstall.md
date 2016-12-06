1. Check vm.swappiness on all your nodes

```
$ cat /proc/sys/vm/swappiness
30

$ sudo bash -c "echo 'vm.swappiness = 1' >> /etc/sysctl.conf"
$ sudo sysctl -p
vm.swappiness = 1
```

Modified the vm.swappiness to 1 in virtual-guest/tuned.conf

```
$ sudo vi /usr/lib/tuned/virtual-guest/tuned.conf
$ sudo reboot

$ cat /proc/sys/vm/swappiness
1
```

2. Show the mount attributes of all volumes

```
$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime,seclabel)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
devtmpfs on /dev type devtmpfs (rw,nosuid,seclabel,size=7599028k,nr_inodes=1899757,mode=755)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,seclabel)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,seclabel,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,seclabel,mode=755)
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,seclabel,mode=755)
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpuacct,cpu)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup on /sys/fs/cgroup/net_cls type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
configfs on /sys/kernel/config type configfs (rw,relatime)
/dev/xvda1 on / type xfs (rw,relatime,seclabel,attr2,inode64,noquota)
rpc_pipefs on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw,relatime)
selinuxfs on /sys/fs/selinux type selinuxfs (rw,relatime)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=28,pgrp=1,timeout=300,minproto=5,maxproto=5,direct)
debugfs on /sys/kernel/debug type debugfs (rw,relatime)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,seclabel)
mqueue on /dev/mqueue type mqueue (rw,relatime,seclabel)
nfsd on /proc/fs/nfsd type nfsd (rw,relatime)
tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=1497312k,mode=700,uid=1000,gid=1000)
```


3. Show the reserve space of any non-root, ext-based volumes


```
$ df
Filesystem     1K-blocks   Used Available Use% Mounted on
/dev/xvda1      31444004 898392  30545612   3% /
devtmpfs         7599028      0   7599028   0% /dev
tmpfs            7486540      0   7486540   0% /dev/shm
tmpfs            7486540  16620   7469920   1% /run
tmpfs            7486540      0   7486540   0% /sys/fs/cgroup
tmpfs            1497312      0   1497312   0% /run/user/1000
```

Verify again how to check

4. Show that transparent hugepages is disabled

```
$ cat /sys/kernel/mm/transparent_hugepage/enabled
[always] madvise never
$ cat /sys/kernel/mm/transparent_hugepage/defrag
[always] madvise never
```

Add the following to the bottom of /etc/rc.d/rc.local

```
sudo vi /etc/rc.d/rc.local
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
echo never > /sys/kernel/mm/transparent_hugepage/defrag
fi
```

```
$ sudo chmod u+x /etc/rc.d/rc.local
$ sudo reboot

$ cat /sys/kernel/mm/transparent_hugepage/defrag
always madvise [never]
$ cat /sys/kernel/mm/transparent_hugepage/enabled
always madvise [never]
```




5. Report the network interface attributes

```
$ ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 02:96:18:93:31:33 brd ff:ff:ff:ff:ff:ff


$ /sbin/ifconfig -a
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.13.100  netmask 255.255.240.0  broadcast 172.31.15.255
        inet6 fe80::96:18ff:fe93:3133  prefixlen 64  scopeid 0x20<link>
        ether 02:96:18:93:31:33  txqueuelen 1000  (Ethernet)
        RX packets 2629379  bytes 2680955636 (2.4 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1810783  bytes 5304068276 (4.9 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 0  (Local Loopback)
        RX packets 1360765  bytes 2928677677 (2.7 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1360765  bytes 2928677677 (2.7 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```



6. Show forward and reverse host lookups using getent and nslookup

```
$ sudo yum install bind-utils

$ hostname
ip-172-31-13-100.ap-southeast-1.compute.internal

$ nslookup ip-172-31-13-100.ap-southeast-1.compute.internal
Server:         172.31.0.2
Address:        172.31.0.2#53

Non-authoritative answer:
Name:   ip-172-31-13-100.ap-southeast-1.compute.internal
Address: 172.31.13.100


$ getent hosts ip-172-31-13-100.ap-southeast-1.compute.internal
172.31.13.100   ip-172-31-13-100.ap-southeast-1.compute.internal
```

```
$ sudo yum install nscd
$ sudo touch /etc/netgroup
$ sudo service nscd start
Redirecting to /bin/systemctl start  nscd.service

$ sudo service nscd status
Redirecting to /bin/systemctl status  nscd.service
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2016-12-05 16:09:22 UTC; 21s ago
  Process: 2238 ExecStart=/usr/sbin/nscd $NSCD_OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 2239 (nscd)
   CGroup: /system.slice/nscd.service
           └─2239 /usr/sbin/nscd

Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal nscd[2239]: 2...
Dec 05 16:09:22 ip-172-31-13-100.ap-southeast-1.compute.internal systemd[1]: S...
Hint: Some lines were ellipsized, use -l to show in full.

```


8. Verify the ntpd service is running

```
$ sudo yum install ntp ntpdate ntp-doc
$ sudo chkconfig ntpd on

$ sudo service ntpd start

$ sudo service ntpd status
Redirecting to /bin/systemctl status  ntpd.service
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2016-12-05 16:16:31 UTC; 30s ago
  Process: 2350 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 2351 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─2351 /usr/sbin/ntpd -u ntp:ntp -g

Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: L...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: L...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: L...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: L...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: L...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal systemd[1]: S...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: 0...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: 0...
Dec 05 16:16:31 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: 0...
Dec 05 16:16:32 ip-172-31-13-100.ap-southeast-1.compute.internal ntpd[2351]: 0...
Hint: Some lines were ellipsized, use -l to show in full.

```
