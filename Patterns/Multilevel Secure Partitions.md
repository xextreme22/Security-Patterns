# **Multilevel Secure Partitions**

## **Intent**
Confines execution of a process in a system partition which has been assigned to a specific confidentiality or integrity level. Access from this process to other partitions (processes or data) is restricted according to the rules of a MULTILEVEL SECURITY model, where processes have sensitivity levels.

## **Example**
ChronOS is now building a web system. The system’s Web, Application, and Database Servers are separated because it is clear that the Web Server has a higher exposure to attacks. If the Web server is compromised, ChronOS will be unable to provide some business services to its clients but at least the hacker is denied immediate access to Application and Database servers where the corporate data is stored. ChronOS wants to limit the damage from any one attack and prevent the attacker erasing their steps from the logs. The normal operation of the application requires processes to request and obtain services between the elements of the system.

## **Context**
Executing processes in a computing system. Processes need to call other processes to ask for services or to collaborate in the computation of an algorithm and usually share data and other resources. The environment can be centralized or distributed. Some processes may be malicious or contain errors. 

In multilevel models data and procedures are classified into sensitivity levels and users have access to them according to their clearances. These models have been formalized in three different ways [1]: 

- The Bell-La Padula model, intended to control leakage of information between levels. 
- The Biba model, which controls data integrity. 
- The Lattice model, which generalizes the partially ordered levels of the previous models using the concept of mathematical lattices.

## **The Bell-La Padula confidentiality model** 
This model classifies subjects and data into sensitivity levels. Orthogonal to these levels, compartments or categories are defined, which correspond to divisions or groupings within each level. The classification, C, of a data object defines its sensitivity. Similarly, users or subjects in general are given clearance levels. In each level an access matrix may further refine access control. A security level is defined as a pair (classification level, set of categories). 

A security level dominates another (denoted as =>) if and only if its level is greater or equal than the other level and its categories include the other categories. Two properties, known as “no read up” and “no write down” properties, define secure flow of information: 

**Simple security (ss) property.** A subject s may read object o only if its classification dominates the object’s classification, i.e., C(s) => C(o). This is the no read-up property. 

**\*-Property.** A subject s that can read object o is allowed to write object p only if the classification of p dominates the classification of o, i.e., C(p) => C(o). This is the no write-down property. 

This model also includes trusted subjects that are allowed to violate the security model. These are necessary to perform administrative functions (e.g., declassify documents, increase a user’s clearance). 

## **The Biba integrity model** 
Biba’s model classifies the data into integrity levels (I) and defines two properties dual to the simple security and \* properties: 

**Single integrity property.** Subject s can modify object o only if I(s) => I(o). 

**Integrity \*-property.** If subject s has read access to object o with integrity level I(o), s can write object p only if I(o) =>I(p). 

The first property establishes that an untrusted subject cannot write to objects of a higher level of integrity, or she would degrade that object.

## **Problem**
Many security attacks propagate through weak parts of a system. After finding an entry point, the malicious software may access a directory or some other unit of the architecture, frequently escalating its power because it will inherit rights from the compromised units. PROTECTION RINGS can help with this problem, but they can only be applied at the operating system level because of their need for hardware support. 

The following forces affect the solution: 

- Even if one part of the system is compromised or corrupted, other units will remain unaffected; that is, attacks or errors in a partition at some level should not propagate to other levels. 
- The solution should be applicable to all architectural levels of the system, not just the operating system level. 
- The solution should be applicable to distributed environments, not just single-processor systems.

## **Solution**
Divide the architecture in such a way that transfers of control or data access from one division to another follows the Bell LaPadula and/or Biba restrictions. Assign functionality to levels according to their sensitivity.

## **Structure**
Figure 32 shows a class diagram for the solution. A Client Process is assigned by the System to a specific Partition according to their function and degree of trust. A process can call other partitions for services according to a set of Transition Rules, based on a multilevel model.

![](./Images/multilevel_secure_partitions_structure.png)

*Figure 32: Class diagram for the MULTILEVEL SECURE PARTITIONS pattern*

