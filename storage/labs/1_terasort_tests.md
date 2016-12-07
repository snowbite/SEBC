```

$ sudo useradd snowbite
$ sudo passwd snowbite

$ HADOOP_USER_NAME=hdfs hdfs dfs -mkdir /user/snowbite
$ HADOOP_USER_NAME=hdfs hdfs dfs -chown snowbite /user/snowbite

$ su - snowbite

$ time /opt/cloudera/parcels/CDH/bin/hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce/hadoop-examples.jar teragen -Dmapreduce.job.maps=4 -Ddfs.block.size=33554432 107374182 /user/snowbite/teragen
16/12/07 14:58:30 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-13-100.ap-southeast-1.compute.internal/172.31.13.100:8032
16/12/07 14:58:31 INFO terasort.TeraSort: Generating 107374182 using 4
16/12/07 14:58:31 INFO mapreduce.JobSubmitter: number of splits:4
16/12/07 14:58:31 INFO Configuration.deprecation: dfs.block.size is deprecated. Instead, use dfs.blocksize
16/12/07 14:58:31 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1481045112286_0011
16/12/07 14:58:31 INFO impl.YarnClientImpl: Submitted application application_1481045112286_0011
16/12/07 14:58:32 INFO mapreduce.Job: The url to track the job: http://ip-172-31-13-100.ap-southeast-1.compute.internal:8088/proxy/application_1481045112286_0011/
16/12/07 14:58:32 INFO mapreduce.Job: Running job: job_1481045112286_0011
16/12/07 14:58:38 INFO mapreduce.Job: Job job_1481045112286_0011 running in uber mode : false
16/12/07 14:58:38 INFO mapreduce.Job:  map 0% reduce 0%
16/12/07 14:58:49 INFO mapreduce.Job:  map 1% reduce 0%
...
16/12/07 15:02:36 INFO mapreduce.Job:  map 97% reduce 0%
16/12/07 15:02:39 INFO mapreduce.Job:  map 98% reduce 0%
16/12/07 15:02:57 INFO mapreduce.Job:  map 99% reduce 0%
16/12/07 15:03:09 INFO mapreduce.Job:  map 100% reduce 0%
16/12/07 15:03:24 INFO mapreduce.Job: Job job_1481045112286_0011 completed successfully
16/12/07 15:03:24 INFO mapreduce.Job: Counters: 31
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=488812
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=344
                HDFS: Number of bytes written=10737418200
                HDFS: Number of read operations=16
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=8
        Job Counters
                Launched map tasks=4
                Other local map tasks=4
                Total time spent by all maps in occupied slots (ms)=961317
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=961317
                Total vcore-seconds taken by all map tasks=961317
                Total megabyte-seconds taken by all map tasks=984388608
        Map-Reduce Framework
                Map input records=107374182
                Map output records=107374182
                Input split bytes=344
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=1834
                CPU time spent (ms)=167520
                Physical memory (bytes) snapshot=642469888
                Virtual memory (bytes) snapshot=6301605888
                Total committed heap usage (bytes)=792199168
        org.apache.hadoop.examples.terasort.TeraGen$Counters
                CHECKSUM=230593859918397906
        File Input Format Counters
                Bytes Read=0
        File Output Format Counters
                Bytes Written=10737418200

real    4m57.658s
user    0m7.593s
sys     0m0.524s






$ time /opt/cloudera/parcels/CDH/bin/hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce/hadoop-examples.jar terasort /user/snowbite/teragen /user/snowbite/terasort
16/12/07 15:04:27 INFO terasort.TeraSort: starting
16/12/07 15:04:29 INFO input.FileInputFormat: Total input paths to process : 4
Spent 360ms computing base-splits.
Spent 12ms computing TeraScheduler splits.
Computing input splits took 373ms
Sampling 10 splits of 320
Making 8 from 100000 sampled records
Computing parititions took 1730ms
Spent 2106ms computing partitions.
16/12/07 15:04:31 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-13-100.ap-southeast-1.compute.internal/172.31.13.100:8032
16/12/07 15:04:32 INFO mapreduce.JobSubmitter: number of splits:320
16/12/07 15:04:32 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1481045112286_0012
16/12/07 15:04:32 INFO impl.YarnClientImpl: Submitted application application_1481045112286_0012
16/12/07 15:04:32 INFO mapreduce.Job: The url to track the job: http://ip-172-31-13-100.ap-southeast-1.compute.internal:8088/proxy/application_1481045112286_0012/
16/12/07 15:04:32 INFO mapreduce.Job: Running job: job_1481045112286_0012
16/12/07 15:04:40 INFO mapreduce.Job: Job job_1481045112286_0012 running in uber mode : false
16/12/07 15:04:40 INFO mapreduce.Job:  map 0% reduce 0%
16/12/07 15:04:52 INFO mapreduce.Job:  map 1% reduce 0%
16/12/07 15:04:53 INFO mapreduce.Job:  map 3% reduce 0%
16/12/07 15:04:54 INFO mapreduce.Job:  map 4% reduce 0%
16/12/07 15:05:04 INFO mapreduce.Job:  map 5% reduce 0%
...
16/12/07 15:11:03 INFO mapreduce.Job:  map 100% reduce 90%
16/12/07 15:11:04 INFO mapreduce.Job:  map 100% reduce 91%
16/12/07 15:11:12 INFO mapreduce.Job:  map 100% reduce 92%
16/12/07 15:11:15 INFO mapreduce.Job:  map 100% reduce 93%
16/12/07 15:11:18 INFO mapreduce.Job:  map 100% reduce 94%
16/12/07 15:11:20 INFO mapreduce.Job:  map 100% reduce 95%
16/12/07 15:11:27 INFO mapreduce.Job:  map 100% reduce 96%
16/12/07 15:11:34 INFO mapreduce.Job:  map 100% reduce 97%
16/12/07 15:11:40 INFO mapreduce.Job:  map 100% reduce 98%
16/12/07 15:11:46 INFO mapreduce.Job:  map 100% reduce 99%
16/12/07 15:11:54 INFO mapreduce.Job:  map 100% reduce 100%
16/12/07 15:12:02 INFO mapreduce.Job: Job job_1481045112286_0012 completed successfully
16/12/07 15:12:03 INFO mapreduce.Job: Counters: 50
        File System Counters
                FILE: Number of bytes read=4773953054
                FILE: Number of bytes written=9477367699
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=10737469400
                HDFS: Number of bytes written=10737418200
                HDFS: Number of read operations=984
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=16
        Job Counters
                Launched map tasks=320
                Launched reduce tasks=8
                Data-local map tasks=317
                Rack-local map tasks=3
                Total time spent by all maps in occupied slots (ms)=3346500
                Total time spent by all reduces in occupied slots (ms)=1314703
                Total time spent by all map tasks (ms)=3346500
                Total time spent by all reduce tasks (ms)=1314703
                Total vcore-seconds taken by all map tasks=3346500
                Total vcore-seconds taken by all reduce tasks=1314703
                Total megabyte-seconds taken by all map tasks=3426816000
                Total megabyte-seconds taken by all reduce tasks=1346255872
        Map-Reduce Framework
                Map input records=107374182
                Map output records=107374182
                Map output bytes=10952166564
                Map output materialized bytes=4662833155
                Input split bytes=51200
                Combine input records=0
                Combine output records=0
                Reduce input groups=107374182
                Reduce shuffle bytes=4662833155
                Reduce input records=107374182
                Reduce output records=107374182
                Spilled Records=214748364
                Shuffled Maps =2560
                Failed Shuffles=0
                Merged Map outputs=2560
                GC time elapsed (ms)=49277
                CPU time spent (ms)=1616700
                Physical memory (bytes) snapshot=160636329984
                Virtual memory (bytes) snapshot=517148606464
                Total committed heap usage (bytes)=184515297280
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=10737418200
        File Output Format Counters
                Bytes Written=10737418200
16/12/07 15:12:03 INFO terasort.TeraSort: done

real    7m36.809s
user    0m11.058s
sys     0m0.578s



```


Notes:

To List Jobs
/usr/bin/yarn application -list

To Kill Jobs
yarn application -kill $ApplicationId

To delete folder skipping trash

hadoop fs -rmr -skipTrash URI
Eg. /opt/cloudera/parcels/CDH/bin/hadoop fs -rmr -skipTrash /user/snowbite/teragen 

