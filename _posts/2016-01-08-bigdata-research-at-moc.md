---
ID: 54
post_title: Big Data Enablement @MOC
author: ataturk
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/bigdata-research-at-moc/
published: true
post_date: 2016-01-08 06:21:37
---
&nbsp;

[toc]
<h3><a id="overview"></a><span style="text-decoration: underline"><strong>Overview</strong></span></h3>
<div class="page" title="Page 1">
<div class="layoutArea">
<div class="column">

MOC is part of the <a href="http://massbigdata.org">Mass Big Data Initiative</a> and at its core is a BigData project. Leveling the field for BigData Platform (BDP) innovation by enabling research and innovation in a production environment is among MOC's main goals. More specifically, its aims are (i) to enable BigData Platform (BDP) research in the Commonwealth region (ii) enabling users &amp; government to easily upload and share public datasets (iii) exploiting innovative infrastructures such as SSDs, low-latency networking, RDMA, accelerators etc... in BDP research.

Currently we are focused on multiple studies that will establish the foundation for the above goals, such as building a public dataset repository, building an on-demand bare-metal BigData provisioning system, building an efficient storage and datacenter-scale caching mechanism that utilize SSDs and enable fast processing of public datasets in BigData platforms provisioned over MOC, providing container-based cloud services using OpenShift, and exposing innovative infrastructures such as GPUs and FPGAs to cloud users as a service.

<hr />

<h3><a id="motivation"></a><span style="text-decoration: underline"><strong>Sub-Projects</strong></span></h3>
<div class="page" title="Page 1">
<div class="layoutArea">
<div class="column">

<strong><em>On-Demand Bare-Metal BigData @ MOC:</em></strong>
Increasingly, there is a demand for bare-metal bigdata solutions for applications that cannot tolerate the unpredictability and performance degradation of virtualized systems. Existing bare-metal solutions can introduce delays of 10s of minutes to provision a cluster by installing operating systems and applications on the local disks of servers. This has motivated recent research developing sophisticated mechanisms to optimize this installation. These approaches assume that using network mounted boot disks incur unacceptable run-time overhead. Our analysis suggest that while this assumption is true for application data, it is incorrect for operating systems and applications, and network mounting the boot disk and applications result in negligible run-time impact while leading to faster provisioning time. We developed a network mounted bigdata provisioning system prototype and working on turning our solution to an on-demand bare-metal bigdata provisioning system.

For more details check:

Ata Turk, Ravi S. Gudimetla, Emine Ugur Kaynar, Jason Hennessey, Sahil Tikale, Peter Desnoyers, and Orran Krieger. <a href="https://www.usenix.org/node/196348">An experiment on bare-metal bigdata provisioning.</a> In 8th USENIX Workshop on Hot Topics in Cloud Computing (HotCloud 16), Denver, CO, 2016. USENIX Association.

<strong><em>MOC Dataset Repository (Cloud Dataverse):</em></strong>
In collaboration with Harvard <a href="http://dataverse.org/">Dataverse</a> team, we are building a dataset repository similar to AWS Public datasets, that will initially host public datasets of the Commonwealth of Massachusetts and then will expand to hosting public and community datasets of researchers in the Massachusetts region. The core project that we work on to provide a dataset repository with added computational abilities is called the Cloud Dataverse project. MOC Dataset Repository (MOC-DR) is the first implementation of Cloud Dataverse.

MOC-DR will host a variety of data sets. Large data sets from the Commonwealth of Massachusetts, academy and industry ( e.g. daily Massachusetts traffic congestion and public transportation information, CERN physical experimentation datasets, human genome datasets and stock market exchange transaction archives) that normally require hours or days to locate, download, customize, and analyze will be available for access. Dataset metadata and associated provenance information will be hosted on MOC-DR, a Dataverse portal [1].

Usage scenarios of the dataset repository include:

(1) Users will be able to upload data sets into MOC-DR: (a) A user will login to MOC-DR, (b) will be authenticated against the MOC authentication system (e.g. Keystone), (c) will be presented an interface for uploading datasets and providing the metadata information (current Dataverse data upload interface). (d) Datasets will be stored in MOC Object Storage backend (CEPH), with which Dataverse communicates via Swift calls.

(2) Users will be able to download Datasets from MOC-DR: (a) A user will log into MOC-DR (using Keystone credentials), (b) browse through public datasets, (c) select a dataset to work on, (d) obtain the Swift Object Store endpoints for the selected dataset, (f) from here on out a user can either use the given endpoints to download the datasets or use MOC Big Data Processing System to analyze the data on MOC without needing to worry about the cost of storing the data or the time required to download it.

<b><i>MOC Big Data as a Service (BDaaS) on Engage1:</i></b>

<span style="font-weight: 400">In collaboration with MIT, Northeastern University, Intel, TwoSigma  and RedHat CEPH team), we are building an on demand Big Data processing system that is closely tied with the MOC public dataset repository. The service will run on the servers of the Engage1 cluster.</span>

<span style="font-weight: 400">One envisioned standard use case for this system is as follows:</span>

<span style="font-weight: 400">(1) User selects a dataset to work on the MOC Big Data Processing environment: After selecting a dataset to work on, (a) users will be directed to MOC On-Demand BigData processing UI, (b) will be able to provision a BigData environment to work on the selected dataset (by setting environment configuration parameters, e,g, number of nodes, storage, processing, memory capacity, the set of Big Data processing applications (e.g. Hadoop, Pig, Spark, …) to have, etc...), (c) using the users credentials and her desired configurations, "MOC Imaging System” will create a Big Data processing cluster for the user, and (d) user will be able to use Hadoop-ecosystem applications to process and analyze the dataset she selected using provided resources.</span>

