# **Administrator Object**

## **Intent**
In ROLE-BASED ACCESS CONTROL environment, user-role assignment and role-privileges assignment are the major administrative tasks. Each user has a unique subject that describes the user’s permitted roles and user must activate roles associated with his subject to access information. This pattern creates subjects for users and delegates administrative responsibilities.

## **Example**

## **Context**
ROLE-BASED ACCESS CONTROL environments that require management of user-role assignments and role-privileges assignments.

## **Problem**
The question is: how can you manage user-role assignments and role-privileges assignments?

The following forces apply:

- A single user may allow playing multiple roles, but he may be restricted to play only one role at any given time. 
- Roles may have overlapping access rights due to the overlapping responsibilities of job titles. In such a situation, one role (senior role) may have privileges to access all pieces of information authorized for one or more other roles (junior roles) but inverse is not true. A user playing a senior role must be allowed to play all associated junior roles. However, when the user is playing a particular junior role, he may or may not allowed playing senior role at the same time. 
- In general, senior roles have privileges to access all piece of information granted for its junior roles. However, sometimes some of the junior role’s privileges can be private to that role. For example, in software development domain, senior roles may not be allowed to see programmer’s incomplete codes. 
- A group of users can play the same role, but some roles can only be taken by a limited number of users at any given time. For instant, there should be only one manager in a branch office of a bank. 
- One or more administrators can centrally handle users and roles, but this is infeasible or inefficient for more complex applications such as distributed applications.

## **Solution**
Create a distinct set of roles (administrator roles) with different privileges to manage users and roles. Create a separate role (Central administrator) to manage those administrator roles. Each administrator is given options to handle user-role assignment based on their administrative privileges.

![](./Images/administrator_object_solution.png)

CentralAdministrator defines an interface to create, delete and modify ConcreteAdministrators. 

AbstractAdministrator defines an interface for user-role assignment. 

ConcreateAdministrators extend and implement the AbstractAdministrator interface based on authorized administrative privileges. 

AdminCommand declares an interface for performing administrative actions. 

AssignRole, RevokeRole and ModifyRole define bindings between Subject and administrative actions. Implements execute() to invoke the corresponding operation(s) on Subject. 

Subject is a list of roles (role names and their status) that a particular user is permitted. Administrators create subjects for users by invoking Subject’s interface.

ConcreteAdministrator issues a request to command objects to add, delete or modify roles. In turn, the ConcreteCommand (AssignRole, RevokeRole and ModifyRole) objects invoke the Subject to alter its content. 

The CentralAdministrator handles the person-role assignment and role-privilege assignment for administrator roles

## **Structure**

## **Dynamics**

## **Implementation**
- Since a user may have multiple roles, the Subject class may be implemented to hold a list of roles and their status. Administrator can be implemented to add, revoke, and modify these roles. 
- ConcreteAdministrator can be implemented with permitted operations. The methods assignRole(), revokeRole() and modifyRole() can be implemented to send requests to Admincommand object to assign, revoke or modify roles. 
- Operation() method can be implemented to provide options to perform only permitted operations and to revoke method according to the selection. 
- The Abstract Factory pattern might be used to create different types of ADMINISTRATOR OBJECT.

## **Example Resolved**
- Administrator roles are designed with only operations permitted to them. Consequently, administrators are restricted to perform only permitted operations. 
- The CentralAdministrator can assign administrator roles and privileges by creating ConcreteAdministrator roles. 
- Role assignment and removal are the major operations performed by ConcreteAdministrators. In complex applications, administrators may be restricted to assign roles but not to remove or assign and remove roles to and from a particular set of users. Since each ConcreteAdministrator is given options to perform only permitted operations, this pattern prevents misuse of administrative privileges. 
- Subject of a particular user can be chosen from a list of Subjects, and then would see a list of assigned roles, along with the information about those roles. Roles can easily be selected for deletion and modification.

## **Consequences**

## **Known Uses**
In OASIS, the administrators are responsible for certain security domains, they are allowed to access and manage the security information of these particular domains. The meta-administrators are responsible for specifying domains, defining security concepts. The meta-administrators and administrators are variants of the CentralAdministrator and ConcreteAdministrators of this pattern.

## **See Also**
- Administrator Manager components of OASIS [1] is a refinement of this pattern. 
- This pattern uses the Command pattern [2] to design administrative actions as objects.
- See ACTOR AND ROLE LIFECYCLE pattern.
- See CAPABILITY pattern.

## **References**

[1] Fernandez, E. B., Larrondo-Petrie, M. M., & Gudes, E. (1994). A method-based authorization model for object-oriented databases. In *Security for Object-Oriented Systems* (pp. 135-150). Springer, London.

[2] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Kodituwakku, S. R., Bertok, P., & Zhao, L. (2001). APLRAC: A Pattern Language for Designing and Implementing Role-Based Access Control. In *EuroPLoP* (Vol. 1, p. 2001).
