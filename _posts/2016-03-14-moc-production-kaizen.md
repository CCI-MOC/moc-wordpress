---
ID: 524
post_title: MOC Research Production (Kaizen)
author: kamfonik
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/moc-production-kaizen/
published: true
post_date: 2016-03-14 14:34:09
---
&nbsp;

[toc]
<h3><span style="text-decoration: underline;"><strong>Overview</strong></span></h3>
Kaizen, MOC's production OpenStack cluster, provides a cloud platform for the research and development projects being worked on by the MOC and its outside partners.

In the spring of 2016, our cloud hosted projects for <span style="color: #000000;">approximately 90 </span>students from our <a href="http://okrieg.github.io/EC500/index-spring-2016.html">Cloud Computing courses</a> at Boston University and Northeastern University.  The class was offered again in Spring 2017 to 40 students at Boston University. It is also home to small number of longer-term projects owned by researchers at our partner institutions.  Fun fact - one of those projects is this very website, which is served by a small OpenStack instance.

Kaizen means 'continuous improvement' in Japanese, and we chose this name to reflect the MOC's emphasis on innovation. Two core MOC projects, <a href="http://massopen.cloud/blog/project-hil/">HIL</a> and <a href="http://massopen.cloud/blog/monitoring-massachusetts-open-cloud-mocmon/">MOCMon</a>, had been integrated into Kaizen. Many other projects currently being developed on top of the existing cloud will be incorporated into the production cloud once they are ready for release.  In this way Kaizen perpetuates a cycle of improvement - each new project that is incorporated adds new features to the cloud, which in turn enables more opportunities for future development and research.

To see Kaizen in action, check out our <a href="https://www.youtube.com/playlist?list=PLt4NWsdhZYQMXKXiyjEDpfxAJfFak7uC7">Video Tutorials</a>. For those who prefer text to video, we also maintain a <a href="https://github.com/CCI-MOC/moc-public/wiki/openstack-tutorial-index">text-based tutorial</a> on our public wiki.

<hr />

<h3><span style="text-decoration: underline;"><strong>Architecture</strong></span></h3>
Kaizen is deployed on 32 compute servers and two redundant controller nodes, with an additional services node to host various tools for monitoring and deployment. It can be expanded to include more nodes from the cluster, which comprises 48 Cisco UCS C220 M3 servers.  All of these servers are located in one of Northeastern University's pods at the <a href="http://www.mghpcc.org/">Massachusetts Green High Performance Computing Center</a> (MGHPCC) in Holyoke, MA.

The cluster is currently running OpenStack Mitaka on Red Hat Enterprise Linux OpenStack Platform.  <span style="line-height: 1.75;">Server configuration and OpenStack deployment on the cluster is heavily automated using Puppet.  Our production puppet scripts are hosted publicly on </span><a style="line-height: 1.75;" href="https://github.com/CCI-MOC/kilo-puppet">GitHub</a><span style="line-height: 1.75;">.</span>

Kaizen OpenStack uses Ceph as a storage backend with a Fujitsu CD10000 appliance providing 136 TB storage. W<span style="line-height: 1.75;">e also expose a Swift endpoint via the RADOS Gateway client.</span>

<hr />

<h3> <span style="text-decoration: underline;"><strong>Diagrams</strong></span></h3>
<img class=" wp-image-736 aligncenter" src="http://massopen.cloud/wp-content/uploads/2016/03/NUTopologyForWebsite-300x225.png" alt="NUTopologyForWebsite" width="873" height="655" />

<img class=" wp-image-743 aligncenter" src="http://massopen.cloud/wp-content/uploads/2016/03/NUCD10kForWebsite-300x225.jpg" alt="NUCD10kForWebsite" width="823" height="617" />

<hr />

<h3 style="color: #000000;"><span style="text-decoration: underline;"><strong><span style="color: #000000; text-decoration: underline;">Project Team</span></strong></span></h3>
<strong><span style="text-decoration: underline;">Core Project Team</span></strong>
<ul>
 	<li>Radoslav Milanov, Senior Infrastructure Engineer (Boston University) <a href="mailto:radonm@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Laura Kamfonik, Junior Infrastructure Engineer (Boston University)  <a href="mailto:kamfonik@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Duaa Tashkandi, Intern (Boston University) <!--a href="mailto:"&gt;[icon name="envelope-o" class="" unprefixed_class=""]&lt;/a--></li>
 	<li>Lily Sturmann, Intern (Boston University) <!--a href="mailto:"&gt;[icon name="envelope-o" class="" unprefixed_class=""]&lt;/a--></li>
 	<li>Piyanai Saowarattitada, MOC Director of Engineering and Infrastructure</li>
