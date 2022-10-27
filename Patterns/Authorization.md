# **Authorization**

## **Intent**
This pattern describes who is authorized to access specific resources in a system, in an environment in which we have resources whose access needs to be controlled. It indicates, for each active entity that can access resources, which resources it can access, and how it can access them.

## **Example**
In a medical information system, we keep sensitive information about patients. Unrestricted disclosure of this data would violate the privacy of the patients, while unrestricted modification could jeopardize the health of the patients.

## **Context**
Any environment in which we have resources whose access needs to be controlled.

## **Problem**
We need to have a way to control access to resources, including information. The first step is to declare who is authorized to access resources in specific ways. Otherwise, any active entity (user, process) could access any resource and we could have confidentiality and integrity problems. 

How do we describe who is authorized to access specific resources in a system? 

The solution to this problem must balance the following forces: 

- The authorization structure must be independent of the type of resources. For example, it should describe access by users to conceptual entities, access by programs to operating system resources, and so on, in a uniform way. 
- The authorization structure should be flexible enough to accommodate different types of subjects, objects, and rights. 
- It should be easy to modify the rights of a subject in response to changes in their duties or responsibilities.

## **Solution**
Indicate, for each active entity that can access resources, which resources it can access and how.

## **Structure**
The Subject() class describes an active entity that attempts to access a resource (Protection Object) in some way. The ProtectionObject() class represents the resource to be protected. The association between the subject and the object defines an authorization, from which the pattern gets its name. The association class Right() describes the access type (for example, read, write) the subject is allowed to perform on the corresponding object. Through this class one can check the rights that a subject has on some object, or who is allowed to access a given object. 

Figure 185 shows the elements of an authorization in form of a class diagram.

![](./Images/authorization_structure.png)

*Figure 185: Class model for AUTHORIZATION*

## **Dynamics**

## **Implementation**
An organization, according to its policies, should define all the required accesses to resources. The most common policy is need-to-know, in which active entities receive access rights according to their needs. 

This pattern is abstract and there are many implementations: the two most common approaches are ACCESS CONTROL LISTs and Capabilities [1]. ACCESS CONTROL LISTs (ACLs) are kept with the objects to indicate who is authorized to access them, while Capabilities are assigned to processes to define their execution rights. Access types should be application oriented.

## **Example Resolved**
A hospital using an authorization system can define rules that allow only doctors or nurses to modify patient records, and only medical personnel to read patient records. This approach allows only qualified personnel to read and modify records.

## **Consequences**
The following benefits may be expected from applying this patter: 

- The pattern applies to any type of resource. Subjects can be executing processes, users, roles, user groups. Protection objects can be transactions, data, memory areas, I/O devices, files, or other resources. Access types are individually definable and can be application-specific in addition to the usual read and write. 
- It is convenient to add or remove authorizations. 
- Some systems separate administrative authorizations from user authorizations for further security, on the principle of separation of duties [2]. 
- The request may not need to specify the exact object in the rule: the object may be implied by an existing protected object [3]. Subjects and access types may also be implied. This improves flexibility at the cost of some extra processing time to deduce the specific rule needed. 

The following potential liabilities may arise from applying this pattern: 

- If there are many users or many objects, a large number of rules must be written. 
- It may be hard for the security administrator to realize why a given subject needs a right, or the implications of a new rule. 
- Defining authorization rules is not enough, we also need an enforcement mechanism.

## **Variants**
The full access matrix model usually described in textbooks also includes: 

- Predicates or guards, which may restrict the use of the authorization according to specific conditions
- Delegation of some of the authorizations by their holders to other subjects through the use of a Boolean ‘copy’ flag

Figure 186 extends AUTHORIZATION to include those aspects. Right now, includes not only the type of access allowed, but also a predicate that must be true for the authorization to hold, and a copy flag that can be true or false, indicating whether or not the right can be transferred. CheckRights is an operation to determine the rights of a subject or to find who has the rights to access a given object.

![](./Images/authorization_variants.png)

*Figure 186: Extended AUTHORIZATION*

## **Known Uses**
This pattern defines the most basic type of authorization rule, on which most more complex access-control models are based. It is based on the concept of access matrix, a fundamental security model [1,4]. Its first object-oriented form appeared in [5]. Subsequently, it has appeared in several other papers and products [6,7]. It is the basis for the access control systems of most commercial products, such as Unix, Windows, Oracle, and many others. PACKET FILTER FIREWALL implements a variety of this pattern in which the subjects and objects are defined by Internet addresses.

## **See Also**
ROLE-BASED ACCESS CONTROL is a specialization of this pattern. REFERENCE MONITOR complements this pattern by defining how to enforce the defined rights.

## **References**

[1] Pfleeger, C. P. (2003). Security in Computing (Third Edition). Prentice-Hall.

[2] Wood, C., & Fernandez, E. B. (1979). *Authorization in a decentralized database system*. IBM.

[3] Fernández, E. B., Summers, R. C., & Lang, T. (1975, September). Definition and evaluation of access rules in data management systems. In *Proceedings of the 1st International Conference on Very Large Data Bases* (pp. 268-285).

[4] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc..

[5] Fernandez, E. B., Larrondo-Petrie, M. M., & Gudes, E. (1994). A method-based authorization model for object-oriented databases. In *Security for Object-Oriented Systems* (pp. 135-150). Springer, London.

[6] Essmayr, W., Pernul, G., & Tjoa, A. M. (1998). Access controls by object-oriented concepts. In *Database Security XI* (pp. 325-340). Springer, Boston, MA.

[7] Kodituwakku, S. R., Bertok, P., & Zhao, L. (2000). A pattern language for designing and implementing role-based access control. *Procs. KoalaPLoP 2001*.

## **Source**
Schumacher, M., Fernandez-Buglioni, E., Hybertson, D., Buschmann, F., & Sommerlad, P. (2006). Security Patterns: Integrating Security and Systems Engineering (1st ed.). Wiley.
