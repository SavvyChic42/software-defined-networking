If you haven’t heard about Software-defined Networking and why it matters, you’re not alone. Although SDN has been one of the major breakthroughs of the computer industry in the past decade, few people know what it actually is and why it matters. In this post, I will try to explain what SDN is, where the idea comes from, what it actually solves and how it would affect the IT industry in general.

Acknowledgement:Introduction to SDN (Software-defined Networking)https://youtu.be/DiChnu_PAzA?si=Aw9IyBNcGiPkd6kq

Context: Where does all of this come from?
In the early-2010s, networking was considered to be a pretty much dead field of research. People were starting to leave the field as it seemed that it had entered a phase where not much was left to be explored and doing meaningful research was unnecessarily hard. While most other fields in computer science had extremely agile and open communities, networking remained a world of boring hundred page long RFCs and an industry dominated by a few big players, where things changed very slowly.

Doing research in networking was hard. Say for example, one were to experiment with a new protocol, say, something to replace IP. How would one even do that? Software simulations were one way to go, but it wouldn’t be simulating the real internet. The barrier was the fact that networks were not programmable. Programming the behavior of a network, at least in the sense that we do regular programming, was next to impossible because networking was done by independent autonomous self-contained boxes in a distributed fashion. To program the network, you had to program all these boxes (which is said much easier than done) so that the collective behavior of them would satisfy a certain requirement.

As you can imagine, this is an undesirable place for any researcher. Nowadays, most of us are used to programming, testing, identifying problems, fixing, and repeating this procedure until we can achieve what we want. To better understand how we ended up there and where the idea of SDN comes from, one should first understand how traditional networking used to be done.

The Status Quo Ante
As mentioned before, networking is generally done through these network appliances or boxes that process and forward messages. If you have ever done anything that requires two computer systems to talk, whether it was a multi-node application or something that required socket programming or anything remotely similar to those, you know things can go south way too easily:

These boxes have to agree on different things (e.g. topology discovery, routing, IP ranges, etc.) through detailed protocols, and this is where the problem begins. A small deviation in these implementations of the protocols can cause catastrophic errors hindering the communication between network boxes. That is why we have these standardizing bodies and committees that are concerned with extremely elaborate protocol design and consensus on these specification throughout the industry.

Protocols, 200-page RFCs, other boring stuff
So lets see how this all worked out for the consumer. Say some large enterprise wanted a new feature in their networks, say something like a more flexible way for them to reconfigure the networks in their data centers quickly. How would they achieve this? Well, most likely:

They would have had to communicate their requirements to their vendors
The new needs would have had to go through a standardizing body
The standardized protocols and conventions would be implemented and integrated into the proprietary operating systems in the network appliances
This process is so lengthy and inefficient that in the end, the market needs might change before the required features are production-ready.

Take the OSPF (Open Shortest Path First) protocol as an example. OSPF is widely used in ISP networks for topology discovery and routing. While the heart of the protocol is using Dijkstra’s algorithm in a network to find optimal routing paths, every router in the network is part of that computation. The implementation is done in every router, and the collective behavior is realized by each of these routers acting autonomously and running the logic of the protocol separately and playing their part. At the end of the day, every router is running an implementation of Dijkstra’s algorithm for itself as if there is no central authority to tell them what to do. That is what we call a distributed control plane.

Out of the 244 pages of the OSPF RFC, about 4–5 pages are about Dijkstra’s algorithm. All the rest is about maintaining this distributed state and communicating effectively between the nodes. This is a perfect example of how the distributed attribute adds an enormous amount of complexity to a problem.