## **Dynamics**
Figure 33 shows a sequence diagram for a process requesting a service from a different partition (p2), than the one where it is executing (p1). Requests for services in other partitions follow the rules of the multilevel model.

![](./Images/multilevel_secure_partitions_dynamics.png)

*Figure 33: Sequence diagram for requesting a service within a partition*

## **Implementation**
The system must support a multilevel access control policy with mandatory restrictions, including data labeling and rule enforcement. Transitions from one partition to another should happen only through PROTECTED ENTRY POINTS.

## **Example Resolved**
Figure 34 shows the solution provided by HP to the ChronOS problem [2]. Now, if the web server is compromised, the attacker cannot deface web pages, cannot attack the web application server, cannot erase the log (they are all in different partitions).

![](./Images/multilevel_secure_partitions_example_resolved.png)

*Figure 34: HP's Virtual Vault architecture*

## **Consequences**
This pattern has the following advantages: 

- A compromised (taken over) or corrupted partition cannot propagate its attack or errors to other partitions. 
- The solution does not depend on hardware support and can be applied in any architectural level, from applications to the operating system, to distribution units as in a Service-Oriented Architecture (SOA). 
- The solution is particularly suitable for distributed systems because it depends only on local attributes of the procedures and data involved. 
- A multilevel model defines the relationship between subject and objects using a lattice model where it can be formally proven that the permitted accesses do not violate security restrictions. This only proves security in an abstract sense and could be affected by implementation details, but it is a good security guideline for the complete system. 
- Can lead to a more systematic definition of privileges because the levels can help in structuring them. This also makes administration simpler.

Possible disadvantages include:

- It may not be easy or even possible to place the required functions and data in the appropriate levels. This is particularly true at the application level; most commercial environments are not hierarchical. 
- It is not simple to change the levels of existing programs or data. A trusted program is needed to override the rules and move subjects or data to other partitions [1].

## **Known Uses**
- HP’s Virtual Vault [2,3,4]—This is a secure platform for Internet applications that includes a trusted HP-UX operating system (a Unix variant), an enterprise server, firewalls, and other protection devices. It was part of a secure server system, the HP Praesidium family of products and appears to be the first use of this pattern. It also reduces the root privileges and controls inheritance of rights in forked threads (See Controlled Inheritance of Rights in [5]). 
- HP’s restructuring of Unix’ Sendmail program and other system programs [6]. 
- HP’s UX-11i, the current version of HP’s operating system, also uses this approach [7].

## **See Also**
- MULTILEVEL SECURITY pattern.
- See PROTECTED ENTRY POINTS pattern. 
- See PROTECTION RINGS pattern.
- Protected Address Space (Sandbox) [5]. The MSP pattern can only control the actions from a process with respect to other partitions, the Protected Address Space pattern can define precisely which resources within a partition can be accessed by the process.

## **References**

[1] Gollmann, D. (2006). Computer security. *Wiley. Second Edition*.

[2] Rubin, C., & Arnovitz, D. (1994-1999). *Virtual Vault*. HP Praesidium. Hewlett Packard

[3] VirtualVault. (1998). Philosophy of Protection. <http://docs.hp.com/en/B5413-90027/B5413-90027.pdf>

[4] (n.d.). What makes virtual vault secure?. http://www.docs.hp.com/en/J4255- 90011/apas01.html#d0e11429 

[5] Fernandez, E. B. (2002). Patterns for operating systems access control. *Procs. of PLoP 2002*.

[6] Zhong, Q., & Edwards, N. (1998). Security control for COTS components. *Computer*, *31*(6), 67-73.

[7] Wikipedia contributors. (n.d.-b). HP-UX Operating system. Wikipedia. https://en.wikipedia.org/wiki/HP-UX 

## **Sources**
Fernandez, E. B., & laRed Martinez, D. (2008, October). Patterns for the secure and reliable execution of processes. In *Proceedings of the 15th Conference on Pattern Languages of Programs* (pp. 1-16).
