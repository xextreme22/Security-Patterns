# **Capability**

## **Intent**
The CAPABILITY pattern allows controlled access to objects by providing a credential or ticket to a subject to allow it to access an object in a specific way. Capabilities are given to the principal.

## **Example**
We are designing a system that allows registered users to read or modify confidential documents. We need to verify that a specific user can access a confidential document in an efficient and secure manner. In particular, we worry that if the parts of our system that deal with access control are too large and/or distributed, they may be compromised by attackers.

## **Context**
Distributed systems in which access to resources must be controlled. The systems have a policy decision point and its corresponding policy enforcement points that enforce the access policy. A system is composed of subjects that need to access resources to perform their tasks. In the system, not every subject can access any object: access rights are defined and can be modeled as an access matrix, in which each row represents a subject, and each column represents an object. An entry of the matrix is indexed by a specific subject and a specific object and lists the types of actions that this subject can execute on this object. The system’s implementation is vulnerable to threats from attackers that may compromise its components.

## **Problem**
In some of these systems the number of subjects and/or objects can be large. In this case, the direct implementation of the access matrix can use significant amounts of storage, and the time to search a large matrix can be significant.

In practice, the matrix is sparse. Subjects have rights on few objects and thus most of the entries are empty. How can we implement the access matrix in a space- and time-efficient way? 

The solution to this problem must resolve the following forces:

- The matrix may have many subjects and objects. Finding the rule that authorizes a specific request to an object may take a lot of time (unordered entries). 
- The matrix can be very sparse and storing it as a matrix would require storing many empty entries, thus wasting space. 
- Subjects and objects may be frequently added or removed. Making changes in a matrix representation is inefficient. 
- The time spent for accessing a centralized access matrix may result in an additional overhead. 
- A request received by a policy enforcement point indicates the requester identity, the requested object and the type of access requested. The requester identity, in particular, is controlled by the requester, and so may be forged by a malicious user. 
- The size of the units that can create and/or modify the policies (such as policy administration points) has an impact on the security of the system. Minimizing their size will reduce their chance of being compromised by attackers.

## **Solution**
Implement the access matrix by issuing a set of capabilities to each subject. A capability specifies that the subject possessing the capability has a right on a specific object. Policy enforcement points and the policy decision point of the system enforce the access policy by checking that the capability presented by the subject at the access time is authentic, and by searching the capability for the requested object and access type. Trust a minimum part of the system – create a unique capability issuer that is responsible for issuing the capabilities. The capabilities must be implemented in a way that allows the policy decision point to verify their authenticity, so that a malicious user cannot forge one.

## **Structure**
Figure 154 illustrates the solution. In order to protect the Objects, a CapabilityProvider, the minimum trusted part of our system, issues a set of Capabilities to each Subject by using a secure channel. A Capability contains a set of Rights that the Subject can perform on a specific Object. A Subject accesses an Object only if a corresponding Right exists in one of the Subject’s Capabilities. At execution time, the PDP is responsible for checking the Capability’s authenticity andXXX searching the Capability for both the requested Object and the requested accessType in order to make an access decision.

![](./Images/capability_structure.png)

*Figure 154: Class diagram for the CAPABILITY pattern*

## **Dynamics**
Figure 155 shows a sequence diagram describing the typical use case of ‘Request object access’. The Subject requests access to an Object by including a corresponding Capability. The request is intercepted by a PEP, which forwards the request to the PDP. It can then check that the Capability holds the accessType requested by the Subject.

![](./Images/capability_dynamics.png)

*Figure 155: Sequence diagram for the use case ‘Request object access’*

## **Implementation**
Since a capability must be unforgeable and unmodifiable, it can be implemented as hardware or software: 

- As hardware: 
  - Tags. Tagging allows for the categorization of each word as data or a capability. Then no copying should be allowed from capability to data or vice versa, no arithmetic operation should be allowed on capabilities. A disadvantage of this method is the memory waste by using tags. 
  - Segmentation. Whole segments of memory are used exclusively for capabilities or for data. No operation should be allowed between partitions of different types. A disadvantage of this is that many processes may need two segments. 
