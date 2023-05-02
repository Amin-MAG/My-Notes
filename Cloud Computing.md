# Cloud Computing

# Introduction

Cloud computing refers to data and applications being stored and run on the cloud, rather than local computer.

## Pros and cons

- **✔️** It is cost-efficient. 
- **✔️** It is much more reliable.
- **✔️** It is scalable.

## Types

### IaaS

The cloud provider will manage
- Servers
- Virtualization
- Storage
- Networking

The user must take care of 
- Applications
- Data
- Operating System
- Middleware
- Runtime

### PaaS

In this type of cloud computing the cloud provider will manage
- Servers
- Virtualization
- Storage
- Networking
- Operating System
- Middleware
- Runtime

The user must take care of 
- Applications
- Data

### SaaS

In this type of cloud computing the cloud provider will manage almost everything
- Servers
- Virtualization
- Storage
- Networking
- Operating System
- Middleware
- Runtime
- Applications
- Data

## References

- [The Basics of Cloud Computing](https://www.lucidchart.com/blog/cloud-computing-basics)
- [Cloud Computing Explained](https://www.youtube.com/watch?v=_a6us8kaq0g)

# Digital twin technology

A digital twin is an exact, three-dimensional digital replica of a place or thing. Often, it ingests data from real-world sensors, allowing it to be continually updated with real-time operating information via the cloud.

In that way, digital twins give companies and scientists the means to analyze and experiment with real objects in hypothetical environments, seeing how potential scenarios might play out without risking resources—or in some cases, lives—in order to actually test them.

Indeed, digital twins can help optimize everything from airplanes and automobiles to skyscrapers and training programs, whether they already exist in physical form or are still in the planning stages.

### Manufacturing

Here, digital twins mirror everything from factories’ assembly lines to their loading docks. By analyzing factory performance in a digital environment, manufacturers can uncover problems and ideate potential solutions. If production is too slow, for example, a manufacturer might use a digital twin of the production line to locate potential bottlenecks and model proposed changes to the line in order to determine how those changes would affect production. If the manufacturer wants to eliminate waste, it can model changes to the production line in a similar fashion to see whether they will produce the desired results. Running “what if” scenarios like that allows manufacturers to test solutions to see if they’ll work before they invest resources into actually building them.

Digital twins also can be useful in maintenance scenarios. If a machine part breaks during production, for instance, manufacturers can duplicate the incident in a digital twin to get a better picture of its impact on the real-life production line. In doing so, they can find the source of the breakage sooner and develop workarounds if the part can’t be immediately replaced.

### Engineering

Engineers can use digital twins to build digital simulations of systems they’ve designed. An automotive engineer, for example, can run a simulation of a vehicle crashing into a wall to determine how the car would react, then make design modifications to improve its safety performance—all without spending money to build and wreck a car. An aerospace engineer can do the same thing with a rocket to test its performance without endangering astronauts who might otherwise need to be on board.

### **Healthcare**

Doctors might one day be able to create digital twins of patients using their medical information. Alongside artificial intelligence, such twins could help them diagnose diseases, prescribe medication and monitor wellness by learning patients’ normal biological rhythms and flagging deviations that might require intervention.

## References

[What Is Digital Twin Technology and How Does it Work?](https://www.nutanix.com/theforecastbynutanix/technology/designing-digital-twins-to-make-safer-products-and-services)

# Edge computing

![Untitled](Cloud%20Computing/Untitled.png)

The advent of edge computing as a buzzword you should perhaps pay attention to is the realization by these companies that there isn’t much growth left in the cloud space. Almost everything that can be centralized has been centralized. Most of the new opportunities for the “cloud” lie at the “edge.”

So, what is edge?

The word edge in this context means literal geographic distribution. Edge computing is computing that’s done at or near the source of the data, instead of relying on the cloud at one of a dozen data centers to do all the work. It doesn’t mean the cloud will disappear. It means the cloud is coming to you.

> MOST OF THE NEW OPPORTUNITIES FOR THE “CLOUD” LIE AT THE “EDGE”
> 

### **LATENCY**

One great driver for edge computing is the speed of light. If a Computer A needs to ask Computer B, half a globe away, before it can do anything, the user of Computer A perceives this delay as latency. The brief moments after you click a link before your web browser starts to actually show anything is in large part due to the speed of light. Multiplayer video games implement numerous elaborate techniques to mitigate true and perceived delay between you shooting at someone and you knowing, for certain, that you missed.

> EDGE COMPUTING HAS PRIVACY BENEFITS, BUT THEY AREN’T GUARANTEED
> 

Voice assistants typically need to resolve your requests in the cloud, and the roundtrip time can be very noticeable. Your Echo has to process your speech, send a compressed representation of it to the cloud, the cloud has to uncompress that representation and process it — which might involve pinging another API somewhere, maybe to figure out the weather, and adding more speed of light-bound delay — and then the cloud sends your Echo the answer, and finally you can learn that today you should expect a high of 85 and a low of 42, so definitely give up on dressing appropriately for the weather.

### **PRIVACY AND SECURITY**

It might be weird to think of it this way, but the security and privacy features of an iPhone are well accepted as an example of edge computing. Simply by doing encryption and storing biometric information on the device, Apple offloads a ton of security concerns from the centralized cloud to its diasporic users’ devices.

But the other reason this feels like edge computing to me, not personal computing, is because while the compute work is distributed, the definition of the compute work is managed centrally. You didn’t have to cobble together the hardware, software, and security best practices to keep your iPhone secure. You just paid $999 at the cellphone store and trained it to recognize your face.

The management aspect of edge computing is hugely important for security. Think of how much pain and suffering consumers have experienced with [poorly managed Internet of Things devices](https://www.theverge.com/2016/10/21/13362354/dyn-dns-ddos-attack-cause-outage-status-explained).

> you could probably tell me which version of Windows you’re running. But do you know which version of Chrome you have? Edge computing will be more like Chrome, less like Windows.
> 

### **BANDWIDTH**

Security isn’t the only way that edge computing will help solve the problems IoT introduced. The other hot example I see mentioned a lot by edge proponents is the bandwidth savings enabled by edge computing.

For instance, if you buy one security camera, you can probably stream all of its footage to the cloud. If you buy a dozen security cameras, you have a bandwidth problem. But if the cameras are smart enough to only save the “important” footage and discard the rest, your internet pipes are saved.

Almost any technology that’s applicable to the latency problem is applicable to the bandwidth problem. Running AI on a user’s device instead of all in the cloud seems to be [a huge focus for Apple and Google right now](https://www.theverge.com/2017/10/19/16502538/mobile-ai-chips-apple-google-huawei-qualcomm).

> COMPANIES WILL CONTROL EVEN MORE OF YOUR LIFE EXPERIENCES THAN THEY DO RIGHT NOW
> 

But Google is also working hard at making even websites more edge-y. [Progressive Web Apps](https://www.theverge.com/circuitbreaker/2018/4/11/17207964/web-apps-quality-pwa-webassembly-houdini) typically have offline-first functionality. That means you can open a “website” on your phone without an internet connection, do some work, save your changes locally, and only sync up with the cloud when it’s convenient.

Google also is getting smarter at combining local AI features for the purpose of privacy *and* bandwidth savings. For instance, [Google Clips](https://www.theverge.com/2018/2/27/17055618/google-clips-smart-camera-review) keeps all your data local by default and does its magical AI inference locally. It doesn’t work very well at its stated purpose of capturing cool moments from your life. But, conceptually, it’s quintessential edge computing.

## References

[What is edge computing?](https://www.theverge.com/circuitbreaker/2018/5/7/17327584/edge-computing-cloud-google-microsoft-apple-amazon)

# Fog computing

Fog computing is a decentralized computing infrastructure in which data, compute, storage and applications are located somewhere between the data source and the cloud. Like edge computing, fog computing brings the advantages and power of the cloud closer to where data is created and acted upon. Many people use the terms fog computing and edge computing interchangeably because both involve bringing intelligence and processing closer to where the data is created. This is often done to improve efficiency, though it might also be done for security and compliance reasons.

The fog metaphor comes from the meteorological term for a cloud close to the ground, just as fog concentrates on the edge of the network.The term is often associated with Cisco; the company's product line manager, Ginny Nichols, is believed to have coined the term. Cisco Fog Computing is a registered name; fog computing is open to the community at large.

## **Fog computing vs. edge computing**

According to the OpenFog Consortium started by Cisco, the key difference between edge and fog computing is where the intelligence and compute power are placed. In a strictly foggy environment, intelligence is at the local area network (LAN), and data is transmitted from endpoints to a fog [gateway](https://internetofthingsagenda.techtarget.com/definition/gateway), where it's then transmitted to sources for processing and return transmission.

In edge computing, intelligence and power can be in either the endpoint or a gateway. Proponents of edge computing praise its reduction of points of failure because each device independently operates and determines which data to store locally and which data to send to a gateway or the cloud for further analysis. Proponents of fog computing over edge computing say it's more scalable and gives a better big-picture view of the network as multiple data points feed data into it.

![Untitled](Cloud%20Computing/Untitled%201.png)

## **How fog computing works**

Fog networking complements -- doesn't replace -- cloud computing; fogging enables short-term [analytics at the edge](https://internetofthingsagenda.techtarget.com/photostory/450432721/IoT-predictions-2018-What-will-the-next-year-of-connectedness-bring/1/IoT-edge-analytics-brings-real-time-intelligence-to-devices), while the cloud performs resource-intensive, longer-term analytics. Although edge devices and sensors are where data is generated and collected, they sometimes don't have the compute and storage resources to perform advanced analytics and machine learning tasks. Though cloud servers have the power to do this, they are often too far away to process the data and respond in a timely manner.

In addition, having all endpoints connecting to and sending raw data to the cloud over the internet can have privacy, security and legal implications, especially when dealing with sensitive data subject to regulations in different countries. Popular fog computing applications include smart grids, [smart cities](https://internetofthingsagenda.techtarget.com/definition/smart-city), smart buildings, vehicle networks and software-defined networks.

## **Fog computing benefits and drawbacks**

Like any other technology, fog computing has its pros and cons. Some of the advantages to fog computing include the following:

- **Bandwidth conservation.** Fog computing reduces the volume of data that is sent to the cloud, thereby reducing bandwidth consumption and related costs.
- **Improved response time.** Because the initial data processing occurs near the data, latency is reduced, and overall responsiveness is improved. The goal is to provide millisecond-level responsiveness, enabling data to be processed in near-real time.
- **Network-agnostic.** Although fog computing generally places compute resources at the LAN level -- as opposed to the device level, which is the case with edge computing -- the network could be considered part of the fog computing architecture. At the same time, though, fog computing is network-agnostic in the sense that the network can be wired, Wi-Fi or even 5G.

Of course, fog computing also has its disadvantages, some of which include the following:

- **Physical location.** Because fog computing is tied to a physical location, it undermines some of the "anytime/anywhere" benefits associated with cloud computing.
- **Potential security issues.** Under the right circumstances, fog computing can be subject to security issues, such as Internet Protocol (IP) address spoofing or man in the middle (MitM) attacks.
- **Startup costs.** Fog computing is a solution that utilizes both edge and cloud resources, which means that there are associated hardware costs.
- **Ambiguous concept.** Even though fog computing has been around for several years, there is still some ambiguity around the definition of fog computing with various vendors defining fog computing differently.

![https://cdn.ttgtmedia.com/rms/onlineImages/procon_fogcomputing_mobile.jpg](https://cdn.ttgtmedia.com/rms/onlineImages/procon_fogcomputing_mobile.jpg)

## **Applications of fog computing**

There are any number of potential use cases for fog computing. One increasingly common use case for fog computing is traffic control. Because sensors -- such as those used to detect traffic -- are often connected to cellular networks, cities sometimes deploy computing resources near the cell tower. These computing capabilities enable real-time analytics of traffic data, thereby enabling traffic signals to respond in real time to changing conditions.

This basic concept is also being extended to autonomous vehicles. Autonomous vehicles essentially function as edge devices because of their vast onboard computing power. These vehicles must be able to ingest data from a huge number of sensors, perform real-time data analytics and then respond accordingly.

## References

[What is Fog Computing?](https://internetofthingsagenda.techtarget.com/definition/fog-computing-fogging)

# Hierarchical architecture

# Smart manufacturing

[Articles to read](Articles.md)

[Resource Scheduling](Resource%20Scheduling.md)

[Orchestrating Serverless Workflows](Orchestration.md)