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
This guide explains the use of OpenStack Sahara as configured in the MOC. Sahara provides virtualized Big Data as a Service. The following will be a walkthrough of both the cluster and job operations that Sahara provides, with plenty of helpful tips (and warnings) sprinkled throughout. If you have questions or discover bugs, please OPEN A TICKET: <a href="https://osticket.massopen.cloud/">https://osticket.massopen.cloud/</a>

*Accessing the Sahara UI:* Sahara actions are contained within the "Data Processing" section in the "Project" section of Horizon.
<h1>Sahara End-User Guide</h1>
<h2><strong>Cluster creation</strong></h2>
You may also use the following wizard:

<a href="https://kaizen.massopen.cloud/dashboard/project/data_processing/clusters/cluster_guide">https://kaizen.massopen.cloud/dashboard/project/data_processing/clusters/cluster_guide</a><br />
<a href="https://engage1.massopen.cloud/dashboard/project/data_processing/clusters/cluster_guide">https://engage1.massopen.cloud/dashboard/project/data_processing/clusters/cluster_guide</a>
<h3>Plugin selection</h3>
Three plugins are offered. Each is considered "simple".
<ul>
 	<li>Vanilla: Offers Hadoop 2.7.1 plus Pig, Hive, Oozie, and YARN</li>
 	<li>Spark: Offers Spark 2.1.0 (<strong>without</strong> YARN)</li>
 	<li>Storm: Offers Storm 1.0.1</li>
</ul>
New plugin versions are offered each time MOC's OpenStack version is upgraded. (Last upgrade: January 2018.)
<h3>Node group template design</h3>
Navigate to Data Processing → Clusters → Node Group Templates → Create Template
<ul>
 	<li>Name: Must follow rules for valid hostname</li>
 	<li>Flavor: Make sure to move away from default m1.nano, since this will probably cause kernel panic and crash Sahara engine</li>
 	<li>Availability Zone: Just set to "nova"</li>
        <li>Floating IP pool: optional (you can assign it later through "Instances" area); probably only need it for master node group</li>
 	<li>Storage location: choice between Ephemeral and Cinder
<ul style="list-style-type: circle">
 	<li>Cinder can be much bigger than ephemeral disk</li>
 	<li>Make sure to set Cinder Availabilty Zone and Volume Type</li>
</ul>
        <li>Make sure to select base image</li>
</li>
 	<li>Node processes: typical topology...
<ul style="list-style-type: circle">
 	<li>Vanilla: master=[namenode, resourcemanager,historyserver, oozie], worker=[datanode, nodemanager]
 	<li>Spark: master=[namenode, master], worker=[datanode,slave]</li>
 	<li>Storm: master=[nimbus], worker=[supervisor, zookeeper]</li>
</ul>
</li>
 	<li>Parameters: Available settings depend on which processes have been selected; you may wish to set dfs.datanode.du.reserved, Heap Size for various processes</li>
 	<li>Security: Auto Security Group feature should handle opening the proper ports</li>
</ul>
<h3>Cluster template design</h3>
Navigate to Data Processing → Clusters → Cluster Templates → Create Template
<ul>
 	<li>Name: Must follow rules for valid hostname</li>
 	<li>Anti-Affinity (optional): Makes sure that instances start on different compute hosts. This is good for reliable HDFS replication. You are bound by number of compute hosts</li>
 	<li>Node Groups:
<ul style="list-style-type: circle">
 	<li>Plugin must match</li>
</ul>
</li>
 	<li>Parameters: Available settings depend on which processes have been selected; you may wish to set dfs.replication (defaults to number of data/worker nodes), dfs.blocksize</li>
</ul>
<h3>Cluster launching and scaling</h3>
Navigate to Data Processing → Clusters → Cluster Templates → Launch Cluster
<ul>
 	<li>Name: Must follow rules for valid hostname</li>
 	<li>Cluster Template: Plugin must match</li>
 	<li>Cluster Count: Create "separate" clusters - (alternatively, keep in mind if you want more nodes, then you can scale cluster later, or edit cluster template)</li>
 	<li>Base Image: Usually there is just one choice of OS/version per plugin, but sometimes more</li>
 	<li>Keypair: Highly recommended, since images do not have default passwords</li>
 	<li>Neutron Management Network: make sure to choose a private (tenant) network, not the floating network</li>
</ul>
<h3>Cluster access</h3>
<ul>
 	<li>Usernames: "ubuntu" for Ubuntu images, "centos" for CentOS 7</li>
        <li>Web UIs: Data Processing -&gt; Clusters -&gt; Clusters -&gt; [cluster name] -&gt; General Info -&gt; (scroll down) shows what ports various services expose Web UIs on
        <li><a href="#manual">Skip ahead to "Manual Interaction" section if you don't plan to use Sahara Job API</a><li>

</ul>
<h2><strong>Job execution</strong></h2>
<h3>Swift integration</h3>
Jobs on Sahara clusters support Swift I/O. To be more specific, this means you can write a Swift path instead of an HDFS path in any MapReduce or Spark job. Here are some helpful hints regarding the use of Swift:
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
</ul>
</li>
 	<li>Job Binaries: These are any jar file, script, etc. that you submit with your job.
<ul style="list-style-type: circle">
 	<li>Advised that you store in Swift</li>
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
<a><h3>Manual interaction</h3></a>
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