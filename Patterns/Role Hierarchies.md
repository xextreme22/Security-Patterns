# **Role Hierarchies**

## **Intent**
In most of the organizations, users have overlapping responsibilities. Therefore, users acting different roles need to perform some common operations.

## **Example**

## **Context**
ROLE-BASED ACCESS CONTROL environments that require management of user-role assignments and role-privileges assignments.

## **Problem**
Natural role hierarchies can be identified from most of the organizations. The problem is: how can you design and implement such role hierarchies?

The following forces apply:

- Roles must be designed in such a way that roles in lower down in the hierarchy (senior roles) has all privileges granted to its junior roles, but junior roles should not have additional privileges granted for its senior roles. 
- Although senior roles are allowed to access information granted for its junior roles, some of the privileges granted to junior roles may not be granted to a particular senior role(s).

## **Solution**
Define all role objects in the hierarchy with a common interface that conform to the most senior role’s privileges. Use Hook Method pattern to define the interface. For each role, implement these methods such a way that only authorized methods get execute.

![](./Images/role_hierarchies_solution.png)

Role. Defines the interface including all methods (operations) permitted to roles in the hierarchy. The authorized methods are implemented to invoke the corresponding method in AccessMethods. Maintains a reference to AccessMethods.

SeniorRoles (SeniorRoleA, SeniorRoleB, SeniorRoleB1, SeniorRoleB2) Overrides the methods, which are authorized to them, but they are not implemented by any of its junior roles.

If any method authorized to its junior role is not authorized to it, override the method with an empty implementation.

AccessMethods declares the interface for all operations perform within the organization and implements them.

Client always requests Role to execute methods (operations).

Role objects forward the valid requests to AccessMethods and invalid requests are denied.

## **Structure**

## **Dynamics**

## **Implementation**
Since each role must be implemented to reflect its access rights, senior roles need to implement only authorized methods, which are not implemented by any of its junior roles. When senior role is not granted some of the methods granted to its junior roles, they can be overridden with empty implementation.

## **Example Resolved**
- When a senior role is revoked from a user, he does not have rights to execute operations permitted for junior roles. If user need to act a junior role, it should be reassigned. 
- Users can request to execute unauthorized methods, but the implementation of role objects ensures that only authorized operations get execute. 
- Role hierarchies reduce the number of user role assignments. However, it is difficult to handle some restrictions. For example, once a user creates a session with a senior role, all is junior roles are also activated. Therefore, it is hard to restrict users to act only one role at a time.

## **Consequences**

## **Known Uses**
Many object-oriented user interface toolkits use this pattern to add graphical embellishments to widgets.

## **See Also**
- The Hook Method [1] pattern is used for implementation. 
- The Strategy pattern [1] can be used to design different access control algorithms.

## **References**

[1] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Kodituwakku, S. R., Bertok, P., & Zhao, L. (2001). APLRAC: A Pattern Language for Designing and Implementing Role-Based Access Control. In *EuroPLoP* (Vol. 1, p. 2001).