- As software: 
  - Cryptography. Usually used, the capabilities may be encrypted by the capability issuer’s key.

## **Example Resolved**
To enforce access control, we create a policy decision point and its corresponding policy enforcement points that are responsible for intercepting and controlling accesses to confidential documents. When a user logs on to the system, a robust token issuer provides a set of tokens that indicate which confidential documents are authorized. Tokens are digitally signed so that they can’t be created or modified by users. At request time, a user wishing to access a confidential document presents its token to the policy enforcement point, and then to the policy decision point, which grants them access to the document. If a user does not present a token corresponding to the document and the access mode, access is refused.

## **Consequences**
The CAPABILITY pattern offers the following benefits: 

- Because the capability is sent together with the request, the time spent for accessing an authorization is much less than the time that would have been spent searching a whole matrix or searching an ACCESS CONTROL LIST (ACL). 
- The time spent accessing a capability at request time is less than the time that would have been spent accessing a centralized matrix. 
- The part of the system that we need to trust is minimal. The capability provider is only responsible for issuing capabilities to the right users at an initial time. 
- It is harder for malicious users to forge or modify capabilities, since a capability provides a way to verify its authenticity. 

The pattern also has some potential liabilities: 

- The administration of the objects is more difficult: The addition of an object implies the issuing of capabilities to every authorized user. 
- When the environment is heterogeneous, the administration of the rights is more complex. There is no straightforward way to revoke a right, since users are in control of the capabilities they have acquired. A solution could be to add a validity time to each capability, or through indirection, or by using virtual addresses [1]. 
- The right is transferable: that is, a capability can be stolen and replayed by (or given to) a malicious user. (This is not the case in OSs in which accesses to the capabilities are also controlled by the TCB, but those need the support of special hardware.)

## **Known Uses**
- Most of the capability-based systems are operating systems. Usually hardware assistance is needed; for example, capabilities are placed in special registers and manipulated with special instructions (Plessey P250), or they are stored in tagged areas of memory (IBM 6000). 
- Many distributed capability-based systems have been researched and described [2,3,4,5]. Among those, Amoeba [5] is a distributed operating system in which multiple machines can be connected together. It has a microkernel architecture. All objects in the system are protected using a simple scheme. When an object (representing a resource) is created, the server doing the creation constructs a capability in the form of a 128-bit value and returns it to the caller. Subsequent operations on the object require the user to send its capability to the server to both specify the object and to prove the user has permission to manipulate the object. Capabilities are protected cryptographically to prevent tampering. The Symbian operating system uses capabilities [6].

## **See Also**
- The PEP and PDP are from the ACCESS CONTROL LIST pattern. The ACCESS CONTROL LIST pattern is another way to implement the access matrix. 
- Capabilities can be implemented into the VAS (virtual address space) using segmentation. 
- The PEP is just a REFERENCE MONITOR. 
- Access Matrix [7], RBAC are models that can be implemented using ACLs. CREDENTIAL pattern is a type of capability.

## **References**

[1] Anderson, R. (2008). Security Engineering. 2nd edition. John Wiley & Sons, Inc.

[2] Johnson, H. L., Koegel, J. F., & Koegel, R. M. (1985, October). A secure distributed capability based system. In *Proceedings of the 1985 ACM annual conference on The range of computing: mid-80's perspective: mid-80's perspective* (pp. 392-402).

[3] Donnelley, J. E. (1976, August). A Distributed Capability Computing System (DCCS). In *ICCC* (pp. 432-440).

[4] Sandhu, R. S., Coyne, E. J., Feinstein, H. L., & Youman, C. E. (1996). Role-based access control models. *Computer*, *29*(2), 38-47.

[5] (n.d.). Amoeba Operating System. Retrieved October 27, 2012, from [www.cs.vu.nl/pub/amoeba/](http://www.cs.vu.nl/pub/amoeba/) 

[6] Heath, C. (2006). *Symbian OS: platform security*. John Wiley & Sons.

[7] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
