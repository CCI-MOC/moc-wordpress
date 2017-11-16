---
ID: 673
post_title: 'Mix&amp;Match Federation'
author: Lucas
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/mixmatch-federation/
published: true
post_date: 2016-03-18 18:28:24
---
&nbsp;

[toc]
<h3><span style="text-decoration: underline;"><strong>Overview</strong></span></h3>
We are proposing a new set of use cases for OpenStack and a set of changes to enable a multi-landlord cloud model, where multiple service providers can cooperate (and compete) to stand up services in a single cloud, and users can federate resources between various services in various ways---"mix and match." This page describes the model and our plan for the Newton and Ocata cycles.

<a href="https://massopen.cloud/wp-content/uploads/2016/03/MMNENS2016.pdf">MixMatch NENS Poster</a>

<a href="https://massopen.cloud/wp-content/uploads/2016/03/Workshop-Resource-Federation-in-a-Multi-Landlord-Cloud.pdf">MOC Workshop '16 Slides: Mix &amp; Match: Resource Federation</a>

<hr />

<h3><a id="user-content-motivation-and-high-level-use-cases" class="anchor" href="https://github.com/CCI-MOC/moc-public/wiki/Mix-and-Match-Federation#motivation-and-high-level-use-cases"></a><strong><span style="text-decoration: underline;">Motivation</span></strong></h3>
Currently, all clouds are stood up by a single company or organization, creating a vertically integrated monopoly. Any competition is between entire clouds and is limited by the customer's ability to move their data over the connectivity between clouds. We think an alternative model is possible, where different entities can stand up individual services within the same data center and the customer (or intermediaries acting on their behalf) can pick and choose between them. We call this model of having multiple landlords in a cloud an Open Cloud eXchange (<a href="http://www.cs.bu.edu/fac/best/res/papers/ic14.pdf">OCX</a>).

The OCX model would enable more innovation by technology companies, enable cloud research and provide more choice to cloud consumers. We are developing this model in a new public cloud, the Mass Open Cloud (MOC), at the <a href="https://www.mghpcc.org">Massachusetts Green High Performance Computing Center (MGHPCC)</a> data center, which is shared between Boston University, Harvard University, the Massachusetts Institute of Technology, Northeastern University and the University of Massachusetts. Some use cases being explored in the context of the MOC illustrate the potential of this model:
<ul>
 	<li>Harvard and MIT both stand up their own OpenStack cloud for students, but provide resources on-demand to the MOC that can be used by researchers that collaborate between the universities and by external users.</li>
 	<li>A storage company stands up a new innovative block storage service on a few racks in the MGHPCC, operates it themselves and makes it available to users of the MOC and/or the individual university clouds. The storage company is in total control of price, automation and SLA for the service; users can choose if they want to use the service.</li>
 	<li>A company stands up a new compute service with innovative hardware (e.g., FPGAs, crypto accelerator) or pricing model. A customer with a two Petabyte disk volume can switch to using that compute without having to move the data.</li>
 	<li>A research group from Boston University and Northeastern University develops a highly elastic compute service that can checkpoint thousands of servers into SSD  in seconds and broadcast provision a distributed application to allow an interactive medical imaging application that wants thousands of servers for a few seconds.</li>
 	<li>A security sensitive life sciences company exploits software from the <a href="http://www.bu.edu/hic/research/macs/">MACS</a> project to distribute their data across two storage services from non-colluding providers. The data is accessed from a Nova compute service running on a trusted compute platform developed collaboratively with Intel. Since all services are deployed in the same data center, the computation is efficient.</li>
 	<li>Students in a <a href="https://okrieg.github.io/EC500/index.html">cloud computing course</a> offered by Northeastern University and Boston University faculty develop and stand up an experimental proxy service for open stack services that provides users of the MOC a Swift service that combines the inventory of multiple underlying Swift services.</li>
</ul>
We believe solutions to the problems of the OCX model will improve OpenStack generally from a security and reliability perspective. Solving the problems of multiple providers/landlords that don't trust each other also brings defense in depth for a single provider cloud; potentially preventing an exploit of one service from compromising an entire cloud.

<hr />

