---
ID: 1588
post_title: ""
author: piyanai
post_excerpt: ""
layout: page
permalink: >
  https://massopen.cloud/sahara-user-guide/
published: true
post_date: 2016-08-16 14:42:53
---
This guide explains the use of OpenStack Sahara as configured in the MOC. Sahara provides virtualized Big Data as a Service. The following will be a walkthrough of both the cluster and job operations that Sahara provides, with plenty of helpful tips (and warnings) sprinkled throughout. If you have questions or discover bugs, email MOC at moc-team-list at bu.edu.

*Accessing the Sahara UI:* Sahara actions are contained within the "Data Processing" section in the "Project" section of Horizon.
<h1>Sahara End-User Guide</h1>
<h2><strong>Cluster creation</strong></h2>
You may also use the following wizard:

<a href="https://controller-0.kaizen.massopen.cloud/dashboard/project/data_processing/clusters/cluster_guide">https://controller-0.kaizen.massopen.cloud/dashboard/project/data_processing/clusters/cluster_guide</a>
<h3>Plugin selection</h3>
Initially, three plugins are offered. Each is considered "simple". Eventually vendor plugins from Cloudera and Hortonworks will be offered.
<ul>
 	<li>Vanilla: Offers upstream Hadoop 2.7.1, Pig 0.16.0, and Hive 0.11.0, plus Oozie and YARN for job management</li>
 	<li>Spark: Offers Spark 1.6.0 on a Cloudera fork of HDFS</li>
 	<li>Storm: Offers Storm 0.9.2</li>
</ul>
<h3>Node group template design</h3>
Navigate to Data Processing → Clusters → Node Group Templates → Create Template
<ul>
 	<li>Name: Must follow rules for valid hostname</li>
 	<li>Flavor: Make sure to move away from default m1.nano, since this will probably cause kernel panic and crash Sahara engine</li>
 	<li>Availability Zone: Just set to "nova"</li>
 	<li>Storage location: choice between Ephemeral and Cinder
<ul style="list-style-type: circle">
 	<li>Ephemeral for speed</li>
 	<li>Cinder more reliable for HDFS replicas (make sure to set Availabilty Zone)</li>
</ul>
</li>
 	<li>Node processes: Keep in mind validation rules
<ul style="list-style-type: circle">
 	<li>Vanilla: namenode == 1, (resourcemanager, secondary namenode, historyserver, oozie, hiveserver) &lt;= 1; Oozie depends on resourcemanger and historyserver, Nodemanager depends on resourcemanager</li>
 	<li>Spark: namenode == 1, master == 1, datanode &gt;= 1, worker &gt;= 1</li>
 	<li>Storm: nimbus == 1, zookeeper == 1</li>
</ul>
</li>
 	<li>Parameters: Available settings depend on which processes have been selected; you may wish to set dfs.datanode.du.reserved, Heap Size for various processes</li>
 	<li>Security: Auto Security Group feature should handle opening the proper ports</li>
</ul>
<h3>Cluster template design</h3>
Navigate to Data Processing → Clusters → Cluster Templates → Create Template
<ul>
 	<li>Name: Must follow rules for valid hostname</li>
 	<li>Anti-Affinity: Makes sure that instances start on different compute hosts. This is good for reliable HDFS replication. You are bound by number of compute hosts (32 in Kaizen!)</li>
 	<li>Node Groups:
<ul style="list-style-type: circle">
 	<li>Plugin must match</li>
 	<li>Note that if you edit a Node Group Template, you must delete and readd from Cluster Template</li>
</ul>
</li>
 	<li>Parameters: Available settings depend on which processes have been selected; you may wish to set dfs.replication (defaults to number of data/worker nodes), dfs.blocksize</li>
</ul>
<h3>Cluster launching and scaling</h3>
Navigate to Data Processing → Clusters → Cluster Templates → Launch Cluster
<ul>
 	<li>Name: Must follow rules for valid hostname</li>
 	<li>Cluster Template: Plugin must match</li>
 	<li>Cluster Count: Create "separate" clusters - if you want more nodes, then you can scale cluster later, or edit cluster template</li>
 	<li>Base Image: There is a choice of Ubuntu 14.04 or CentOS 6.6/7 for Vanilla plugin; Spark and Storm offer Ubuntu 14.04 only</li>
 	<li>Keypair: Highly recommended, since images do not have default passwords</li>
 	<li>Neutron Management Network: DNS highly recommended; network must have been created redundantly across both OpenStack controllers</li>
 	<li>Scale Cluster: Validation rules still apply</li>
</ul>
<h3>Cluster access</h3>
<ul>
 	<li>Floating IP: Not assigned automatically; recommended that you assign to master node</li>
 	<li>Web UIs: SOCKS proxy may come in handy; Ubuntu images support X forwarding so it may be useful to install a lightweight browser like Midori on one of your instances</li>
 	<li>Usernames: "ubuntu" for Ubuntu images, "cloud-user" for CentOS 6, "centos" for CentOS 7</li>
