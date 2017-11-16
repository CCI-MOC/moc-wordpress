---
ID: 80
post_title: Monitoring Mass Open Cloud
author: ataturk
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/monitoring-massachusetts-open-cloud-mocmon/
published: true
post_date: 2016-01-11 19:10:22
---
[toc]
<h3><strong><span style="text-decoration: underline;">Overview</span></strong></h3>
The MOC Monitoring project aims to (i) present a scalable monitoring platform to collect and retain rich information about the Mass Open Cloud (MOC) and (ii) make that information available for analysis. On this page, we present our design for a scalable cloud monitoring system that can collect, mediate, and expose data coming from multiple layers of the cloud, provide our roadmap for the coming months, and share the immediate team of researchers/engineers working on the project.

<hr />

<h3><strong><span style="text-decoration: underline;">Motivation</span></strong></h3>
Cloud users today have little visibility into the performance characteristics, power consumption and utilization of cloud software and hardware components. The cloud has little visibility into user application performance requirements and critical metrics, such as response time and throughput. Through this project, we attempt to reduce this information gap.

Our fundamental goal is to provide a monitoring infrastructure that can expose rich information about all layers of the cloud (facility, network, hardware, OS, middleware, application and user layers) to all the other layers. In this way, we aim to reduce the need for costly reverse engineering or coarse-grained assumptions. We believe that such detailed, multi-layered information is key to developing intelligent, realistic performance and energy optimization techniques. For example, if the goal is to rapidly reduce data center power used while maintaining service level agreements (SLAs), we believe that it will be essential to identify candidate application components and their power/performance characteristics, and then determine the power saving technique to be applied to those components under SLA constraints.

<hr />

<h3><span style="text-decoration: underline;"><strong>Current MOC Monitoring Architecture</strong></span></h3>
The current design of MOC’s monitoring architecture assumes a standard Infrastructure as a Service (IaaS) cloud setup that is composed of switches, storage and servers on the physical layer, and managed by OpenStack on the virtual layer.

<img class="alignnone wp-image-640" src="http://massopen.cloud/wp-content/uploads/2016/01/MOCMON-300x163.png" alt="MOCMON" width="596" height="324" />

The figure above shows the main components of the current MOC monitoring architecture. Our architecture is composed of four layers:

1. Data collection layer: Monitoring information from different layers are collected in a scalable and low-overhead manner
2. Data retention &amp; consolidation layer: Collected monitoring information is persisted and then consolidated in the time-series database InfluxDB
3. Services layer: Houses IaaS services, such as alerting and metering, as well as privacy preserving Application Programming Interface (API) services to expose monitoring data to unprivileged users
4. Advanced monitoring applications layer: Where a wide variety of value-added services that utilize the monitoring data operate

Virtual layer utilization information is collected using Ceilometer; the syslogs of the individual compute servers and the logs of the OpenStack services are collected using LogStash; and the physical server resource utilization (e.g., CPU, memory, disk utilization), power metrics (e.g., consumed power, fan speeds, temperature sensors) and switch network utilization metrics (e.g., incoming/outgoing traffic on switch ports) are collected using Sensu.

Ceilometer and LogStash are coupled with scalable storage systems MongoDB and ElasticSearch, respectively. Since Sensu is an alerting system, normally data collected by Sensu is not stored in any database. We use the time-series database InfluxDB for persisting the information collected by Sensu. We also correlate and consolidate various types of data collected by Ceilometer and LogStash in InfluxDB. This consolidated database enables formulating complex queries, such as queries that expose the cross-layer state of the components of the datacenter they are utilizing to cloud users, or queries that enable cloud administrators to understand the impacts of changes made in the physical layer on virtual and application-layer performance.

Data collected from different layers can be used for performing various services of MOC. A metering service queries the MongoDB database populated by Ceilometer to create user cloud usage reports for given periods of time. The logging data collected by LogStash and indexed/served by ElasticSearch provides a keyword-based search interface with various filters to the MOC admins to be used for debugging. Sensu goes through a set of checks and creates alerts if any anomalies in systems are observed.

In addition to the above mentioned fundamental services, we are planning to have the MOC monitoring system provide two APIs that enable querying of the consolidated data in InfluxDB. The Security API will provide detailed networking information as well as interaction and packet meta-data information of users who opt-in to supply this data. The Monitoring API will simply expose all correlated performance, resource utilization and power data.

A unique feature of our monitoring system design is transparency to cloud users and administrators for a number of applications. For example, we are working on providing end-users with privacy preserving APIs that expose the state of the physical resources over which their VMs are hosted. This will enable users to achieve and automate stable and repeatable performance. We are also working on correlating and exposing virtual layer performance and physical layer power utilization data to cloud administrators. This can enable more intelligent resource scheduling in data centers as well as participation of data centers in energy market demand-response programs.

<hr />

<h3><span style="text-decoration: underline;"><strong>MOC Monitoring Sub Projects</strong></span></h3>
<h4><b> Monitoring for power management in datacenters: </b></h4>
In this project, we try to use MOC monitoring data for: (1) Participation in emerging smart grid demand response programs in order to reduce datacenter energy costs and stabilize power grid demands, (2) budgeting available power to applications via peak shaving.

