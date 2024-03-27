Apache Hadoop
The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using simple programming models. It is designed to scale up from single servers to thousands of machines, each offering local computation and storage. Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer, so delivering a highly-available service on top of a cluster of computers, each of which may be prone to failures.


- ./config

Change the image: apache/hadoop:3 incase you want to build any other image like image: apache/hadoop:3.3.5 for building Apache Hadoop 3.3.5 image

Create a config file like:

    CORE-SITE.XML_fs.default.name=hdfs://namenode
    CORE-SITE.XML_fs.defaultFS=hdfs://namenode
    HDFS-SITE.XML_dfs.namenode.rpc-address=namenode:8020
    HDFS-SITE.XML_dfs.replication=1
    MAPRED-SITE.XML_mapreduce.framework.name=yarn
    MAPRED-SITE.XML_yarn.app.mapreduce.am.env=HADOOP_MAPRED_HOME=$HADOOP_HOME
    MAPRED-SITE.XML_mapreduce.map.env=HADOOP_MAPRED_HOME=$HADOOP_HOME
    MAPRED-SITE.XML_mapreduce.reduce.env=HADOOP_MAPRED_HOME=$HADOOP_HOME
    YARN-SITE.XML_yarn.resourcemanager.hostname=resourcemanager
    YARN-SITE.XML_yarn.nodemanager.pmem-check-enabled=false
    YARN-SITE.XML_yarn.nodemanager.delete.debug-delay-sec=600
    YARN-SITE.XML_yarn.nodemanager.vmem-check-enabled=false
    YARN-SITE.XML_yarn.nodemanager.aux-services=mapreduce_shuffle
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.maximum-applications=10000
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.maximum-am-resource-percent=0.1
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.resource-calculator=org.apache.hadoop.yarn.util.resource.DefaultResourceCalculator
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.queues=default
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.default.capacity=100
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.default.user-limit-factor=1
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.default.maximum-capacity=100
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.default.state=RUNNING
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.default.acl_submit_applications=*
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.root.default.acl_administer_queue=*
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.node-locality-delay=40
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.queue-mappings=
    CAPACITY-SCHEDULER.XML_yarn.scheduler.capacity.queue-mappings-override.enable=false
** You can add/replace any new config in the similar format in this file.

Check the current directory (optional)
Do a ls -l on the current directory it should have the two files we created above

    docker-3 % ls -l
    -rw-r--r--  1 hadoop  apache  2547 Jun 23 15:53 config
    -rw-r--r--  1 hadoop  apache  1533 Jun 23 16:07 docker-compose.yaml

###Run the docker containers
Run the docker containers using docker-compose

    docker-compose up -d

    docker exec -it docker-3_namenode_1 /bin/bash

###Running an example Job (Pi Job)
    yarn jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.5.jar pi 10 15

The above will run a Pi Job and similarly any hadoop related command can be run.

###Accessing the UI
The Namenode UI can be accessed at http://localhost:9870/ and the ResourceManager UI can be accessed at http://localhost:8088/

Shutdown Cluster
The cluster can be shut down via:

    docker-compose down

Note:
The above example is for Hadoop-3.x line, In case you want to build the Hadoop-2.x, Similar steps but different config & docker-compose.yaml file. Logic can be extracted from: https://github.com/apache/hadoop/tree/docker-hadoop-2

Docker Source Code:
The docker images are built via special branches & the source code for branch 3 lies at https://github.com/apache/hadoop/tree/docker-hadoop-3 and for branch 2 at https://github.com/apache/hadoop/tree/docker-hadoop-2

Reaching out us:
Hadoop Developers can be reached via the hadoop mailing lists: https://hadoop.apache.org/mailing_lists.html

Further Reading
https://hadoop.apache.org/