This chapter provides a historical perspective on enterprise networking, highlighting existing solutions and their limitations. We delve into the evolution of networking technologies and discuss the challenges
faced by traditional solutions like MPLS. The emergence of SD-WAN as an alternative to MPLS is explored, along with its benefits for enterprise networking. We emphasize the importance of thoroughly
analyzing the security of SD-WAN solutions before their widespread adoption. Finally, we briefly overview the literature used as a reference for conducting the analysis.
The discussion includes the evolution of enterprise networking, from early solutions like point-to-point leased lines and frame relay services, to the current era of SD-WAN. The limitations of MPLS, such as
high cost and low bandwidth compared to public internet, are examined. The thesis also addresses the difficulties faced by service providers in offering MPLS to enterprises that increasingly rely on public
clouds for their infrastructure.
Moreover, the chapter highlights the advantages of SD-WAN as a modern enterprise networking solution. SD-WAN’s capability to simplify the configuration and management of enterprise networks by
Virtualizing networking services is discussed. We explore how SD-WAN addresses the challenges faced by traditional solutions and the necessity of conducting a thorough security analysis of SD-WAN solutions to ensure their robustness and reliability.
Finally, the literature review section provides an overview of the references used in the analysis, covering a range of sources that discuss the evolution of enterprise networking, the rise of SD-WAN, and the importance of security analysis in adopting new networking technologies.
Enterprise networks are private networks designed to connect an organization’s branches securely, enabling the sharing of computer resources across various locations, such as company sites, stores,
headquarters, and cloud data centers. These networks form a communication backbone, integrating all computers, mobile devices, and other associated equipment within the organization. This facilitates
seamless interoperability and efficient data management across the enterprise.

Enterprise networks can encompass both local area networks (LANs) and wide area networks (WANs), as shown in the diagram depicting a simple enterprise network with its headquarters, branches, and data
center connected. Historically, enterprise networks utilized telecom networks originally designed for voice communication, employing low-bandwidth modems to transmit data.
With the advent of digitization and the increasing use of the public internet in the 1990s, enterprises began adopting virtual private networks (VPNs) that leveraged existing public infrastructure while incorporating encryption to safeguard data traffic from unauthorized access. Initially, VPNs relied on frame relay services to establish private networks, but this was later supplanted by the widely adopted MPLS protocol, which continues to be the standard for modern enterprise networks.


Enterprise Network
From the mid-2000s, enterprise networks began adopting the MPLS (Multiprotocol Label Switching)

protocol for their private networks. MPLS enhances data speed and overall network performance. In traditional networking, routers determine the path for data packets based on the information contained in the network layer header of each packet. This process involves analyzing the IP header to decide the next hop for the packet, operating at layer 3 of the network.

With MPLS, the routing decision is made based on pre-assigned labels rather than the IP header. These labels are added to data packets by the ingress router as they are forwarded into the operator’s network.MPLS-enabled routers quickly process packets by examining the labels and forwarding them to the next router according to predetermined rules. This label-based forwarding mechanism eliminates the need for time-consuming analysis of the IP header at each hop. The MPLS header, which contains the labels, is added in front of the IP header. A sample MPLS network, showcasing customer edge routers, illustrates how the protocol optimizes data flow and enhances network performance. This example demonstrates the efficiency and versatility of MPLS in enterprise networking scenarios, improving overall connectivity and communication within the organization. The origins of MPLS trace back to Ipsilon Networks’ proposal of a flow management protocol, which operated solely over Asynchronous Transfer Mode (ATM). Later, Cisco introduced tag switching, a more versatile approach that wasn’t restricted to ATM alone. Cisco eventually renamed it to label switching and presented it to the Internet Engineering Task Force (IETF) for standardization.

With contributions from various vendors, MPLS evolved to support multiple networking protocols like T1/E1, ATM, Frame Relay, and DSL. Due to its compatibility with various protocols, it was named multiprotocol switching (MPLS). MPLS offers numerous advantages compared to per-packet routing, including high-speed data transmission, scalability, and the ability to function over various underlying protocols. These factors have led to the widespread adoption of MPLS-based solutions in enterprise networks. Service providers, such as Sonera in Finland, offer MPLS-based solutions like Data Net to their customers. Sonera’s data net is a market-leading example of an MPLS-based service in Finland, showcasing the protocol’s reliability and effectiveness in enterprise networking environments.


MPLS Network
This distributed network design philosophy has its root in the cold-war era when most of the research budget for networking came from the US Department of Defense. They wanted to build networks that were able to withstand a nuclear attack so if parts of the network were wiped out, it was expected that everything else would work. But this lack of centralized control would make it impossible to build programmable networks i.e. a network whose behavior can be easily modified just like computer code. So 30 years later, it had become evident that this approach just wasn’t cutting it.