<h3><span style="text-decoration: underline;"><strong>Architecture</strong></span></h3>
We are building on a feature added in the Kilo release of OpenStack called<strong> Keystone-to-Keystone</strong> federation.  With this feature, <strong>Keystone A</strong> can act as an identity provider for <strong>Keystone B</strong>, providing the user in the former with a <a href="https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language">SAML assertion</a> that describes the user's identity and permissions. Keystone B consumes this assertion and produces a token with which the user can query the services to access the resources.
<h3>Storage</h3>
The use case we are currently focusing for now, is to allow Nova to access volumes, images and snapshots in a service provider with Keystone-to-Keystone federation. For this, we have built a <strong>new OpenStack service that will act as a proxy to Cinder and Glance</strong>. Our new service will transparently forward API calls to the correct service provider after resolving the location of the resources queried, and performing the SAML exchange to get a token for the remote service.

[caption id="attachment_1744" align="aligncenter" width="630"]<img class="wp-image-1744 size-full" src="http://massopen.cloud/wp-content/uploads/2016/03/Proxy_User.png" alt="Proxy_User" width="630" height="540" /> (1) User authenticates to Keystone and receives a token. (2) User asks nova to attach a volume. (3) Nova asks the Cinder endpoint (proxy) for the volume. (4) Proxy asks Keystone for a SAML assertion, which it uses (5) to receive a token for the remote cloud. (6) Finally the proxy forwards the request to the remote service.[/caption]
<h3>Network</h3>
Work is planned towards allowing VMs in different clouds to connect to each other as if they had been launched on the same private network (without the need the need to assign public IP addresses to the instances). This is conducive to the mix and match of compute resources across providers. The solution will make use of VXLAN tunneling and be orchestrated through the mixmatch proxy service. (In the past, we looked at vendor SDN solutions which plug into OpenStack Neutron like Midonet and Contrail.)

<hr />

<h3><strong><span style="text-decoration: underline;">Project Team</span></strong></h3>
<strong><span style="text-decoration: underline;">Core Project Team</span></strong>
<ul>
 	<li>Kristi Nikolla (Boston University)  <a href="mailto:knikolla@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Eric Juma <a href="mailto:ericjuma@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Jeremy Freudberg <a href="mailto:jfreud@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<strong style="line-height: 1.5;"><span style="text-decoration: underline;">Contributors</span></strong>
<ul>
 	<li>Sean Perry (HP)</li>
 	<li><span style="line-height: 1.5;">Jon Bell (Boston University)</span></li>
 	<li><span style="line-height: 1.5;">Davanum Srinivas (Mirantis)</span></li>
 	<li>Adam Young (Red Hat)</li>
 	<li>Minying Lu (Boston University)</li>
 	<li>George Silvis III (Boston University)</li>
 	<li>Wjdan Alharthi (Boston University)</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Timeline</strong></span></h3>
<h4>2017</h4>
<ul>
 	<li>January through June: External testing in production deployments for storage</li>
 	<li>May through August: Work on network federation implemented as a PoC</li>
</ul>
<h4>2016</h4>
<ul>
 	<li>July: Discussion with upstream Nova team about solution architecture</li>
 	<li>July through October 2016: Implementing proxy service</li>
 	<li>October: Presenting our solution in a vBrownBag lightning talk at the Ocata OpenStack Summit in Barcelona</li>
 	<li>December: First release of the proxy as an upstream OpenStack project</li>
</ul>

<hr />

<h3><a id="user-content-planning-and-getting-involved" class="anchor" href="https://github.com/CCI-MOC/moc-public/wiki/Mix-and-Match-Federation#planning-and-getting-involved"></a><strong><span style="text-decoration: underline;">Planning and Getting Involved</span></strong></h3>
To get involved, email (mail:<a href="mailto:team@lists.massopen.cloud">team@lists.massopen.cloud</a>) or join the #moc IRC channel on freenode.  You can also look at our current status on Trello, at <a href="https://xmail.bu.edu/owa/redir.aspx?SURL=Eu1O8kkg-i9bIgQtLZlucoD4Fg7rdMRq012HxEIwCAJ8JIH9l0_TCGgAdAB0AHAAcwA6AC8ALwB0AHIAZQBsAGwAbwAuAGMAbwBtAC8AYgAvAEIAUQBRAEYAZAB5AEwAeAA.&amp;URL=https%3a%2f%2ftrello.com%2fb%2fBQQFdyLx" target="_blank" rel="noopener">https://trello.com/b/BQQFdyLx</a>

Code is available at <a href="https://github.com/openstack/mixmatch">https://github.com/openstack/mixmatch</a>

Documentation is available at <a href="http://mixmatch.readthedocs.io">http://mixmatch.readthedocs.io</a>
<h3></h3>