# **Software Container**

## **Intent**
A SOFTWARE CONTAINER provides an execution environment for applications sharing a host operating system, binaries, and libraries with other containers. Containers are lightweight, portable, extensible, reliable, and secure.

## **Example**
Our organization has development and testing teams working at distributed locations. We generally use cloud computing in addition to company’s local infrastructure. We need a quick and easy way to provide a standardized environment to execute and test all kinds of applications. Even if we have applications from different companies running on our system, we have to provide each one a standard platform for execution. In addition to this, necessary isolation between these applications is security requirement. We have to furnish frequent releases of our software, so the deployment process and distribution is becoming difficult. For a cost-effective operation we need to reduce overhead and improve security. Our applications execute in a few commonly used operating systems, we do not have very specific customized platform requirements.

## **Context**
Software developers/Institutions developing many applications that will execute in multiple computer systems and/or cloud-based virtual environments and using a few types of operating systems. Security, reliability, and overhead are concerns

## **Problem**
We want to be able to run applications in a self-contained environment in such a way that both (application and execution environment) can be treated as a single unit.

The solution to this problem is guided by the following forces: 

- Overhead: We want the overhead of the execution environment to be as low as possible; otherwise, we can use more flexible solutions such as virtual machines. 
- Portability: We want applications to be portable across execution environments; that is, they should be able to be moved, for example, from one location to another location, without large modifications. 
- Controlled Execution: We want to control application execution in a simple and convenient way. 
- Cost: The cost of running applications should be as low as possible in terms of resources.
- Isolation: When multiple applications are running in the same OS we want a strong isolation between them so that if one of them is malicious, compromised, or fails, attacks or errors do not propagate to other applications. 
- Opaqueness: Applications running on the same OS should not be aware of each other to ensure security. 
- Transparency: The specific environment should be transparent to the application. 
- Scalability: The number of applications sharing one type of OS should be scalable. 
- Extensibility: It should be possible to dynamically provide additional services to the hosted applications like logging/auditing, filtering, persistence, and others 
- Recovery: We need to be able to recover/replicate the application easily.

## **Solution**
Provide a runtime environment that can support the isolated execution of applications on a shared Host OS, this is a SOFTWARE CONTAINER (Figure 11). Multiple processes can share one container in special circumstance [1]. They also may share binaries and libraries with other containers. Containers provide isolated execution and extensible services to the application.

![](./Images/software_container_solution.png)

*Figure 11: Two Containers sharing one OS*

## **Structure**
Figure 12 shows the class diagram for this pattern. A Container controls a set of Applications sharing a Host OS that provides a set of Resources. An Interceptor mediates the services provided to the application by the container. Applications hosted in containers can be accessed remotely through Proxies, where the Container acts as a broker. The client interacts with the Application Proxy, which represents the application. The application interacts with the Client Proxy, which represents the client. The Container provides a set of Services to the applications. Container Images are stored in image repositories.

![](./Images/software_container_structure.png)

*Figure 12: Class diagram of the Container pattern*

## **Dynamics**
Figure 13 shows a use case to execute an application in a container. A remote client executes an application through its proxy. The container transmits client requests to the Application through the Client Proxy. In order to execute the request or access a resource, application issues an OS call. This call goes to the interceptor before it is forwarded to Host OS.

![](./Images/software_container_dynamics.png)

*Figure 13: Sequence diagram for the use case ‘Execute an application in container’*

## **Implementation**
- In a virtualized environment containers can be mixed with virtual machines or bare metal servers. In other words, we can execute applications directly in a physical processor, in a virtual processor, or in a container. 
- The Host OS should be able to have primitives to execute each process in strongly isolated execution environment; for example, Linux-based containers such as Docker use kernel namespaces and groups to isolate containers. Docker uses the libcontainer library to build implementations on top of libvirt, LXC [2,3]. In this way, resources can be isolated, and services restricted to let applications have a specific view of the operating system [4].

## **Example Resolved**
The company started using containers on their servers. They built images for their applications so that each could run in a separate container. This provided lower overhead and more security for the whole system. In addition, the use of containers and application images made distribution and deployment of applications very easy

## **Consequences**
This pattern presents the following advantages: 

- Overhead: Containers are more efficient since they do not require separate guest Oss as for the case of VMs (Compare Figure 14 with Figure 11). They will, however, be slower as compared to directly executing an application on Host OS without virtualization. 
- Portability: Containers can be executed in any processor, and they can relieve application developers and testers of worrying about application distribution. 
- Controlled Execution: Containers can control application execution as they can control and filter interactions with the Host OS. 
- Cost: Host OS is shared by multiple containers, so unlike VMs we do not need to purchase separate licenses for Guest Oss on each VM. 
- Isolation: The Container can use the facilities of the host OS to provide isolation between applications running on the same OS. This feature protects the other applications of attacks or errors in applications running in different containers. 
- Opaqueness: Applications running in separate containers on the same OS are not aware of each other, which can prevent attacks. 
- Transparency: The specific environment becomes transparent to the application when executed within a container. Changes made to the OS or Application can be handled by container modification, without affecting other containers. 
- Scalability: Containers make the system more scalable since the number of applications sharing one OS can be increased provided enough hardware resources are available. 
- Extensibility: Interceptors allow adding services such as logging/auditing, security, or others to an application.

