# **Mandatory Access Control**

## **Intent**
The MAC pattern governs access based on the security level of subjects (e.g., users) and objects (e.g., data). Access to an object is granted only if the security levels of the subject and the object satisfy certain constraints. The MAC pattern is also known as MULTILEVEL SECURITY model and lattice-based access control.

## **Example**
The MAC pattern addresses the following two problems in the DAC pattern. First, there is nothing that prevents a user who is granted read access to a file by the owner of the file from copying the content of the file and granting read access to other users. For example, consider Figure 48. Let’s suppose John wants to access File1 which he does not have permission to access. Of course, a DAC system will not allow him to access the file since he has no permission. However, if there is a third person who has read access to File1 granted by (Jane) who is the owner of File1, and the person grants John read access to File1, then John can read File1 without Jane being aware of it. Secondly, a user who has write access to a file may write a Trojan horse program into the file to copy the contents of the file. A Trojan horse program disguises as if it is a utility program while copying file contents. In Figure 48, the Trojan horse program copies the contents of File1 into File2 which John can access.

![](./Images/mandatory_access_control_example.png)

*Figure 48:  Trojan Horse Problem in the DAC Pattern*

## **Context**
Development of access control systems that handle classified objects and need to limit users’ actions according to a hierarchy of classifications.

## **Problem**
The MAC pattern can solve the above problems with the DAC pattern in a multi-layered environment (e.g., military and government systems) by assigning security levels to users and objects. In this sense, the solution of the DAC pattern can be considered as a problem of the MAC pattern. That is, if a DAC system is deployed in a multi-layered environment, the MAC pattern can be applied to improve the confidentiality of the system. Use the MAC pattern:

- Where the environment is multi-layered. For example, in the military domain, users and files are classified into distinct levels of hierarchy (e.g., Unclassified, Public, Secret, Top Secret), and user access to files is restricted based on the classification. 
- When security policies need to be defined centrally. Access control decisions are to be imposed by a mediator (e.g., security administrator), and users should not be able to manipulate them.

## **Solution**
The MAC pattern solves the above issues in a classified environment by assigning security levels to users and objects. In the MAC pattern, a user can read a file, but cannot write to the file if the user’s security level is higher than or equal to the file’s security level. A user can write to a file but cannot read the file if the user’s security level is lower than or equals to the file’s security level. For example, consider the military domain where documents are classified and categorized as shown in Figure 49.

![](./Images/mandatory_access_control_solution_1.png)

*Figure 49: A MAX Example*

In the diagram, John has read and write access to File1 since his classification and category are same as that of File1. However, Jane can only read File1, but cannot write to the file because the classification of File1 is lower than that of Jane. Smith cannot read and write to File1 because his category is different from the category of File1. Bill can write to File1 but cannot read the file because the classification of the file dominates his classification.

![](./Images/mandatory_access_control_solution_2.png)

*Figure 50: Trojan Horse Solution in the MAC Pattern*

Using the MAC pattern, the Trojan horse problem in Figure 48 can be resolved as shown in Figure 50. In Figure 50, the Trojan horse program running on behalf of Jane can read File1 since the security level of Jane is equal to that of File1. However, it cannot write to File2 because Jane’s security level is lower than the security level of Fil2. Thus, John is not able to access File1.

## **Structure**
Figure 51 shows the solution structure of the MAC pattern [1].

- User represents a user or a group of users who interacts with the system. A user is assigned a hierarchical security level (e.g., SECRET, CONFIDENTIAL) and non-hierarchical category (e.g., U.S., Allies) to which the user belongs. A user may have multiple login IDs which can be activated simultaneously. A user also may create and delete one or more subjects. 
- Subject represents a computer process that acts on behalf of a user to request an operation on an object. For instance, an ATM machine being used by a user can be viewed as a subject. A subject may be given the same security level as the user or any level below the user’s security level. 
- Object represents any information resource (e.g., files, databases) in the system that can be accessed by the user. Similar to users, an object is assigned a hierarchical security level and a non-hierarchical category to which the object belongs. 
- Operation is an action being performed on an object invoked by a subject. 
- SecurityLevel represents a sensitivity assigned to users (subjects) and objects. A security level consists of a classification and a category. While classifications are hierarchical, categories are non-hierarchical. 
- ReferenceMonitor checks accessibility based on the following constraints. 
  - Simple security property - A subject S is allowed read access to an object O only if L(S) ≥ L(O). 
  - Star property - A subject S is allowed write access to an object O only if L(S) ≤ L(O). 