Proprietary to the bone
Contrary to most areas in the industry where open platforms and community-driven projects play an important role, networking had remained a world of proprietary software running on custom ASICs/FPGA.

The idea that you purchase network devices that have little to no customization ability, run the vendor’s own closed-source proprietary firmware and software and the hardware is carefully crafted to match that software (and nobody else’s) is very similar to how computing itself was done up until the 80s, when big manufacturers like IBM shipped their (giant) hardware with their own customized closed-source operating systems and even applications.

Indirect Programming
As mentioned before, when you have distributed control, to get something done, you would need protocols which different manufacturers would have to implement in their software or hardware. The network boxes run what is called a bucket of protocols, each serving a specific purpose and sometimes acting as a point band-aid.

This is obviously an obstacle for research and development. Distributed protocols are by nature hard to program. It is fundamentally different from running some code, identifying the problems, making modifications, and then running it again as we would do in writing a normal piece of code. One way to put it, is that distributed protocols entail indirect programming instead of directly programming elements of security, routing, traffic-engineering, policy, etc. When doing this genre of programming, we are talking to each and every node and not the network itself. This is due to a lack of central management in traditional networks.

Take IP multi-casting as an example. The idea has been around since the dawn of time and we never really got there to a point where it would be widespread used. It is very hard to design a universal solution where the network switches would be able to independently figure out when they have to multi-cast.

Vision: Programmable Networks
Software-defined networking is about making networks programmable. We want to move from this world of black-box proprietary hardware running protocols that take ages to be developed, approved and adopted by the industry to an age of open interfaces that would allow us to change the behavior of our networks easily, run experiments, and modify them again and again just like we do programming for an Android or iOS app. To achieve this:

Networking is making a shift from customized hardware (ASICs/FPGA) running proprietary code to open software run on commodity hardware (merchant silicon).

This development is comparable to what happened to computers themselves in the 80s. Instead of big manufacturers like IBM developing the hardware, the OS, the applications and shipping everything altogether, the industry shifted towards operating systems being developed separately to be run on all hardware platforms through a set of standard interfaces (x86 ISA for example) and in turn providing easy-to-use interfaces to develop applications (such as POSIX) and hence acting as a hub for applications to run.

Today, the same concept is coming to networks. Instead of big companies like Cisco, delivering their hardware appliances with closed-source hardly modifiable static functions, these appliances would implement an interface (called the south-bound protocol) which can be used by something called the SDN controller or the network OS to set the behavior of the system. But we’ll get to that.

Networking is becoming part of computing

Distributed protocols are by nature hard to program. It is fundamentally different from running some code, identifying the problems, making modifications, and then running it again as we would do in writing a normal piece of code. One way to put it, is that distributed protocols entail indirect programming instead of directly programming elements of security, routing, traffic-engineering, policy, etc.


In traditional networking, each new feature has to be implemented indirectly through a distributed protocol. In our vision of future networks, we would like to directly program the network through an abstraction API.

In our vision of next-generation networks, we want to have certain APIs that would allow this kind of direct programming. The brains of the network would be moved up above these APIs where it is determined what the network does. Hence, instead of programming the control aspect of the network in each box, we would have a centralized way to directly control the entire network. All of these, in essence, mean that networking is moving towards a software discipline and is becoming part of computing.

Introducing SDN
The most fundamental idea in SDN or Software-defined Networking, is separating control plane from data plane. If you have no idea what that means, don’t freak out! We’ll make it clear through this section.

Dumb Switches and Smart Controllers

The structure of a traditional router / switching device

In every traditional switching device, there usually is a smart part and a dumb part. The dumb part is called the switching fabric which makes low-latency input-to-output switching possible. The smart part or the control usually maintains some sort of a switching table based on some logic. It might be running a protocol such as OSPF or IS-IS, it might be configured directly using an IOS shell, etc. Modern routers run advanced operating systems and are able to perform complicated control logic. In turn, the switching fabric is configured by this control unit. So there are generally two classes of functions in networking:

Data Plane (→ switching fabric) which forwards packets through the network
Control Plane (→ control unit) which decides how the forwarding should be done
The idea behind SDN is the physical separation of these two functions. So instead of each switch or router running complicated control logic to decide how it should forward packets, we take that control functionality from each network node and put them in one logically centralized unit called the SDN Controller.


