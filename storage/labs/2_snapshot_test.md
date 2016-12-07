$hdfs dfs -mkdir /user/snowbite/precious

Need SuperUser to allow snaphot
```
$ HADOOP_USER_NAME=hdfs hdfs dfsadmin -allowSnapshot /user/snowbite/precious
Allowing snaphot on /user/snowbite/precious succeeded
```

Upload Testfile
```
$ hdfs dfs -put test.txt /user/snowbite/precious
$ hdfs dfs -ls /user/snowbite/precious
Found 1 items
-rw-r--r--   3 snowbite supergroup         20 2016-12-07 15:28 /user/snowbite/precious/test.txt
```

```
$ hdfs dfs -createSnapshot /user/snowbite/precious sebc-hdfs-test
Created snapshot /user/snowbite/precious/.snapshot/sebc-hdfs-test
```

```
$ hdfs dfs -rm /user/snowbite/precious/test.txt
16/12/07 15:41:45 INFO fs.TrashPolicyDefault: Moved: 'hdfs://ip-172-31-13-100.ap-southeast-1.compute.internal:8020/user/snowbite/precious/test.txt' to trash at: hdfs://ip-172-31-13-100.ap-southeast-1.compute.internal:8020/user/snowbite/.Trash/Current/user/snowbite/precious/test.txt
[snowbite@ip-172-31-13-100 ~]$ hdfs dfs -ls /user/snowbite/precious
```

Note that this example uses the preserve option to preserve timestamps, ownership, permission, ACLs and XAttrs.

```
hdfs dfs -cp -ptopax /user/snowbite/precious/.snapshot/sebc-hdfs-test /user/snowbite/precious
$ HADOOP_USER_NAME=hdfs hdfs dfs -cp -ptopax /user/snowbite/precious/.snapshot/sebc-hdfs-test/test.txt /user/snowbite/precious/.
$ hdfs dfs -ls /user/snowbite/precious
Found 1 items
-rw-r--r--   3 snowbite supergroup         20 2016-12-07 15:28 /user/snowbite/precious/test.txt
```

