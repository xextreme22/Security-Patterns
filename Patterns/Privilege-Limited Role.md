# **Privilege-Limited Role**

## **Intent**
After the administrator has assigned a subject to users, a user creates a Session with one of the roles in his subject to execute operations. This pattern achieves role-privileges assignment by restricting method execution.

## **Example**

## **Context**
Any environment in which we need to control access to computing resources.

## **Problem**
The system must assess user requests on the basis of privileges belonging to the users’ role. The question is: how can you implement the role object to reflect the role-privileges assignment?

The following forces apply:

- Once a user is assigned a particular role, he can access information by creation a session. However, roles must be assigned privileges to ensure that roles are granted least privileges for information access. 
- Role validation can be easier when roles are implemented with only permitted operations. However, users may be confused when some operations are either not present or disable.

Since administrator pattern creates only subjects for users, roles assigned to the subject should be granted access rights to ensure that roles are not allowed to execute operations they do not have privileges for.

## **Solution**
Only let the user to access what they are permitted. Define a role class for each role introduced by the Role Based Access Control pattern. The class interface should reflect the privileges of the role.

![](./Images/privilege-limited_role_solution.png)

Role defines the interface with only permitted operation for the role and maintains a reference to AccessMethod. 

AccessMethods declares the interface for all operations perform within the organization and implements them.

Client request Role object to execute operations. In turn, the Role object forwards it to the corresponding method in AccessMethods object. In turn, AccessMethods execute the requested operation.

## **Structure**

## **Dynamics**

## **Implementation**

## **Example Resolved**
- The role classes are defined with only operations permitted to them. Consequently, users are strictly limited to access only information to perform their job functions. As a result, roles need not implement the operations they do not permit. 
- Actual operations are implemented within the AccessMethods class, and it is exclusively included in Role objects. Therefore, user access to the information is completely controlled by roles. 
- In general, organizations perform a large number of operations, and it is infeasible to define all of them as a single class (AccessMethods). In such a situation, AccessMethods can be decomposed into a number of small objects.

## **Consequences**

## **Known Uses**
OMG-CORBA uses the Proxy variant of the Role Privilege Assignment pattern for two purposes. The client-stub guard clients against the concrete implementation of their servers and the ORB. ORB uses the IDL-skeleton to forward requests to concrete remote server components.

## **See Also**
- Limited-View [1] designs application so user can request to perform only legal operations. 
- Proxy pattern [2] provides an indirection to access information. In particular, Protection Proxy pattern protects information from unauthorized access.

## **References** 

[1] Harrison, N., Foote, B., & Rohnert, H. (Eds.). (2000). *Pattern languages of program design 4* (pp. 15-32). Reading, MA: Addison-Wesley.

[2] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Kodituwakku, S. R., Bertok, P., & Zhao, L. (2001). APLRAC: A Pattern Language for Designing and Implementing Role-Based Access Control. In *EuroPLoP* (Vol. 1, p. 2001).
