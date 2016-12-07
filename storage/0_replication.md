There are some issues with the BDR. Destination does not show the replication site.

Below is the distcp command

'''
$ cd /opt/cloudera/parcels/CDH-5.8.3-1.cdh5.8.3.p0.2/share/doc/hadoop-0.20-mapreduce/examples
$ HADOOP_USER_NAME=hdfs hadoop jar hadoop-examples.jar teragen 5242880 /user/hdfs/snowbite
$ HADOOP_USER_NAME=hdfs hdfs dfs -ls hdfs://172.31.13.100/user/hdfs/snowbite

16/12/07 02:51:40 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-13-100.ap-southeast-1.compute.internal/172.31.13.100:8032
16/12/07 02:51:41 INFO terasort.TeraSort: Generating 5242880 using 2
16/12/07 02:51:41 INFO mapreduce.JobSubmitter: number of splits:2
16/12/07 02:51:42 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1481045112286_0003
16/12/07 02:51:42 INFO impl.YarnClientImpl: Submitted application application_1481045112286_0003
16/12/07 02:51:42 INFO mapreduce.Job: The url to track the job: http://ip-172-31-13-100.ap-southeast-1.compute.internal:8088/proxy/application_1481045112286_0003/
16/12/07 02:51:42 INFO mapreduce.Job: Running job: job_1481045112286_0003
16/12/07 02:51:48 INFO mapreduce.Job: Job job_1481045112286_0003 running in uber mode : false
16/12/07 02:51:48 INFO mapreduce.Job:  map 0% reduce 0%
16/12/07 02:51:59 INFO mapreduce.Job:  map 23% reduce 0%
16/12/07 02:52:01 INFO mapreduce.Job:  map 52% reduce 0%
16/12/07 02:52:03 INFO mapreduce.Job:  map 63% reduce 0%
16/12/07 02:52:04 INFO mapreduce.Job:  map 100% reduce 0%
16/12/07 02:52:05 INFO mapreduce.Job: Job job_1481045112286_0003 completed successfully
16/12/07 02:52:05 INFO mapreduce.Job: Counters: 31
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=244350
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=167
                HDFS: Number of bytes written=524288000
                HDFS: Number of read operations=8
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=4
        Job Counters
                Launched map tasks=2
                Other local map tasks=2
                Total time spent by all maps in occupied slots (ms)=26475
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=26475
                Total vcore-seconds taken by all map tasks=26475
                Total megabyte-seconds taken by all map tasks=27110400
        Map-Reduce Framework
                Map input records=5242880
                Map output records=5242880
                Input split bytes=167
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=172
                CPU time spent (ms)=14300
                Physical memory (bytes) snapshot=719953920
                Virtual memory (bytes) snapshot=3154223104
                Total committed heap usage (bytes)=797442048
        org.apache.hadoop.examples.terasort.TeraGen$Counters
                CHECKSUM=11257830824958050
        File Input Format Counters
                Bytes Read=0
        File Output Format Counters
                Bytes Written=524288000
				
				
				
$ HADOOP_USER_NAME=hdfs hadoop distcp hdfs://172.31.13.100/user/hdfs/snowbite hdfs://172.31.5.137/user/snowbite
16/12/07 03:04:34 INFO tools.DistCp: Input Options: DistCpOptions{atomicCommit=false, syncFolder=false, deleteMissing=false, ignoreFailures=false, overwrite=false, skipCRC=false, blocking=true, numListstatusThreads=0, maxMaps=20, mapBandwidth=100, sslConfigurationFile='null', copyStrategy='uniformsize', preserveStatus=[], preserveRawXattrs=false, atomicWorkPath=null, logPath=null, sourceFileListing=null, sourcePaths=[hdfs://172.31.13.100/user/hdfs/snowbite], targetPath=hdfs://172.31.5.137/user/snowbite, targetPathExists=false, filtersFile='null'}
16/12/07 03:04:34 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-13-100.ap-southeast-1.compute.internal/172.31.13.100:8032
16/12/07 03:04:48 INFO tools.SimpleCopyListing: Paths (files+dirs) cnt = 4; dirCnt = 1
16/12/07 03:04:48 INFO tools.SimpleCopyListing: Build file listing completed.
16/12/07 03:04:48 INFO Configuration.deprecation: io.sort.mb is deprecated. Instead, use mapreduce.task.io.sort.mb
16/12/07 03:04:48 INFO Configuration.deprecation: io.sort.factor is deprecated. Instead, use mapreduce.task.io.sort.factor
16/12/07 03:04:48 INFO tools.DistCp: Number of paths in the copy list: 4
16/12/07 03:04:48 INFO tools.DistCp: Number of paths in the copy list: 4
16/12/07 03:04:48 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-13-100.ap-southeast-1.compute.internal/172.31.13.100:8032
16/12/07 03:04:48 INFO mapreduce.JobSubmitter: number of splits:3
16/12/07 03:04:49 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1481045112286_0004
16/12/07 03:04:49 INFO impl.YarnClientImpl: Submitted application application_1481045112286_0004
16/12/07 03:04:49 INFO mapreduce.Job: The url to track the job: http://ip-172-31-13-100.ap-southeast-1.compute.internal:8088/proxy/application_1481045112286_0004/
16/12/07 03:04:49 INFO tools.DistCp: DistCp job-id: job_1481045112286_0004
16/12/07 03:04:49 INFO mapreduce.Job: Running job: job_1481045112286_0004
16/12/07 03:05:26 INFO mapreduce.Job: Job job_1481045112286_0004 running in uber mode : false
16/12/07 03:05:26 INFO mapreduce.Job:  map 0% reduce 0%
16/12/07 03:05:28 INFO mapreduce.Job:  map 33% reduce 0%
16/12/07 03:05:33 INFO mapreduce.Job:  map 67% reduce 0%
16/12/07 03:05:39 INFO mapreduce.Job:  map 100% reduce 0%
16/12/07 03:06:00 INFO mapreduce.Job: Job job_1481045112286_0004 completed successfully
16/12/07 03:06:00 INFO mapreduce.Job: Counters: 33
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=375006
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=524289561
                HDFS: Number of bytes written=524288000
                HDFS: Number of read operations=56
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=13
        Job Counters
                Launched map tasks=3
                Other local map tasks=3
                Total time spent by all maps in occupied slots (ms)=32729
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=32729
                Total vcore-seconds taken by all map tasks=32729
                Total megabyte-seconds taken by all map tasks=33514496
        Map-Reduce Framework
                Map input records=4
                Map output records=0
                Input split bytes=345
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=189
                CPU time spent (ms)=11440
                Physical memory (bytes) snapshot=677388288
                Virtual memory (bytes) snapshot=4756922368
                Total committed heap usage (bytes)=1053818880
        File Input Format Counters
                Bytes Read=1216
        File Output Format Counters
                Bytes Written=0
        org.apache.hadoop.tools.mapred.CopyMapper$Counter
                BYTESCOPIED=524288000
                BYTESEXPECTED=524288000
                COPY=4
				
				
$ HADOOP_USER_NAME=hdfs hdfs dfs -ls hdfs://172.31.5.137/user/snowbite
Found 3 items
-rw-r--r--   3 hdfs supergroup          0 2016-12-07 03:05 hdfs://172.31.5.137/user/snowbite/_SUCCESS
-rw-r--r--   3 hdfs supergroup  262144000 2016-12-07 03:05 hdfs://172.31.5.137/user/snowbite/part-m-00000
-rw-r--r--   3 hdfs supergroup  262144000 2016-12-07 03:05 hdfs://172.31.5.137/user/snowbite/part-m-00001


$ HADOOP_USER_NAME=hdfs hdfs fsck /user/hdfs/snowbite -files -blocks
Connecting to namenode via http://ip-172-31-13-100.ap-southeast-1.compute.internal:50070
FSCK started by hdfs (auth:SIMPLE) from /172.31.13.100 for path /user/hdfs/snowbite at Wed Dec 07 03:11:58 UTC 2016
/user/hdfs/snowbite <dir>
/user/hdfs/snowbite/_SUCCESS 0 bytes, 0 block(s):  OK

/user/hdfs/snowbite/part-m-00000 262144000 bytes, 2 block(s):  OK
0. BP-790346808-172.31.13.100-1481015016604:blk_1073743602_2778 len=134217728 Live_repl=3
1. BP-790346808-172.31.13.100-1481015016604:blk_1073743605_2781 len=127926272 Live_repl=3

/user/hdfs/snowbite/part-m-00001 262144000 bytes, 2 block(s):  OK
0. BP-790346808-172.31.13.100-1481015016604:blk_1073743603_2779 len=134217728 Live_repl=3
1. BP-790346808-172.31.13.100-1481015016604:blk_1073743604_2780 len=127926272 Live_repl=3

Status: HEALTHY
 Total size:    524288000 B
 Total dirs:    1
 Total files:   3
 Total symlinks:                0
 Total blocks (validated):      4 (avg. block size 131072000 B)
 Minimally replicated blocks:   4 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          4
 Number of racks:               1
FSCK ended at Wed Dec 07 03:11:58 UTC 2016 in 3 milliseconds


$ HADOOP_USER_NAME=hdfs hdfs fsck /user/snowbite -files -blocks
Connecting to namenode via http://ip-172-31-5-137.ap-southeast-1.compute.internal:50070
FSCK started by hdfs (auth:SIMPLE) from /172.31.5.139 for path /user/snowbite at Wed Dec 07 03:31:47 UTC 2016
/user/snowbite <dir>
/user/snowbite/_SUCCESS 0 bytes, 0 block(s):  OK

/user/snowbite/part-m-00000 262144000 bytes, 2 block(s):  OK
0. BP-1146240190-172.31.5.137-1481019106690:blk_1073743056_2232 len=134217728 Live_repl=3
1. BP-1146240190-172.31.5.137-1481019106690:blk_1073743058_2234 len=127926272 Live_repl=3

/user/snowbite/part-m-00001 262144000 bytes, 2 block(s):  OK
0. BP-1146240190-172.31.5.137-1481019106690:blk_1073743055_2231 len=134217728 Live_repl=3
1. BP-1146240190-172.31.5.137-1481019106690:blk_1073743057_2233 len=127926272 Live_repl=3

Status: HEALTHY
 Total size:    524288000 B
 Total dirs:    1
 Total files:   3
 Total symlinks:                0
 Total blocks (validated):      4 (avg. block size 131072000 B)
 Minimally replicated blocks:   4 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          3
 Number of racks:               1
FSCK ended at Wed Dec 07 03:31:47 UTC 2016 in 2 milliseconds


The filesystem under path '/user/snowbite' is HEALTHY


'''