---
ID: 15
post_title: >
  Hardware Isolation Layer (HIL, formerly
  HaaS)
author: henn
post_excerpt: ""
layout: post
permalink: https://massopen.cloud/blog/project-hil/
published: true
post_date: 2016-01-07 13:57:45
---
&nbsp;

[toc]
<h3><strong><span style="text-decoration: underline;">Motivation</span></strong></h3>
Hardware Isolation Layer (HIL, formerly known as HaaS)) is an open-source bare metal isolation service that automates allocation and management of isolated pools of non-virtualized compute resources to users. By following a minimal approach, only performing network isolation, HIL allows mutually untrusting and incompatible bare metal services to be deployed in a data center. Resources can then be shifted between pools as supply and demand dictate, increasing utilization and efficiency.  For example, separate pools, allocated out of HIL, are used for the production MOC OpenStack cloud, for staging areas, for <a href="http://massopen.cloud/option-5/">various HPC clusters</a> and for research experiments such as <a href="http://sesa.github.io/">EbbRT</a>.

HIL is being developed as part of the MOC. HIL allows us to securely develop the MOC in the MGHPCC alongside the many production services deployed in this data center, and incrementally move resources into the MOC as demand warrants. It is also critical to the marketplace model that is central to the MOC; by decoupling the operators of hardware and services, HIL will enable a consumption-based marketplace for hardware.

<hr />

<h3><span style="text-decoration: underline;"><strong>Architecture/Development</strong></span></h3>
The Architecture of HIL, while still being realized, is shown below.

<a href="http://massopen.cloud/wp-content/uploads/2016/01/hil_arch.svg" rel="attachment wp-att-1410"><img class="alignnone size-full wp-image-1410" src="http://massopen.cloud/wp-content/uploads/2016/01/hil_arch.svg" alt="hil_arch" /></a>The HIL Architecture is implemented by components linked via REST APIs to form a micro-kernel for the data-center. These components (some of which are still under development) can be categorized as:
<ul>
 	<li><b>core HIL components:</b> those parts strictly necessary to realize the HIL architecture</li>
 	<li><b>system drivers</b>: pluggable implementations of a standard interface to a specific external system (e.g. network switch control)</li>
 	<li><b>optional services</b>: HIL-related services which can be overridden on a per-allocation basis</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Project Team</strong></span></h3>
<strong><span style="text-decoration: underline;">Core Project Team</span></strong>
<ul>
 	<li>Naved Ansari, Mass Open Cloud at Boston University <a href="mailto:naved001@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Ian Denhardt, Mass Open Cloud</li>
 	<li>Peter Desnoyers, Northeastern University <a href="mailto:pjd@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Jason Hennessey, Boston University <a href="mailto:henn@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Kyle Hogan, Boston University <a href="mailto:klhogan@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Shuwen (Jethro) Sun</li>
 	<li>Sahil Tikale, Boston University <a href="mailto:tikale@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<strong><span style="text-decoration: underline;">Contributors</span></strong>
<ul>
 	<li>Ryan Abouzahra, former USAF</li>
 	<li>Jonathan Bell, Boston University <a href="mailto:jbell@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Logan Bernard, Boston University</li>
 	<li>Rohan Garg, Northeastern University</li>
 	<li>Orran Krieger, Boston University <a href="mailto:okrieg@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Zhaoliang Liu, former Northeastern University</li>
 	<li>Nick Matsuura, USAF</li>
 	<li>Apoorve Mohan, Northeastern University <a href="mailto:apoorve@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Andrew Mohn, former Boston University <a href="https://www.linkedin.com/in/andrewmohn">[icon name="linkedin-square" class="" unprefixed_class=""]</a></li>
 	<li>Kristi Nikola, Boston University <a href="mailto:knikolla@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>George Silvis, Boston University <a href="mailto:gsilvis@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Ron Unrau</li>
 	<li>Valerie Young, former Boston University</li>
 	<li>Hang Xu, Boston Univeristy <a href="mailto:xuh@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>George Silvis III</li>
 	<li>Ravisantosh Gudimetla</li>
 	<li>Jonathan Bernard</li>
 	<li>Abhishek Raju</li>
 	<li>Zespre Schmidt (National Chiao Tong University, Taiwan)</li>
 	<li>Ritesh Singh</li>
</ul>
<strong><span style="text-decoration: underline;">Team Lead</span></strong>
<ul>
 	<li>Jason Hennessey, Boston University [<a href="mailto:henn@bu.edu">Email</a>]</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Progress through June 2016</strong></span></h3>
<ul>
 	<li>Brocade support for Engage1 cluster</li>
 	<li>Implementing a driver mechanism for Out of Band Management systems, like IPMI. This is prep work for supporting others (like Dell iDRAC) as well as recursive HIL</li>
 	<li>ATLAS team ran a successful prototype on top of HIL</li>
 	<li>basic auth</li>
 	<li>basic logging</li>
 	<li>Additional query functions to support easier use of HIL</li>
 	<li>Prototyped Ironic and MaaS managing nodes under HIL</li>
 	<li>Created Continuous Integration tests to improve code quality by testing every Pull Request on github before it is reviewed/integrated</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Progress July - September 2016