Today’s datacenters are capable of providing significant flexibility in power consumption owing to the advancements in dynamic power management techniques in servers as well as to the increasing diversity of cloud jobs ranging from latency-critical transactional jobs to delay-tolerant jobs. We believe that if we couple this flexibility with a systematic and accurate cloud power and performance monitoring infrastructure, cloud datacenters would be promising candidates for participating in emerging energy market demand response programs such as regulation service reserves (RSR) and peak power shaving. This participation not only can help stabilize the power grid and enable substantial electricity generation from renewables, but also could decrease rapidly growing energy costs of cloud datacenters due to the incentive credits offered by energy market operators for reserve provisioning.

A rich set of control knobs are available in a datacenter, including job scheduling or power capping of servers via voltage-frequency scaling and via changing resources allocated to each VM. In our initial analysis, we used the number of vCPUs allocated to a VM as an example, and also as a proxy to mimic a combination of various control knobs. Results of RSR provisioning and peak shaving studies we made on MOC using this control knob is presented in:
<ul>
 	<li>A. Turk, H. Chen, O. Tuncer, H. Li, Q. Li, O. Krieger, A. K. Coskun, “Seeing into a Public Cloud: Monitoring the Massachusetts Open Cloud”, in USENIX CoolDC’2016.
https://www.usenix.org/system/files/conference/cooldc16/cooldc16-paper-turk.pdf</li>
</ul>
<h4><strong>Security analytics to detect cloud attacks</strong></h4>
Public IaaS clouds are subject to many security threats including data breaches, account compromises, denial-of-service attacks and abuse of cloud services. Traditional security controls such as encryption, data integrity, replication, and two-factor authentication are necessary to raise the cloud security posture, but not sufficient to mitigate against all these threats.

We propose an analytics-based security service (called MOSAIC) implemented on top of the monitoring platform that will use a variety of machine learning algorithms to profile the legitimate activity of cloud users and applications, and detect anomalous activities related to a wide-range of attacks. Our system aims to provide early warning to both the cloud provider and users of the cloud about possible threats experienced by the cloud infrastructure, and will complement existing security defenses.

We also plan to investigate possible mitigation techniques once suspicious activities are detected, for example migrating suspicious VMs in an isolated part of the cloud and performing more detailed monitoring to determine the root cause of the observed behavior.

<hr />

<h3><span style="text-decoration: underline;"><strong>Project Team</strong></span></h3>
<strong><span style="text-decoration: underline;">Core Project Team</span></strong>

• Prof. Ayse Kivilcim Coskun (Boston University) <a href="mailto:acoskun@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Prof. Alina Oprea (Northeastern University)<a href="mailto:alina.oprea@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Ata Turk, PhD (Boston University) <a href="mailto:ataturk@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Raja Sambasivan (Boston University) [icon name="envelope-o" class="" unprefixed_class=""]<a href="mailto:ataturk@bu.edu">
</a>• Ruoyu Chen<a href="mailto:chen.ruo@husky.neu.edu"> [icon name="envelope-o" class="" unprefixed_class=""]</a>
• Jeremy Freudberg <a href="mailto:jfreud@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Nagasai Vinaykumar Kapalavai <a href="mailto:kapalavai.n@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Rohit Kumar <a href="mailto:kumar.ro@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a><a href="mailto:rrs@bu.edu">
</a>• Lily Sturmann  <a href="mailto:lily.sturmann@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a><a href="mailto:rrs@bu.edu">
</a>• Trishita Tiwari <a href="mailto:trtiwari@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
<strong><span style="text-decoration: underline;">Contributors</span></strong>

• Susanne Balle (Intel)
• Julia A. Santos (Intel)
• Hua Li (Wayfair.com)
• Qingqing Li (Boston University)
• Hao Chen (Boston University)
• Dimitri Makrigiorgos <a href="mailto:dmak1112@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Gen Ohta <a href="mailto:ohta.g@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Ugur Kaynar <a href="mailto:ukaynar@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>
• Ozan Tuncer <a href="mailto:otuncer@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a>

<hr />

<h3><strong><span style="text-decoration: underline;">Progress</span></strong></h3>
<p style="padding-left: 30px;">• <em>March 2016:</em> Test of monitoring stack in Staging, Production deployment of Sensu+InfluxDB+Graphana
• <em>April 2016</em>: Visualization of MOC hardware utilization in Horizon; Evaluation of Monasca as alternative monitoring solution, decision to not proceed with Monasca for now.
• <em>May--August 2016:</em> Incremental backups of InfluxDB, formatted emails, calibrated thresholds for alerting system, complete testing of Ceilometer &amp; ElkStack in staging.
• <em>June--August 2016:</em> Integration of datacenter layer large component (e.g. IRCs, water pumps, chilling towers, ...) power usage into the monitoring system.
• <em>September--November 2016:</em> Exposing monitoring data to trusted researchers.
• <em>September--December 2016:</em> MOC monitoring show back system (virtualized resources per project)
• <em>November--December 2016:</em> MOC monitoring show back system (bare-metal resources)
• <em>December 2016-- March 2017:</em> Collect network flow data from MOC switches to get detailed information on both internal and external network activity of cloud VMs.
• January 2017: Create monitoring working group
• February <em>2017 -- Present: </em>Re-evaluate Monasca due to performance issues w/Ceilometer.
• March<em> 2017-- April 2017: </em>Evaluate different end-to-end tracing systems for integration into the monitoring infrastructure.</p>


<hr />

<h3><span style="text-decoration: underline;"><strong>Planning and Getting Involved</strong></span></h3>
To get involved in this project, please send email to (mail:<a href="mailto:team@lists.massopen.cloud">team@lists.massopen.cloud</a>) and/or join the #moc irc channel on free node.
<h3></h3>
<h3></h3>