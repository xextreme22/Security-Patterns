# **Access Control List**

## **Intent**
The ACCESS CONTROL LIST (ACL) pattern allows controlled access to objects by indicating which subjects can access an object and in what way. There is usually an ACL associated with each object.

## **Example**
We are designing a system in which documents should be accessible only to specific registered users, who can either retrieve them for reading or submit modified versions. We need to verify that a specific user can access the document requested in a rapid manner.

## **Context**
Centralized or distributed systems in which access to resources must be controlled. The systems comprise a policy decision point and policy enforcement points that enforce the access policy. A system has subjects that need to access resources to perform tasks. In the system, not every subject can access any object: access rights are defined and can be modeled as an access matrix, in which each row represents a subject, and each column represents an object. An entry in the matrix is indexed by a specific subject and a specific object and lists the types of actions that this subject can execute on this object.

## **Problem**
In some systems the number of subjects and/or objects can be large. In this case, the direct implementation of an access matrix can use significant amounts of storage, and the time used for searching this large matrix can be significant. 

In practice, the matrix is sparse. Subjects have rights on few objects and thus most of the entries are empty. 

How can we implement the access matrix in a space- and time-efficient way? 

The solution to this problem must resolve the following forces: 

- The matrix may have many subjects and objects. Finding the rule that authorizes a specific request to an object may take a lot of time, as entries are unordered. 
- The matrix can be very sparse: storing it as a matrix would require storing many empty entries, thus wasting space. 
- Subjects and objects may be frequently added or removed. Making changes in a matrix representation is inefficient. 
- The time spent for accessing a centralized access matrix may result in an additional overhead. 
- A request received by a policy enforcement point indicates the requester identity, the requested object and the type of access requested. The requester identity, in particular, is controlled by the requester, and so may be forged by a malicious user.

## **Solution**
Implement the access matrix by associating each object with an ACCESS CONTROL LIST (ACL) that specifies which actions are allowed on the object, by which authenticated users. Each entry in the list comprises a subject’s identifier and a set of rights. Policy enforcement points enforce the access policy by requesting the policy decision point to search the object’s ACL for the requesting subject identifier and access type. For the system to be secure, the subject’s identity must be authenticated prior to its access to any objects. Since the ACLs may be distributed, like the objects they are associated with, several policy administration points may be responsible for creating and modifying the ACLs.

## **Structure**
Figure 150 illustrates the solution. To be protected, an Object must have an associated ACL. This ACL is made up of ACLEntries, each of which contains a set of Rights permitted for a specific authenticated Subject. An authenticated Subject accesses an Object only if a corresponding Right exists in the Object’s ACL. For security reasons, only the PDP can create and modify ACLs. At execution time, the PDP is responsible for searching an Object’s ACL for a Right in order to make an access decision.

![](./Images/access_control_list_structure.png)

*Figure 150: Class diagram for the ACCESS CONTROL LIST pattern*

## **Dynamics**
Figure 151 shows a sequence diagram describing the typical use case for ‘Request object access’. The authenticated Subject’s request for access to an Object is intercepted by a PEP, which forwards the request to the PDP. It can then check that the ACL corresponding to the Object contains an ACLEntry which corresponds to the Subject, and which holds the accessType requested by the Subject.

![](./Images/access_control_list_dynamics.png)

*Figure 151: Sequence diagram for use case ‘Request object access’*

## **Implementation**
A decision must be made regarding the granularity of the ACLs. For example, it is possible to regroup users, such as the minimal access control lists in UNIX. It is also possible to have a finer-grained access control system. For example, the extended access control lists in UNIX allow specified access not only for the file’s owner and owner’s group, but also for additional users or groups.

The choice of access types can also contribute to a finer-grained access control system. For example, Windows defines over ten different permissions, whereas UNIX-like systems usually define three. 

A creation/inheritance policy must also be defined: what should the ACL look like at the creation of an object? From what objects should it inherit its permissions?

