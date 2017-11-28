---
ID: 2307
post_title: 'MOC Marketplace Concept at the MOC &#8211; GIJI'
author: Lucas
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/moc-marketplace/
published: true
post_date: 2017-01-12 20:59:41
---
[toc]
<h3><span style="text-decoration: underline;"><strong>Overview</strong></span></h3>
<span class="author-a-z69ziz76zz85zz71z21qmpfoz85zz78zao">The Mass Open Cloud (MOC) needs a simple intuitive GUI for its public cloud marketplace</span><span class="author-a-c7d1gz122zvz79zez80zz72zjeo6z80z">. Our goal is to provide a tool to enh</span><span class="author-a-iz69zbnp6vz89zmz87zz75zz77zr8z85zz85z">a</span><span class="author-a-c7d1gz122zvz79zez80zz72zjeo6z80z">nce the user experience by automating mundane but complicated steps to use cloud resources as well as presenting to our users all available cloud services including but not limited to network routing and ability to choose between a virtual machine (OpenStack instances) and container technologies. </span>
<h3><span style="text-decoration: underline;"><strong>Motivation</strong></span></h3>
<span class="author-a-z69ziz76zz85zz71z21qmpfoz85zz78zao">OpenStack Horizon is inadequate to serve naive users who lack basic cloud knowledge and to whom the cloud is simply a tool to get computing results for their scientific research for example. </span><span class="author-a-c7d1gz122zvz79zez80zz72zjeo6z80z">This MOC marketplace GUI serves to be a simpler tool streamlining the cloud experience. It can be used by itself or alongside Horizon. </span><span class="author-a-z69ziz76zz85zz71z21qmpfoz85zz78zao">Developing a new GUI from scratch can be both time-consuming and challenging. A collaboration with the Cyverse’s Atmosphere/Troposphere project </span><span class="author-a-c7d1gz122zvz79zez80zz72zjeo6z80z">has helped</span><span class="author-a-z69ziz76zz85zz71z21qmpfoz85zz78zao"> accelerat</span><span class="author-a-c7d1gz122zvz79zez80zz72zjeo6z80z">e</span><span class="author-a-z69ziz76zz85zz71z21qmpfoz85zz78zao"> the development of the MOC GUI. </span>

As part of the Open Cloud Exchange, Cloud Dataverse, OpenShift and NetEx are being integrated as services.   From this effort, a standard Rest API will be developed which will be used by 3rd party developers to develop future services.

<hr />

<h3><span style="text-decoration: underline;"><strong>Architecture</strong></span></h3>
Marketplace Architecture
<table style="width: 917px;">
<tbody>
<tr>
<td style="width: 342.9375px;"><img class="alignnone size-medium wp-image-2386" src="https://massopen.cloud/wp-content/uploads/2017/01/flowchart-212x300.png" alt="flowchart" width="212" height="300" /></td>
<td style="width: 559.0625px;"><img class="alignnone wp-image-2512" src="https://massopen.cloud/wp-content/uploads/2017/01/giji.001-300x225.png" alt="giji-001" width="488" height="366" /></td>
</tr>
</tbody>
</table>

<hr />

<h3><span style="text-decoration: underline;"><strong>Current GUI Concept</strong></span></h3>
In the marketplace, we will show content that is from the service developer to indicate its general classification.

&nbsp;

<img class="alignnone wp-image-2783" src="https://massopen.cloud/wp-content/uploads/2017/01/Screen-Shot-2017-07-21-at-09.16.25.png" alt="screen-shot-2017-07-21-at-09-16-25" width="686" height="419" />

Upon selection of a particular service, the specific information needed to use that service will be displayed in a modal.

<img class="alignnone wp-image-2515" src="https://massopen.cloud/wp-content/uploads/2017/01/giji.005-300x225.png" alt="giji-005" width="404" height="303" />

<hr />

<h3 style="color: #000000;"><span style="text-decoration: underline;"><strong><span style="color: #000000; text-decoration: underline;">Project Team</span></strong></span></h3>
<strong><span style="text-decoration: underline;">Core Project Team</span></strong>
<ul>
 	<li>Robert B. Baron   [icon name="envelope-o" class="" unprefixed_class=""]</li>
 	<li>Lucas H. Xu (Boston University) <a href="mailto:xuh@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Timeline</strong></span></h3>
<ul>
 	<li>Fall 2017: Support of NetEx</li>
 	<li>July 2017: Proof of Concept of using Slurm workload manager on GIJI</li>
 	<li>June 2017: Proof of Concept of HIL (Hardware Isolation Layer)</li>
 	<li>May 2017: Proof of Concept of Cloud Dataverse in MOC Simple GUI(GIJI).</li>
 	<li>March 2017: Presentation of MOC Simple GUI (GIJI) for OpenStack summit got accepted.</li>
 	<li>January 2017: Create MOC Simple GUI code base based on atmosphere on Github</li>
 	<li>July 2016: Contact Cyverse atmosphere team with Steve Gregory</li>
</ul>

<hr />

<h3><strong><span style="text-decoration: underline;">Planning and Getting Involved</span></strong></h3>
To get involved in this project, please send an email to (<a href="mailto:team@lists.massopen.cloud">MOC team-list</a>) and/or join the #moc IRC channel on Freenode or join our team on Slack.

Code is available at:
<ul>
 	<li><a href="https://github.com/CCI-MOC/giji-frontend">https://github.com/CCI-MOC/giji-frontend</a></li>
 	<li><a href="https://github.com/CCI-MOC/giji-backend">https://github.com/CCI-MOC/GUI-Backend</a></li>
</ul>
Demo site at:
<ul>
 	<li><a href="https://giji.massopen.cloud/">https://giji.massopen.cloud/</a></li>
</ul>