![](./Images/software_container_consequences.png)

*Figure 14: A Virtual Machine*

Liabilities of the pattern include the following: 

- Use of containers can slow down application execution since we are using an additional layer of interceptors for indirection of messages between OS and application. However, this overhead should be smaller than using separate virtual machines. 
- Containers are meant to provide isolation between applications, so if there is a need for collaboration between applications, the task becomes more difficult if they are executing in separate containers. This can still be achieved for example, Docker provides an option to securely link containers through ‘bridge’ network [5]. 
- We can only use one OS in each container. If we need different operating systems, we can have separate sets of containers or use virtual machines. 
- Security or reliability flaws in the common OS affect all the applications running on it.

## **Known Uses**
- Docker provides portable, lightweight containers, using Linux virtualization [4]. Docker was known to have security vulnerabilities but few security feature were added recently including hardware signing for container image developers, digital signatures for authenticating images in repository and image scanning for known vulnerabilities [6]. 
- Cisco – Cisco Virtual Application Container Services automate the provisioning of virtual private data centers and deploy applications with compliant, secure containers [7]. 
- Rocket – A product of CoreOS. This container attempts to be composable, secure, open format and runtime components, and simple discoverable images [8].

In addition to these, Windows server containers and Hyper V containers supported for Windows Server 2016. [9]. Other container providers include Google, Amazon Web Services, IBM, Microsoft, and HP.

## **See Also**
- Interceptor [10]–allows services to be added transparently to a framework and triggered automatically in the present of specific events. 
- Broker [11]–the Broker structures distributed systems with separate components that interact by remote service calls. A broker coordinates communications, including forwarding requests and sending back results and exceptions. 
- REFERENCE MONITOR. In a computational environment in which users or processes make requests for data or resources, this pattern enforces declared access restrictions when an active entity requests resources. It describes how to define an abstract process that intercepts all requests for resources and checks them for compliance with authorizations 
- VIRTUAL MACHINE OPERATING SYSTEM. Provides a set of replicas of the hardware architecture (Virtual Machines) that can be used to execute (maybe different) operating systems with a strong isolation between them. 
- CONTROLLED VIRTUAL ADDRESS SPACE (Sandbox). How to control access by processes to specific areas of their virtual address space (VAS) according to a set of predefined access rights? Divide the VAS into segments that correspond to logical units in the programs. Use special words (descriptors) to represent access rights for these segments. 

A formal analysis of component containers is presented in [12].

## **References**

[1] Docker. (2015). Using Supervisor with Docker. Docker Docs. Retrieved December 17, 2015, from <https://docs.docker.com/engine/articles/using_supervisord/> 
[2] LXC. (2015). Linux Containers – LXC – Introduction. Retrieved June 9, 2015, from <https://linuxcontainers.org/lxc/introduction/> 
[3] Linux LXD. (2015). Linux Containers – LXD – Introduction. Retrieved June 9, 2015, from <https://linuxcontainers.org/lxd/introduction> 
[4] Docker. (2015). Docker. Retrieved April 10, 2015, from <http://www.docker.com/>.
[5] Container Links. (2015). Legacy container links. Retrieved December 1, 2015, from <https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/#container-linking>.
[6] Charles Babcock. (2015). Docker Tightens Security Over Container Vulnerabilities. Retrieved December 1, 2015, from [http://www.informationweek.com/cloud/platform-as-a-service/docker-tightens-security-over-container-vulnerabilities/d /d-id/1323178](http://www.informationweek.com/cloud/platform-as-a-service/docker-tightens-security-over-container-vulnerabilities/d%20/d-id/1323178).
[7] Cisco. (2015). Virtual Application Container Services. Retrieved June 22, 2015, from <http://www.cisco.com/c/en/us/products/switches/virtual-application-container-servicesvacs/index.html>
[8] Alex Polvi. (2014). CoreOS is building a container runtime, rkt. Retrieved April 25, 2015, from <https://coreos.com/blog/rocket/>.
[9] Simon Bisson. (2015). First look: Run VMs in VMs with Hyper-V containers. Retrieved December 9, 2015, from <http://www.networkworld.com/article/3013224/virtualization/first-look-run-vms-in-vms-with-hyper-v-containers.html>.
[10] Schmidt, D. C., Stal, M., Rohnert, H., & Buschmann, F. (2000). *Pattern-Oriented Software Architecture Volume 2: Patterns for Concurrency and Networked Objects* (Volume 2 ed.). Wiely*.*
[11] Buschmann, F., Meunier, Regine, Rohnert, H., Sommerlad, P., & Stal, M. (1996). *Pattern-Oriented Software Architecture Volume 1: A System of Patterns* (Volume 1 ed.). Wiley.
[12] Sridhar, N., & Hallstrom, J. O. (2006, March). A behavioral model for software containers. In *International Conference on Fundamental Approaches to Software Engineering* (pp. 139-154). Springer, Berlin, Heidelberg.

## **Sources**
Syed, M. H., & Fernandez, E. B. (2015, October). The software container pattern. In *Proceedings of the 22nd Conference on Pattern Languages of Programs* (pp. 1-7).
