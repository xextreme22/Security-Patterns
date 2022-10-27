# **Microkernel Operating System**

## **Intent**
The MICROKERNEL OPERATING SYSTEM pattern describes how to move as much of the operating system functionality as possible from the kernel into specialized servers, coordinated by a microkernel. The microkernel itself has a very basic set of functions. Operating system components and services are implemented as external and internal servers.

## **Example**
We are building an operating system to support a range of applications with different reliability and security requirements and a variety of plugins. We would like to provide operating system versions with different types of modules: some more secure, some less so.

## **Context**
A variety of applications with diverse requirements that need to execute together sharing hardware resources.

## **Problem**
In general-purpose environments we need to be able to add new functionality with variation in security and other requirements, as well as provide alternative implementations of services to accommodate different application requirements. 

The solution to this problem must resolve the following forces: 

- The application platform must be able to cope with continuous hardware and software evolution: these additions may have very different security or reliability requirements. 
- Strong security or reliability requirements indicate the need for modules with well-defined interfaces. 
- We may want to perform different types of security checks in different modules, depending on their security criticality. 
- We would like a minimum of functionality in the kernel, so that we have a minimum of processes running in supervisor mode. A simple kernel can be checked for possible vulnerabilities, which is good for security.

## **Solution**
Separate all functionality into specialized services with well-defined interfaces and provide an efficient way to route requests to the appropriate servers. Each server can be built with different security constraints. The kernel mainly routes requests to servers and has minimal functionality.

## **Structure**
The Microkernel is the central communication for the operating system. There is one Microkernel and several InternalServers and ExternalServers, each providing a set of specialized services (Figure 133). In addition to the servers, an Adapter is used between the Client and the Microkernel or an external server. The Microkernel controls the internal servers.

![](./Images/microkernel_operating_system_structure.png)

*Figure 133: Class diagram for MICROKERNEL OPERATING SYSTEM pattern*

## **Dynamics**
A client requests a service from an external server using the following sequence (Figure 134):

1. The Adapter receives the request from the Client and asks the Microkernel for a communication link with the ExternalServer. 
1. The Microkernel checks for authorization to use the server, determines the physical address of the ExternalServer and returns it to the Adapter. 
1. The Adapter establishes a direct communication link with the ExternalServer. 
1. The Adapter sends the request to the ExternalServer using a procedure call or a remote procedure call (RPC). The RPC can be checked for well-formed commands, correct size, and type of parameters (that is, we can check signatures). 
1. The ExternalServer receives the request, unpacks the message, and delegates the task to one of its own methods. All results are sent back to the Adapter. 
1. The Adapter returns to the Client, which in turn continues with its control flow.

![](./Images/microkernel_operating_system_dynamics.png)

*Figure 134: Sequence diagram for an operating system call through the microkernel*

## **Implementation**
1. Identify the core functionality necessary for implementing external servers and their security constraints. Typically, basic functions of the operating system should be internal servers; utilities, or user-defined services should go into external servers.
1. Define policies to restrict access to external and internal servers. Clients may be allowed to call only specific servers. 
1. Find a complete set of operations and abstractions for every category of server identified. 
1. Determine strategies for request transmission and retrieval. 
1. Structure the microkernel component. The microkernel should be simple enough to ensure its security properties; for example, it should be impossible to infect it with malware. 
1. Design and implement the internal servers as separate processes or shared libraries. Add security checks in each server using the PROTECTED ENTRY POINTS pattern. 
1. Implement the external servers. Add security checks in each service provided by the servers using AUTHORIZATION and AUTHENTICATOR.

## **Example Resolved**
By implementing our system using a microkernel, we can have several versions of each service, each with different degrees of security and reliability. We can replace servers dynamically if needed. We can also control access to specific servers and ensure that they are called in the proper way.

## **Consequences**
The MICROKERNEL OPERATING SYSTEM pattern offers the following benefits: 

- Flexibility and extensibility: if you need an additional function or an existing function with different security requirements, you only need to add an external server. Extending the system capabilities or requirements also only requires addition or extension of internal servers. 
- The microkernel mediates all calls for services and can apply authorization checks. In fact, the microkernel is in effect a concrete realization of a REFERENCE MONITOR. 
- The well-defined interfaces between servers allow each server to check every request for their services. 
- It is possible to add even more security by putting fundamental functions in internal servers. 
- Servers usually run-in user mode, which further increases security. 
- The microkernel is very small and can be verified or checked for security. 

The pattern also has the following potential liability: 

- Communication overhead since all messages must go through the Microkernel.

## **Known Uses**
- The PalmOS Cobalt (Figure 135) operating system has a preemptive multitasking kernel that provides basic task management. Many applications in PalmOS do not use the microkernel services; they are handled automatically by the system. The microkernel functionality is provided for internal use by system software or for certain special purpose applications [1].

![](./Images/microkernel_operating_system_known_uses_1.png)

*Figure 135: PalmOS Microkernel combined with layered OS architecture [1]*

- The QNX Microkernel (Figure 136) is intended mostly for communication and process scheduling in real-time systems [2]. It will be used in the new RIM systems, adopting a layered architecture [3].

![](./Images/microkernel_operating_system_known_uses_2.png)

*Figure 136: QNX microkernel architecture [2]*

- Mach and Windows NT also use some form of microkernels [4].

## **Variants**
Layered Microkernel. The MICROKERNEL OPERATING SYSTEM pattern can be combined with the LAYERED OPERATING SYSTEM pattern. In this case, servers can be assigned to levels and a call is accepted only if it comes from a level above the server level.

## **See Also**
This pattern is a specialization of the Microkernel pattern [5]. As indicated, the microkernel itself can be considered as a concrete version of the REFERENCE MONITOR pattern.

## **References**

[1] Palmos. (n.d.). <http://www.palmos.com/dev/tech/overview.html>

[2] QNX Software Systems. (n.d.). <http://www.qnx.com>  

[3]  Wikipedia contributors. (n.d.). QNX. Wikipedia. <https://en.wikipedia.org/wiki/QNX> 

[4] Silberschatz, A., Galvin, P., & Gagne, G. (2008). Operating System Concepts. 8th edition. John Wiley & Sons Inc.

[5] Buschmann, F., Meunier, R., Rohnert, H., Sommerlad, P., & Stal, M. (1996). Pattern-Oriented System Architecture: a System of Patterns. Volume 1.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