</strong></span></h3>
<ul>
 	<li>Network(HIL abstraction) access control supported</li>
 	<li>Keystone Authentication integrated</li>
 	<li>Additional query parameters/functions to support easier use of HIL</li>
 	<li>Prototyped Ironic and MaaS managing nodes under HIL</li>
 	<li>Documentation upgrade - developer guidelines to help new developers come on board</li>
 	<li>Incorporation changes to support production level usage</li>
 	<li>Brocade SDN switches supported for Engage1 cluster</li>
 	<li>Implementing driver mechanism for Out of Band Management systems, like IPMI. This is prep work for supporting others (like Dell iDRAC) as well as recursive HIL</li>
 	<li>ATLAS team ran a successful prototype on top of HIL</li>
 	<li>Logging mechanism</li>
 	<li>Created Continuous Integration tests to improve code quality by testing every Pull Request on github before it is reviewed/integrated</li>
</ul>
<h3><span style="text-decoration: underline;"><strong>Progress September 2016 - April 2017
</strong></span></h3>
<ul>
 	<li>A <a href="#papers">paper presented</a> at ACM SoCC! <em>HIL: Designing an Exokernel for the Data Center</em></li>
 	<li>Merged 47 <a href="https://github.com/CCI-MOC/hil/pulls?utf8=%E2%9C%93&amp;q=is%3Apr%20is%3Amerged%20%20merged%3A%3E2016-09-01%20">pull requests</a></li>
 	<li>Began weekly hacking sessions with the Secure Cloud and BMI teams in order to spurn collaboration and Get Stuff Done™.</li>
 	<li>Several improvements for stability/robustness, including bug fixes and improved: test coverage, documentation, input validation and systemd integration.</li>
 	<li>Creating attractive documentation to ease new contributors via <a href="https://hil.readthedocs.io/">ReadTheDocs.io</a></li>
 	<li>Introduced fine-grained <a href="https://hil.readthedocs.io/en/latest/networks.html">Network ACLs</a>, which enables unprivileged providers to offer network services to HIL users. For example, <a href="https://massopen.cloud/blog/bare-metal-imaging/">Bare Metal Imaging</a>.</li>
 	<li>A new key/value store for per-node metadata. Initially, this will be used to support Secure Cloud deployments, specifically documenting whitelisted/known-good measurements for firmware.</li>
 	<li>An API for setting the next boot device</li>
 	<li>Successfully deployed onto portions of the Engaging1 cluster.</li>
</ul>

<hr />

<h3> <span style="text-decoration: underline;"><strong>Upcoming</strong></span></h3>
Some features planned for the upcoming months include:
<ul>
 	<li>Improve network drivers: using SNMP may give compatibility to a wide range of switches.</li>
 	<li>Improved CLI with tab completion</li>
 	<li>Examining inclusion of a driver for Intel OmniPath</li>
 	<li>Complete Ironic/MaaS prototype, propose upstream changes</li>
 	<li>A reset_port function, which will document in code initial port config and enable maintenance like replacing switches or undoing manual adjustments.</li>
 	<li>Propose HIL as an Open Stack project</li>
 	<li>VPN replacement for Headnode functionality: let’s users run headnodes from anywhere instead of relying on HIL’s built-in</li>
 	<li>Client library to give applications a python API to use to write programs against HIL</li>
 	<li>Async API allows scripts to check status on running operations</li>
 	<li>Putting leasing script into production</li>
</ul>
&nbsp;

<hr />

<h3 id="papers"><strong><span style="text-decoration: underline;">Papers</span></strong></h3>
<ul>
 	<li>J. Hennessey, C. Hill, I. Denhardt, V. Venugopal, G. Silvis, O. Krieger, and P. Desnoyers, “Hardware as a service - enabling dynamic, user-level bare metal provisioning of pools of data center resources.,” in <i>2014 IEEE High Performance Extreme Computing Conference</i>, Waltham, MA, USA, 2014. [<a href="https://open.bu.edu/handle/2144/11221">Open BU</a>, <a href="https://open.bu.edu/bitstream/handle/2144/11221/haas.pdf?sequence=1&amp;isAllowed=y">PDF</a>].</li>
 	<li>J. Hennessey, S. Tikale, A. Turk, E. U. Kaynar, C. Hill, P. Desnoyers, and O. Krieger. 2016. <a href="https://open.bu.edu/handle/2144/19198">HIL: Designing an Exokernel for the Data Center</a>. In Proceedings of the Seventh ACM Symposium on Cloud Computing (SoCC '16). DOI: <a href="https://dx.doi.org/10.1145/2987550.2987588">10.1145/2987550.2987588</a></li>
</ul>

<hr />

<h3></h3>
<h3><strong><span style="text-decoration: underline;">Planning and Getting Involved</span></strong></h3>
To get involved in this project, you can:
<ol>
 	<li>Send email to <a href="mailto:hil-dev-list@bu.edu">hil-dev-list@bu.edu</a></li>
 	<li>Join the #moc irc channel on freenode</li>
 	<li>Start reading our documentation <a href="https://github.com/CCI-MOC/hil/">on github</a>, and contribute either code (via pull requests) or issues</li>
</ol>
&nbsp;