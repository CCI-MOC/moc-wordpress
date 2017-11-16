---
ID: 177
post_title: HPC Enablement
author: weinbe2
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/project-elastic-hpc/
published: true
post_date: 2016-01-22 18:09:44
---
&nbsp;

[toc]
<h3><span style="text-decoration: underline;"><strong>Overview</strong></span></h3>
The purpose of the Elastic High Performance Computing (HPC) project is to enable fast, flexible bare metal high performance computing resources on the Mass Open Cloud (MOC). This includes traditional HPC cluster environments, such as SLURM, as well as individual, customized deployments. As we will discuss below, it is ideal to stand up a high performance computing environment on any unutilized resources within the MOC.

<hr />

<h3><span style="text-decoration: underline;"><strong>Motivation</strong></span></h3>
What is High Performance Computing?
<p style="padding-left: 30px;"><em>It turns out that defining “HPC” is kind of like defining the word “car” — you probably know what a car is, but I bet you’d be hard pressed to write a concise, simple definition of one that means anything</em>. — <a href="http://insidehpc.com/hpc-basic-training/what-is-hpc/">http://insidehpc.com/hpc-basic-training/what-is-hpc/ </a></p>
There are multiple use cases for HPC:
<p style="padding-left: 30px;">• Applications needing single, big nodes
• Applications needing a dedicated, strong network to connect nodes
• Applications needing hardware accelerators (eg. GPUs, FPGAs)
• Applications needing specialized deployments (eg. ATLAS)</p>
What unifies these use cases is that they demand optimal performance in their requirements: running bare on the hardware and direct access to the network.

There has been a convergence of late between the requirements of an HPC environment and a cloud computing environment.
<p style="padding-left: 30px;">• HPC environments may have full bi-sectional bandwidth networks and may take advantage of accelerators: two features that Cloud environments are starting to have.
• Cloud environments traditionally have fault tolerance and higher level programming models built-in. While HPC hardware was once built explicitly to avoid faults, this is no longer feasible as we approach the exascale. HPC environments now have to build-in fault tolerance, and, as a consequence, have started to have higher level programming models.</p>
There are still differences between an HPC environment and a Cloud environment. HPC users want every cycle and they fit their problems to available resources. Job schedulers are used to moderate resource usage, which traditionally leads to high utilization. On the other hand, the Cloud never wants to turn away users and always wants to meet peak demand. There should always be enough resources to meet the user’s need. This leads to low utilization of a traditional Cloud environment. When we consider these two use cases, it makes sense to have HPC be a part of the MOC because it utilizes otherwise idle resources.

As a first project, we are deploying a custom, flexible cluster based on OpenMPI, which we can stand up alongside the OpenStack production environment and Hadoop BigData jobs. This is sketched in the below image.

<a href="http://massopen.cloud/wp-content/uploads/2016/01/shared_hpc_sketch.svg" rel="attachment wp-att-589"><img class="alignnone size-medium wp-image-589" src="http://massopen.cloud/wp-content/uploads/2016/01/shared_hpc_sketch.svg" alt="shared_hpc_sketch" width="1" height="1" /></a><img class="alignnone size-medium wp-image-589" src="http://massopen.cloud/wp-content/uploads/2016/01/shared_hpc_sketch.svg" alt="shared_hpc_sketch" width="1" height="1" /><img class="alignnone size-full wp-image-590" src="http://massopen.cloud/wp-content/uploads/2016/01/shared_hpc_sketch.png" alt="shared_hpc_sketch" width="285" height="255" />

This original application has branched into two coupled directions, both integrating traditional queueing mechanisms on HPC systems. These two directions correspond to expanding an HPC system onto both virtualized and physical elastic nodes.

Physical elastic nodes supports the traditional picture of high performance computing, which depends on direct access to the hardware. Given appropriate hardware, the elastic nodes can automatically take advantage of resources which enable and optimize tightly coupled applications. Examples of such applications are MPI jobs or multi-GPU jobs. Virtualized nodes are rapidly catching up to the efficiency of bare metal nodes, and for some applications are essentially equivalent. In this case, virtualization gives us additional benefits: the ability to checkpoint, suspend, migrate, and restore jobs.

To accomodate these two use cases, we have developed a proof-of-concept generic framework to provision elastic HPC nodes which can be integrated with a <a href="http://slurm.schedmd.com/">Slurm</a> HPC cluster. To motivate a generic framework, we must first describe a traditional HPC cluster. These clusters are traditional set ups: in the case of Slurm, there is a single controller node which manages a job queue, and a static set of compute nodes which run executables per the Slurm controller's management. The controller manages the static set of compute nodes with a single hard coded file. A generic framework needs to accomodate this picture of a "static" set of nodes.

The solution we have pursued is pre-loading the Slurm cluster with a set of nodes that may or may not be provisioned at any given time. This pre-loaded set of nodes can be uniquely tied to a machine by a hostname. As we elastically provision and deprovision nodes, we can attach them to a Slurm cluster simply by correctly bringing up a new Slurm compute node, assigning the correct hostname to the resource, and notifying the Slurm controller that the new node is available. This requires additional services for synchronization. All of these resources can live on a single additional VM. This configuration is sketched below:

<img class="alignnone size-medium wp-image-1757" src="http://massopen.cloud/wp-content/uploads/2016/01/cluster-285x300.png" alt="cluster" width="285" height="300" />