</ul>
<h2><strong>Job execution</strong></h2>
<h3>Swift integration</h3>
Jobs on Sahara clusters support Swift I/O. To be more specific, this means you can write a Swift path instead of an HDFS path in any MapReduce or Spark job. (Hive support coming soon, hopefully.) Here are some helpful hints regarding the use of Swift:
<ul>
 	<li>Paths: Format is swift://&lt;container&gt;.sahara/path/to/file</li>
 	<li>Authentication: Username and password must be passed at runtime. If you are using the Sahara API/UI to submit jobs, then this may be able to be abstracted away by creating a "Data Source" (More about this in the next section). Username and password are set by fs.swift.service.sahara.username and fs.swift.service.sahara.password. The value fs.swift.service.sahara.tenant is automatically set when the cluster is first created, but you can override it if needed.</li>
 	<li>Easy transfer: It may be easier to move data from Swift into HDFS first, before running a job. You can use the "distributed copy" command, e.g.
<ul style="list-style-type: circle">
 	<li>[wc_code]hadoop distcp -D fs.swift.service.sahara.username=USERNAME -D fs.swift.service.sahara.password=PASSWORD INPUT_PATH OUTPUT_PATH[/wc_code]</li>
</ul>
</li>
</ul>
<h3>Job submission through UI</h3>
<ul>
 	<li>Data Sources: A useful abstraction of Swift paths (credentials applied automatically, too!) or HDFS paths
<ul style="list-style-type: circle">
 	<li>Name: no spaces</li>
 	<li>Data Source Type: Swift or HDFS, but Sahara doesn't validate that your path or credentials are valid, so be careful</li>
 	<li>URL: Just regular HDFS path, or swift://&lt;container&gt;.sahara/path</li>
 	<li>Until approx. May 2017, Username and password will be required for Swift! (not automatically populated)</li>
</ul>
</li>
 	<li>Job Binaries: These are any jar file, script, etc. that you submit with your job.
<ul style="list-style-type: circle">
 	<li>Advised that you store in Swift, since otherwise it is going in MySQL...</li>
</ul>
</li>
 	<li>Job Templates: This is where the actual job is designed
<ul style="list-style-type: circle">
 	<li>Name: no spaces</li>
 	<li>Main Binary: for Hive, Shell, Spark, and Storm</li>
 	<li>Libs: any additional files, or more commonly the main binary for Java/MapReduce</li>
 	<li>Interface Arguments: Allows you to add custom fields to the job launching form, for args and config parameters (will come in handy in next section)</li>
</ul>
</li>
 	<li>Jobs: (launch from Data Processing → Job Templates → Launch on Existing Cluster)
<ul style="list-style-type: circle">
 	<li>Cluster: Make sure to choose the right one!</li>
 	<li>Input/Output: These only show up for MapReduce and Hive jobs. Otherwise you will set your input and output paths as args (or they could be hardcoded in your job)</li>
 	<li>Main Class: Required for Java and Spark jobs, equivalent to setting edp.java.main_class in Configuration section (or by interface argument)</li>
 	<li>Configuration - Swift: "Use Data Source Substitution for Names and UUID" allows you to use datasource://&lt;source_name&gt; in args -- otherwise you can pass fs.swift.service.sahara.username or fs.swift.service.sahara.password and use regular swift:// style paths</li>
 	<li>Arguments: These are command line arguments, often used for input and output args
Interface Arguments: If you designed these in Job Template step, you will see them here. If they are set as required, you will get reminded to set them if you forget, which can be very helpful since there can be many parameters you have to remember to pass for jobs with Swift I/O, etc.</li>
 	<li>Status (after launching): PENDING, RUNNING, etc. are not very descriptive. Recommended to use web console for your cluster. Oozie is good for this on Vanilla cluster. For Spark and Storm, you can also look in /tmp/&lt;PLUGIN&gt;-edp/&lt;JOB_TEMPLATE&gt;/&lt;JOB_ID&gt;/ , where files like launch_command.log and stderr in that folder are very useful.</li>
</ul>
</li>
</ul>
<h3>Manual interaction</h3>
For users who already know a lot about Hadoop etc., they may prefer to use their cluster "manually".
<ul>
 	<li>Where programs live
<ul style="list-style-type: circle">
 	<li>Vanilla: /opt/hadoop, /opt/hive</li>
 	<li>Spark: /opt/spark</li>
 	<li>Storm: /usr/local/</li>
</ul>
</li>
 	<li>Permissions
<ul style="list-style-type: circle">
 	<li>Spark and Storm just have "ubuntu" user for everything</li>
 	<li>Vanilla: Many Hadoop files and programs belong to "hadoop" user</li>
</ul>
</li>
</ul>