</ul>
<strong><span style="text-decoration: underline;">Contributors</span></strong>
<ul>
 	<li>MOC Core Team</li>
 	<li>Rajiv Shridhar, Director - System &amp; Production Services (Northeastern University)  <a href="mailto:r.shridhar@neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Anand Dhingra, System &amp; Production Services (Northeastern University)  <a href="mailto:a.dhingra@neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Nilay Roy, System &amp; Production Services (Northeastern University)</li>
 	<li>Brent Holden, Field Chief Technologist (Red Hat Enterprise Linux)</li>
 	<li>Jonathan Proulx, Senior Technical Architect (MIT CSAIL)</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Timeline</strong></span></h3>
<ul>
 	<li>April 2015 - Infrastructure began with crucial NEU IT networking (Anand Dhinga) and storage (Nilay Roy) in place to collaborate with the MOC deploying on cluster design and implementation. For starter, NEU provided 129.10.3.0/25 and 129.10.3.248/29 IP addresses for the deployment.</li>
 	<li>June 2015 - Work begins on installing Red Hat and OpenStack on the current infrastructure.  MOC welcomes a big crew of summer interns.</li>
 	<li>July through October 2015 - Collaborated with our MIT CSAIL partner to explore the possibility of creating a two-region community cloud on a Galera cluster having Kaizen as one of the two regions. The collaboration was put on hold due to MIT CSAIL resource issue.</li>
 	<li>November 2015 - The Kaizen production cluster is officially deployed, just in time for an MOC workshop on November 19, hosted at BU.</li>
 	<li>December 2015 - Kaizen gets its first real users, a handful of researchers working for our partner universities and MGHPCC.</li>
 	<li>January 2016 - Automating and Integrating Sensu for monitoring. NEU provided additional 129*/26 IP address set to be used for the <a href="https://okrieg.github.io/EC500/index-spring-2016.html">Cloud Computing classes </a>.</li>
 	<li>February 2016 - About 90 student users from BU and NEU join the cluster. Provided how-to training sessions to student users.</li>
 	<li>March through April 2016 - Improving Kaizen
<ul>
 	<li>Sensu/Grafana/Influxdb for monitoring</li>
 	<li>Security including but not limited to regular updates and Suricata to monitor VM traffic.</li>
 	<li>Scheduled incremental backup.</li>
 	<li>Exploring professional security auditing service.</li>
</ul>
</li>
 	<li>Summer/Fall 2016
<ul>
 	<li>Deploy BMI to elastically allocate OpenStack compute nodes at will</li>
 	<li>Deploy additional services: Heat (orchestration) and Sahara (Hadoop cluster provisioning)</li>
 	<li>Deploy second controller node for redundancy</li>
 	<li>Utilize the larger set of /22 subnet made available by the MIT/CSAIL team</li>
 	<li><span style="line-height: 1.5;">OpenStack Liberty upgrade</span></li>
</ul>
</li>
 	<li>Fall/Winter 2016
<ul>
 	<li><span style="line-height: 1.5;">Finish inter-pod communication </span></li>
 	<li>Scoping possible solutions for instance backups</li>
 	<li>Exploring tools to automate operating system level syslog/crontab tools such as Logwatch</li>
 	<li>Scoping a possible need of hardware/firmware vulnerabilities and known solutions e.g. TPM</li>
</ul>
</li>
 	<li>Spring 2017
<ul>
 	<li>OpenStack Mitaka upgrade</li>
 	<li>Development of <a href="https://github.com/CCI-MOC/moc-openstack-tools">automation tools</a> to handle requests for new OpenStack accounts, account changes, etc.</li>
 	<li>Partnership with the University of Massachusetts to provide helpdesk support to Kaizen users</li>
 	<li>OpenStack training for IT Partners at BU</li>
 	<li>Staging tests for upgrades to Newton and Ocata</li>
 	<li>May 23: Equipment Upgrades during MGHPCC Planned Outage</li>
</ul>
</li>
 	<li>Summer 2017 (Planned)
<ul>
 	<li>OpenStack Newton upgrade</li>
 	<li>Improvements to new user/project automation tools</li>
 	<li>Exploring HIPAA in Kaizen</li>
</ul>
</li>
</ul>

<hr />

<h3><strong><span style="text-decoration: underline;">Planning and Getting Involved</span></strong></h3>
To get involved in this project, please send email to team@lists.massopen.cloud and/or join the #moc irc channel on freenode.