Access is allowed when both the constraints are satisfied. Access is checked only if the user is in the same category as that of the object. With the categories matched, the accessibility of the user for the object is determined by the dominance relations of classifications in the above constraints.

![](./Images/mandatory_access_control_structure.png)

*Figure 51: MAC Solution Structure*

## **Dynamics**
Figure 52 shows the collaboration for requesting an operation. The diagram describes that a subject requests an operation on an object, and the request is intercepted by the REFERENCE MONITOR to check authorization by enforcing the simple security property and star property. The request is performed on the object if it is authorized, otherwise it is denied.

![](./Images/mandatory_access_control_dynamics.png)

*Figure 52: MAC Collaboration*

## **Variants**
The Biba Integrity model [2] can be viewed as a variant of the MAC pattern, emphasizing on integrity rather than confidentiality. Similar to the MAC pattern, Biba model has Simple Integrity property and Integrity Star property which are defined as follows:

- Simple integrity property - A subject S is allowed read access to an object O only if L(O) ≥ L(S). 
- Integrity star property - A subject S is allowed write access to an object O only if L(O) ≤ L(S).
  1. # **Implementation**

## **Example Resolved**

## **Consequences**
The MAC pattern has the following advantages: 

- MAC systems are secure to Trojan horse attacks. 
- The assignment of a classification and category to users and objects is centralized by a mediator. 
- The MAC pattern facilitates enforcing access control policies based on security levels. 

The MAC pattern has the following disadvantages: 

- Introducing a new object or user requires a careful assignment of a classification and category. 
- The mediator who assigns classifications to users and objects should be a trusted person.

## **Known Uses**
Security-Enhanced Linux (SELinux) kernel [3] developed by a collaboration of NSA, MITRE Corporation, NAI labs and Secure Computing Corporation (SCC) enforces the MAC pattern to implement a flexible and fine-grained MAC architecture called Flask which operates independently of the traditional Linux access control mechanisms. TrustedBSD [4] developed by the FreeBSD Foundation provides a set of trusted operating system extensions to the FreeBSD operating system which is an advanced operating system for x86, amd64 and IA-64 compatible architectures. TrustedBSD contains modules that implement MLS (Multi-Level Security) and fixed-label Biba integrity policies which is a variant of the MAC pattern. GeSWall (General Systems Wall) [5] is the Windows security project developed by GentleSecurity. GeSWall implements the MAC pattern to provide OS integrity and data confidentiality transparent and invisible to user.

## **See Also**
The Biba’s Integrity model [2] which addresses integrity issues where access is determined by the integrity levels of subjects and objects, rather than confidentiality. The Chinese Wall model [6] which is similar to the MAC pattern in that it also defines read and write constraints where the write constraint considers the Trojan horse problem. However, unlike the MAC pattern, the Chinese Wall model does not distinguish between users and subjects. Subjects include both users and processes acting on behalf of the user.

## **References**

[1] Kim, D. K., & Gokhale, P. (2006, September). A pattern-based technique for developing UML models of access control systems. In *30th Annual International Computer Software and Applications Conference (COMPSAC'06)* (Vol. 1, pp. 317-324). IEEE.

[2] Biba, K. J. (1977). *Integrity considerations for secure computer systems*. MITRE CORP BEDFORD MA.

[3] SELinux. (n.d.). <http://www.nsa.gov/selinux/>. 

[4]. TrustedBSD. (n.d.). TrustedBSD Project. http://www.trustedbsd.org/. 

[5] General System Wall. (n.d.). http://www.securesize.com/GeSWall/. 

[6] Brewer, D. F., & Nash, M. J. (1989, May). The Chinese Wall Security Policy. In *IEEE symposium on security and privacy* (Vol. 1989, p. 206).

## **Source**
Kim, D. K., Mehta, P., & Gokhale, P. (2006, October). Describing access control models as design patterns using roles. In *Proceedings of the 2006 conference on Pattern languages of programs* (pp. 1-10).