The power of this general framework is it is applicable to both elastic physical and virtual nodes.

For static nodes, we can couple the ability to provision and deprovision physical nodes elastically with the partner Bare Metal Imaging project. Taking advantage of BMI, we can set up the environment for a Slurm cluster once. As needed, we can bring up bare metal images on any physical resource. Using our general framework, we can then attach these new nodes to an existing Slurm cluster. A further power of BMI is it takes advantage of the Hardware Isolation Layer, <a href="http://massopen.cloud/blog/2016/01/07/project-haas/">HIL</a>, to avoid the issue of any other MOC project, such as the OpenStack deployment, inadvertently effecting the bare metal performance of other resources.

For virtual nodes, we can take advantage of the fact that virtualization enables checkpointing, suspending, migrating, and restoring nodes. As a first application, we will use our general framework to expand the bare-metal cluster on Engage1 with virtual compute nodes hosted on the Kaizen production environment. This enables the two-fold benefit of increasing utilization for our Kaizen environment, as well as increasing the resources available to HPC users on the Engage1 cluster.

As a first use case, we will support backfill-style HTC jobs from the Open Science Grid running on the Engage1 cluster. These jobs are typically small, single core jobs, which fit into low priority free cycles on Engage1. If higher priority jobs come along, the Open Science Grid jobs are terminated and requeued. With virtualized nodes, we can instead suspend OSG jobs and resume them when new resources become available. This is the first of many benefits of virtualized elastic HPC nodes.

Beyond these projects, we are also applying this expertise to help other groups utilize the MOC. One such group is the ATLAS collaboration. We can stand up their own production environment alongside a traditional HPC cluster, a <a href="http://massopen.cloud/blog/2016/01/08/bigdata-research-at-moc/">Big Data Deployment</a>, or our own <a href="http://massopen.cloud/blog/2016/01/08/project-kaizen-production/">Kaizen production environment</a>.

Last, by expanding HPC resources on the MOC, we support <a href="http://massopen.cloud/blog/monitoring-massachusetts-open-cloud-mocmon/">MOC Monitoring</a>. By expanding monitoring onto real production HPC systems, we provide a broader set of information that is useful both for performance and energy optimization, as well as anomaly detection.

<hr />

<h3><span style="text-decoration: underline;"><strong>Project Team</strong></span></h3>
<strong><span style="text-decoration: underline;">Core Project Team</span></strong>
<ul>
 	<li>Dr. Christopher N. Hill, Massachusetts Institute of Technology</li>
 	<li>Rajul Kumar, Northeastern University <a href="mailto:kumar.raju@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Sirine Benbrahim, Boston University <a href="mailto:kumar.raju@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<strong><span style="text-decoration: underline;">Past Contributors</span></strong>
<ul>
 	<li>Dr. Evan Weinberg, Boston University <a href="mailto:weinbe2@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Prof. Richard Brower, Boston University</li>
 	<li>Dr. Ata Turk, Boston University <a href="mailto:ataturk@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Laura Kamfonik, Boston University <a href="mailto:kamfonik@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Ravisantosh Gudimetla, Northeastern University</li>
 	<li>Apoorve Mohan, Northeastern University</li>
 	<li>Sourabh Bollapragada, Northeastern University</li>
 	<li>Saul Youssef, Boston University/ATLAS</li>
 	<li>Mike Dugan, Boston University</li>
 	<li>Augustine Abaris, Boston University/ATLAS</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Progress</strong></span></h3>
<p style="padding-left: 30px;">• May 2017 : Presented at Boston OpenStack summit <a href="https://www.openstack.org/summit/boston-2017/summit-schedule/events/18991/hpchtc-and-cloud-making-them-work-together-efficiently">HPC/HTC and Cloud: Making Them Work Together Efficiently</a></p>
<p style="padding-left: 30px;">• March 2017: Ran experiments on the staging environment to test the feasibility and outcome</p>
<p style="padding-left: 30px;">• February 2017: Modified the Slurm scheduler to hold the state of the jobs on Slurm when a node/VM is suspended</p>
<p style="padding-left: 30px;">• October 2016: Presented a poster Open Science Grid Slurm work at <a href="http://systems.cs.brown.edu/nens/2016/program/">NENS 2016</a></p>
<p style="padding-left: 30px;">• September 2016: Contributed an elastic Slurm environment to the BMI project</p>
<p style="padding-left: 30px;">• May 2016: Deployed a static <a href="http://www.bu.edu/tech/support/research/whats-happening/highlights/atlas/">ATLAS</a> Tier-2 node on the MOC</p>
<p style="padding-left: 30px;">• March 2016: Demoed a SLURM+OpenMPI environment on Kaizen</p>
&nbsp;

<hr />

<h3><span style="text-decoration: underline;"><strong>Timeline</strong></span></h3>
<p style="padding-left: 30px;">• January through May 2017: Setting up the modified Slurm scheduler on Engage1 to use virtual nodes on Kaizen
• January through May 2017: Publish general framework for Elastic Slurm and the value it brings to the system</p>


<hr />

<h3><span style="text-decoration: underline;"><strong>Planning and Getting Involved</strong></span></h3>
To get involved in this project, please send email to <a href="mailto:team@lists.massopen.cloud">team@lists.massopen.cloud</a> and/or join the #moc irc channel on freenode.