ACLs are pieces of information of variable length. A strategy for storing ACLs must be chosen. For example, in the Solaris UFS file system, each inode has a field called i\_shadow. If an inode has an ACL, this field points to a shadow inode. On the file system, shadow inodes are used like regular files. Each shadow inode stores an ACL in its data blocks. Linux and most other UNIX-like operating systems implement a more general mechanism called extended attributes (EAs). Extended attributes are name/value pairs associated permanently with file system objects, similar to the environment variables for a process [1].

## **Example Resolved**
To enforce access control, we create a policy decision point and its corresponding policy enforcement points, which are responsible for intercepting and controlling accesses to the documents. For each document, we provide the policy decision point with a list of the users authorized to access the document and how (read or write). At access time, the policy decision point is able to search the list for the user. If the user is on the list with the proper access type, it can grant access to the document, otherwise it will refuse access. 

In our distributed system, we make sure that only authenticated users — that is, users who provided a valid credential — can make requests.

## **Consequences**
The ACCESS CONTROL LIST pattern offers the following benefits: 

- Because all authorizations for a given object are kept together, we can go to the requested object and find out if a subject is there. This is much quicker than searching the whole matrix. 
- The time spent accessing an ACL is less than the time that would have been spent accessing a centralized matrix. 
- Access to unauthorized objects using forged requests on behalf of legitimate subjects is not possible, because we make sure that the requests are from only authenticated subjects. 

The pattern also has the following potential liabilities: 

- The administration of the subjects is rendered more difficult: the deletion of a subject may imply a scan of all ACLs, although this can be done automatically. 
- When the environment is heterogeneous, it needs to be adapted to each type of PEPs. PDPs and PAPs must be implemented in a different way, adding an additional development cost.

## **Known Uses**
- Operating systems such as Microsoft Windows (from NT/2000 on), Novell’s NetWare, Digital’s OpenVMS and UNIX-based systems use ACLs to control access to their resources. 
- In Solaris 2.5, file ACLs allow a finer control over access to files and directories than the control that was possible with the standard UNIX file permissions. It is possible to specify specific users in an ACLEntry. It is also possible to modify ACLs for a file testfile by using the setfacl command in a similar way to the chmod command used for changing standard UNIX permissions: 

*setfacl -s u::rwx,g::—,o::—,m:rwx,u:user1:rwx,u:user2:rwx testfile* 

- IBM Tivoli Access Manager for e-businesses uses ACLs to control access to the web and application resources [2]. 
- Cisco IOS, Cisco’s network infrastructure software, provides basic traffic filtering capabilities with ACLs [3].

## **See Also**
- The PEP and PDP come from the POLICY-BASED ACCESS CONTROL pattern. The CAPABILITY pattern is another way to implement the Access Matrix. 
- Access Matrix [4] and role-based access control are models that can be implemented using ACLs. 
- PEP is just a REFERENCE MONITOR pattern. 
- A variant exists oriented to centralized systems, POLICY ENFORCEMENT pattern, which leverages ad hoc data structures to enhance efficiency. 
- Acegi is a security framework for Java, used to build ACLs [5].

## **References**

[1] Grünbacher, A. (2003). {POSIX} Access Control Lists on Linux. In *2003 USENIX Annual Technical Conference (USENIX ATC 03)*.

[2] IBM. (n.d.). Tivoli Federated Identity Manager. <http://www306.ibm.com/software/tivoli/products/federated-identity-mgr/>

[3] Cisco. (n.d.). Cisco IOS Software. Retrieved June 26, 2007, from [http://www.cisco.com/en/US/products/sw/iosswrel/products_ios_cisco_ios_software_category_h ome.html](http://www.cisco.com/en/US/products/sw/iosswrel/products_ios_cisco_ios_software_category_h%20ome.html)

[4] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc.

[5] Siddiqui, B. (n.d.). Securing Java Applications with Acegi, Part 1: Architectural overvIew and Security Filters. Retrieved October 29, 2012, from <http://www.ibm.com/developerworks/java/library/j-acegi1/index.html> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
