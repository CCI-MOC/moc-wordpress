---
ID: 1516
post_title: Networking
author: tqiu
post_excerpt: ""
layout: post
permalink: https://massopen.cloud/blog/networking/
published: true
post_date: 2016-06-24 20:21:38
---
<br><p></p>
[toc]
<h3>Vision</h3>
The vision of the MOC is to create a cloud architecture that allows providers of computing and storage resources to compete for tenant services at multiple levels, all the way down to the bare metal. Networking, however, is traditionally is viewed as a commodity, much like power or cooling, necessary for even the existence of the cloud. This view implicitly excludes networking as a resource that can be offered by multiple providers and negotiated in a market.

Our vision is to go beyond this traditional view of networking in the cloud datacenter, <strong>by creating and deploying a network architecture where multiple network providers can offer connectivity services — even at the physical layer — to tenants, and where these services can be negotiated in a marketplace. </strong>

The goal of the MOC Network Working Group is to define this vision of networking as a first-class market resource, and an architecture that implements the vision in the context of the OCX. While it is still an open question whether a market will develop for networking resources within a datacenter, our architecture should not preclude this from happening.
<h3>Motivation</h3>
But why do this, isn't providing a full-cross-sectional-bandwidth network among all machines in the datacenter enough? Our view is that although sufficient for many applications, this hinders innovation in the network itself. If, instead, multiple providers can compete for value-added network services, including at the physical layer, the architecture can become a testing ground for innovative services and technologies. New technologies that require deployment across entire paths, such as pFabric or DCTCP, or technologies that are too expensive to deploy to the entire datacenter, such as (currently) 100Gbps Ethernet or Infiniband, currently do not have a simple path for deployment. Our goal is to allow incremental deployment of these and other technologies where they make sense from the functionality and economic points of view.

</div>
<p class="p1"><span class="s1">The Internet offers a powerful analogy. Since its early prototype in </span><span class="s1">1970s, the Internet has evolved from a single-provider environment into </span><span class="s1">a successful ecosystem that can accommodate multiple network </span><span class="s1">providers to compete in a marketplace. An organization today can </span><span class="s1">physically connect to an IXP, and from there choose services from </span><span class="s1">different transit providers that compete on capacity, </span><span class="s1">reliability, connectivity, and cost. This open architecture allows </span><span class="s1">the coexistence of <a href="https://arxiv.org/abs/1302.5966">18+ low-latency providers between the New York </a></span><a href="https://arxiv.org/abs/1302.5966"><span class="s1">and Chicago exchanges</span></a><span class="s1">, or the multiple </span><span class="s1">undersea cables that share similar routes.</span></p>

<div class="column">
<h3>Status</h3>
As of May 2016, we have a preliminary design for the architecture, which centers around a network exchange component, NetEx. We have published a <a href="#yu16netex">paper</a> that describes an initial take on the architecture, as well as a prototype in Mininet. We are now working on incremental design steps to deploy a simplified version of NetEx in the MOC, allowing, among other things, multiple network service providers, and the provisioning of network resources for a given project across pods.
<h3>Members</h3>
<h4>Core Team</h4>
<ul>
	<li>(Lead) <a href="http://www.cs.brown.edu/~rfonseca">Rodrigo Fonseca</a>, Brown University <a href="mailto:rfonseca@cs.brown.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li><a href="http://www.cs.brown.edu/~dyu">Da Yu</a>, Brown University <a href="mailto:da_yu@brown.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li><a href="http://www.doc.ic.ac.uk/~lm111/">Luo Mai</a>, Imperial College London <a href="mailto:luo.mai11@imperial.ac.uk">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li>Piyanai Saowarattitada, Boston University <a href="mailto:piyanai@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<h4>Contributors</h4>
<ul>
	<li>Orran Krieger, Boston University <a href="mailto:okrieg@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li>Somaya Arianfar, Cisco Systems</li>
	<li>David Oran, Cisco Systems</li>
	<li>Peter Desnoyers, Northeastern University <a href="mailto:pjd@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li>Jason Hennessey, Boston University <a href="mailto:henn@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li>Sahil Tikale, Boston University <a href="mailto:tikale@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
	<li>Guy Fedorkow, Juniper Networks</li>
	<li>Tom Nadeau, Brocade</li>
	<li>Larry Rudolph, MIT and Two Sigma</li>
</ul>
<h3>Overview</h3>
<img class="size-medium wp-image-1338 alignright" src="http://massopen.cloud/wp-content/uploads/2016/02/netex-overview-300x250.png" alt="netex-overview" width="300" height="250" />In the OCX architecture, multiple pools of physical resources — compute and storage — are provisioned and managed by different providers. These sets are called pods, and they can be, for example, a rack-scale computer, a physical container, or a storage pod. Each pod is responsible for its internal networking, and they are inter-connected by a commodity network.

We extend this architecture by allowing multiple network providers to bridge these pods and compete by offering services in an open marketplace. To realize this, we design a network marketplace called Network Exchange (NetEx). To register in NetEx, network providers physically connect to a set of programmable Edge-of-Pod (EoP) switches. Similarly to the role of Internet Exchange Points (IXPs), these switches break a datacenter network into an inter-pod network that has alternative physical networking infrastructure exchanged in NetEx, and a closed, fast intra-pod network, e.g., a FatTree network that provides full-bisection intra-pod bandwidth.

Users interact with NetEx by submitting high-level requests for connectivity. The marketplace forwards these requests to eligible providers (i.e., those connecting the relevant pods), which, in turn, return offers consisting of priced path segments, along with their characteristics. NetEx then facilitates the transfer of payment and the provisioning of the path segments selected by users. This high-level interface allows users to obtain service without being aware of the complexities of the different underlays, and providers to only expose the minimum required information for the market operation. Providers are free to implement paths however they see fit, and to use a wide spectrum of valuation and business strategies.

We are, as far as we know, the first to propose a network marketplace extending to the physical layer for a cloud datacenter, but our design borrows liberally from the datapath mechanisms from Pathlet routing, Segment routing, and from some of the market aspects of the ChoiceNet architecture.
<h4>Integrating with the MOC</h4>
The first step works with a slightly less general definition of which traffic goes onto each path segment allocated with network service providers. Currently, MOC projects can define different VLANs as their networking abstraction. NetEx will be used initially to select path segments between two or more pods that will serve as the connection among customer VLANs provisioned in each pod.
<h3>Publications</h3>
<ul>
	<li><a name="yu16netex"></a>D. Yu, L. Mai, S. Arianfar, R. Fonseca, O. Krieger, D. Oran, "Towards a Network Marketplace in a Cloud", in USENIX HotCloud 2016. <a href="http://www.cs.brown.edu/~rfonseca/pubs/yu16netex.pdf">[pdf]</a>
<a href="https://www.usenix.org/conference/hotcloud16/workshop-program/presentation/yu">https://www.usenix.org/conference/hotcloud16/workshop-program/presentation/yu</a></li>
</ul>
<h3>Code</h3>
Initial prototype on <a href="https://github.com/0xdyu/NetEx">GitHub</a>
<h3>Getting Involved</h3>
Send <a href="mailto:rfonseca@cs.brown.edu&amp;subject='MOC Networking'">email</a> to Rodrigo Fonseca.

</div>
</div>
</div>