When we say the SDN controller is logically centralized, it means that the controller is an architectural unit, not necessarily a physical network box. In an SDN, the controller might be running in a separate box, on a virtual machine somewhere, or it might even be running on multiple machines. That is an implementation decision. But from the architectural perspective, it is a centralized single unit which maintains a global view of the network and is able to control all nodes inside the network. Ergo, it is logically centralized and not necessarily physically centralized.

Three-layer design
The SDN controller is the brains of the network. Its function is comparable to what the operating system does in a computer: it acts as a mediator between the applications and the network resources.

South-bound Protocol

The network switches in an SDN are simply viewed as forwarding devices. All they do is that they match packets against certain patterns (defined for them by the controller) and perform an action based on the match (again, instructed to them by the controller). There is of course, a need for a standard way for the controller to talk to these devices and that is called the south-bound protocol. OpenFlow is one such protocol. You can think of OpenFlow as the x86 instruction-set of the network as it is used to do low-level programming of the network forwarding devices.


An example of OpenFlow rules matching against certain fields in each frame/packet and giving an instruction for each match

OpenFlow is standardized by the Open Networking Foundation and it is supported in many hardware and software switches today. An OpenFlow controller, installs certain forwarding rules in each switch. The rules are comprised of a priority number, specific values for each packet/frame fields and an action to be performed if the fields match.

North-bound API

The controller also provides a user-friendly API for applications to use, so all the brain-work can actually be moved up above that API. This API is usually called the north-bound API and it is comparable to the POSIX interface in Unix-based OS-es: It defines easy-to-use high-level interfaces that applications can use for programming the entire network and let the controller figure out how those requests should be translated into the low-level instructions to network resources, i.e., south-bound protocol calls.

This way, the controller can act as an abstraction layer to the entire network. Applications do not have to be concerned with low-level programming of each network node as long as they are talking to the controller through the NB API, which is most commonly a REST API.

This is usually referred to as the three layer architecture of SDN:


Up above, you have the applications. That is where all the cool stuff happens and also where network management happens. For example if you want to develop a new routing algorithm using the Bellman-Ford algorithm, you can write your own application that uses the controller’s north-bound API to install the proper forwarding rules in each network switch. It can also receive information such as the network topology from the controller as the controller has a global view of the network.

In the middle, lies the SDN Controller which is pivotal to this whole operation. It provides high-level interfaces to the applications and encapsulates all the low-level details of how those API calls would be translated into low-level instructions to network devices. The controller uses a standard protocol to configure network devices and receive information from them. That is called the south-bound protocol e.g. OpenFlow.

In the lowermost layer are the network resources. All the switching devices reside there and are configured by the SDN controller. These devices don’t really care what their switching rules really mean. The brains of the network has been moved to the centralized control plane.

That’s great, but what does it all mean for me?
With SDN becoming more and more prevalent, a new genre of possibilities is open to explore as it lifts many barriers for doing actual research and innovation for networks. Long story short, you can now implement your ideas for networks without thousands of dollars worth of equipment and years of experience with esoteric networking technologies.

Before SDN, small players and independent researchers had little chance to enter the market because there were simply too many barriers to entry. Today, you can set up a mini-net on your laptop, implement and test your ideas for new network features or optimizations, publish your features to GitHub, have the community extend your work and so on and so forth. The take-away point of this is, you can deal with networking the way you deal with other kinds of computing and it is no longer so separate from everything else.

An interesting venue in networking that has received a lot of attention lately is the use of Machine Learning for tasks traditionally done partly or completely manually. SDN is a great enabler of such projects. When networks can be programmed just like any other computer infrastructure, developing novel and creative network management methods and transferring our experience in other fields of CS to networking becomes much easier.

TL;DR
Networking is in a transition period from its former static hard-to-program state to a new generation of networks that are highly programmable and this allows easy development of applications for network management. The heart and soul of SDN is about creating a logically centralized source of truth in the network that maintains a global view of it and can act as a mediator between applications and network resources allowing easy direct programming of the entire network instead of going through rigorous protocol design and tedious distributed implementations processes.