<span style="font-weight: 400">We envision to provide a number of options for the Big Data processing environment: (A) The Big Data processing system can be:</span>

<span style="font-weight: 400">(I) a bare metal environment provisioned via MOC Bare Metal Imaging (BMI) system: (II) a virtualized environment provisioned via OpenStack Sahara, or (III) a containerized environment (possibly provisioned via Bluedata’s container based solutions).</span>

<span style="font-weight: 400">(B) The dataset (or the subset of the dataset, e.g. imagery data from the last week) selected by the user can be prefetched into the caching servers in the processing cluster. This prefetching will prevent multiple small object requests to the storage backend and will guard against variance that may be caused by input fetching during computation.</span>

<span style="font-weight: 400">Example dataset types: We plan to host a number of datasets. A representative dataset type we plan to host is large read only datasets that require an extraction and transformation phase to enable further analysis such as the CMS Primary Datasets [2], MBTA Ridership, Alerts &amp; Service Quality Datasets [3], or The 3,000 Rice Genomes Project [4]. These types of datasets are by nature well divided into separately operable chunks, they get incremental updates (e.g. daily, weekly, monthly,…), the latest datasets are considered to be more ‘hot’, and most processing tasks can be done in a single pass over a number of incremental units of data (e.g. compute/compare daily aggregates for the last 10 days).</span>

<span style="font-weight: 400">[1] <a href="http://dataverse.org">http://dataverse.org </a></span>

<span style="font-weight: 400">[2] <a href="http://opendata.cern.ch/collection/CMS-Primary-Datasets">http://opendata.cern.ch/collection/CMS-Primary-Datasets</a></span>

<span style="font-weight: 400">[3] <a href="http://www.mbta.com/about_the_mbta/document_library/?search=blue+book&amp;submit_document_search=Search+Library">http://www.mbta.com/about_the_mbta/document_library/?search=blue+book&amp;submit_document_search=Search+Library</a></span>

<span style="font-weight: 400">[4] </span><a href="http://gigascience.biomedcentral.com/articles/10.1186/2047-217X-3-7"><span style="font-weight: 400">http://gigascience.biomedcentral.com/articles/10.1186/2047-217X-3-7</span></a>

<span style="font-weight: 400">Existing studies indicate that in the HDFS, a few files account for a very high number of accesses and there is a skew in access frequency across HDFS data. Thus, we believe that more sophisticated caching architecture and policies that includes the frequently accessed files will bring considerable benefit to big data analytics.</span>

<span style="font-weight: 400">We are working on improving the performance of Big Data processing Frameworks (e.g. Hadoop, Spark...etc) in MOC BDaaS by implementing a multi-level cache architecture called “D3N”. More details can be found <span style="color: #0000ff"><a href="https://massopen.cloud/d3n/">here</a>.</span></span>

&nbsp;

</div>
</div>
</div>
</div>
</div>
</div>

<hr />

<h3><a id="core-team"></a><span style="text-decoration: underline"><strong>Project Team</strong></span></h3>
<strong><span style="text-decoration: underline">Core Project Team</span></strong>
<ul>
 	<li style="text-align: left">Prof. Orran Krieger (Boston University) <a href="mailto:okrieg@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">Prof. Peter Desnoyers (Northeastern University) <a href="mailto:pjd@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">Ata Turk, PhD (Boston University) <a href="mailto:ataturk@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">E. Ugur Kaynar (Boston University) <a href="mailto:ukaynar@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">Ravisantosh Gudimetla (Red Hat) <a href="mailto:ravig@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">Sahil Tikale Nikhil (Boston University) <a href="mailto:tikale@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">Mania Abdi (Northeastern University) <a href="mailto:mania.abdi287@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li style="text-align: left">Mohammad Hossein Hajkazemi (Northeastern University) <a href="mailto:hajkazemi@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<span style="text-decoration: underline"><strong>Collaborators</strong>
</span>
<ul>
 	<li style="text-align: left">David Cohen (Intel)</li>
 	<li>Gary Berger (Brocade)</li>
 	<li>William Nelson (Lenovo)</li>
 	<li><a href="http://dataverse.org/">Dataverse</a> Team (Harvard)</li>
 	<li>Mercè Crosas (Harvard)</li>
 	<li>Leonid Andreev (Harvard)</li>
 	<li>Zhidong Yu (Intel)</li>
 	<li>Jingyi Zhang (Intel)</li>
 	<li>Piyanai Saowarattitada (MOC)</li>
 	<li>Tom Nadeau (Brocade)</li>
 	<li>Mark Presti (Brocade)</li>
 	<li>Robert Montgomery (Brocade)</li>
 	<li>Pete Moyer (Brocade)</li>
</ul>

<hr />

<h3><a id="timetable"></a><strong><span style="text-decoration: underline">Timeline</span></strong></h3>
<ul>
 	<li>April--August 2017: 2-tiered caching: upstreaming developed code (CEPH).</li>
 	<li>August--November 2017: Caching write-back support.</li>
 	<li>August--November 2017: Containers as an MOC service: production OpenShift setup.</li>
 	<li>November-- 2018: Production MOC BigData as a Service solution.</li>
</ul>

<hr />

<h3><strong><span style="text-decoration: underline"><a id="getting-involved"></a>Planning and Getting Involved</span></strong></h3>
<div class="page" title="Page 2"> To get involved in this project, please send email to (mail:<a href="mailto:team@lists.massopen.cloud">team@lists.massopen.cloud</a>) and/or join the #moc irc channel on free node.</div>