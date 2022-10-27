# **Role-Based Access Control**

## **Intent**
The ROLE-BASED ACCESS CONTROL pattern describes how to assign rights based on the functions or tasks of users in an environment in which control of access to computing resources is required.

## **Example**
The hospital has many patients, doctors, nurses, and other personnel. The specific individuals also change frequently. Defining individual access rights has become a time-consuming activity, prone to errors.

## **Context**
Any environment in which we need to control access to computing resources and in which there is a large number of users and information types, or a large variety of resources.

## **Problem**
For convenient administration of authorization rights, we need to have a means of factoring out rights. Otherwise, the number of individual rights is just too large; granting rights to individual users would require storing many authorization rules, and it would be hard for administrators to keep track of these rules. It is also hard to associate semantic meanings to the rules. How can we reduce the number of rules and make their semantics clearer? 

The solution to this problem must resolve the following forces: 

- Complexity. We would like to make the work of the security administrator as simple as possible. 
- Semantics. In most organizations people are assigned specific functions or tasks. Their rights should correspond to those tasks. 
- Policy. We need to define rights according to organizational policies. 
- Commonality. People performing the same tasks should have the same rights. 
- Policy enforcement. We want to help the organization to define precise access rights for its members according to a need-to-know policy. 
- Flexibility. People joining, leaving, and changing functions should not require complex rights manipulation.

## **Solution**
Most organizations have a variety of job functions that require different skills and responsibilities. Users should be assigned rights based on their job functions or their designated tasks. This corresponds to the application of the need-to-know principle, a fundamental security policy [1]. Job functions can be interpreted as roles that people play in performing their duties. In particular, web-based systems have a variety of users: company employees, customers, partners, search engines and so on.

## **Structure**
Figure 144 shows a class diagram for ROLE-BASED ACCESS CONTROL. The User and Role classes describe registered users and their predefined roles respectively. Users are assigned to roles; roles are given rights according to their functions. The association class Right defines the access types that a user within a role is authorized to apply to the protection object. The combination Role, ProtectionObject and Right is an instance of the AUTHORIZATION pattern.

![](./Images/role-based_access_control_structure.png)

*Figure 144: Class diagram for the ROLE-BASED ACCESS CONTROL pattern*

## **Dynamics**
Use cases include ‘Add a rule’, ‘Delete a rule’, ‘Modify a rule’, ‘Assign user to role’, ‘Assign rights to role’ and so on. We do not show their sequence diagrams here because they are very simple.

## **Implementation**
Roles may correspond to job titles, for example ‘manager’, ‘secretary’. A finer-grained approach is to make them correspond to tasks. For example, a professor has the roles of ‘thesis advisor’, ‘teacher’, ‘committee member’, ‘researcher’ and so on. An approach to defining role rights is described in [2]. 

There are many possible ways to implement roles in a software system. [3] considers the implementation of the data structures needed to apply an RBAC model. Concrete implementations can be found in operating systems, database systems and web application servers.

## **Example Resolved**
The hospital now assigns rights to the roles of doctors, nurses and so on. The number of authorization rules has decreased dramatically as a result.

## **Consequences**
The ROLE-BASED ACCESS CONTROL pattern offers the following benefits: 

- Semantics. We can make the rights given to a role correspond to tasks. 
- Complexity. It allows administrators to reduce the complexity of security. Because there are many more users than roles, the number of roles becomes much smaller. 
- Policy. Organization policies about job functions can be reflected directly in the definition of roles and the assignment of users to roles. 
- Commonality. People performing the same tasks can be given the same rights. 
- Flexibility. It is very simple to accommodate users joining, leaving, or being reassigned. All these use cases require only manipulation of the associations between users and roles. 
- Structure. Roles can be structured into hierarchies for further flexibility and reduction of rules. 
- Role separation. Users can activate more than one session at a time for functional flexibility: some tasks may require multiple views or different types of actions. Role separation is also important to avoid conflicts of interest: we can add UML constraints to indicate that some roles cannot be used in the same session or given to the same user (separation of duties). 

The following potential liability may arise from applying this pattern: 

- Some institutions may not have clearly defined roles in their organization, and some work must therefore be done to define these roles.

## **Variants**
The model shown in Figure 145 additionally considers composite roles and objects: it is an application of the Composite pattern [4]. The figure also includes the concept of a session, which defines a context for the use of a role, may restrict the number of roles used together at execution time, and can be used to enforce role exclusion at execution time.

![](./Images/role-based_access_control_variants.png)

*Figure 145: Class diagram for the Extended RBAC model*

## **Known Uses**
The RBAC pattern represents in object-oriented form a model described in terms of sets in [5]. That model has been the basis of most research papers and implementations of this concept. RBAC is implemented in a variety of commercial systems, including Sun’s J2EE, Microsoft’s Windows 2000 and later, Microsoft’s .NET [6], IBM’s WebSphere, and Oracle, among others. The basic security facilities of Java’s JDK 1.2 have been shown to be able to support a rich variety of RBAC policies. The NIST has developed a standard for RBAC [7]. We have used RBAC to describe access to physical structures [8].

## **See Also**
The pattern whose class diagram is shown in Figure 145 includes AUTHORIZATION and Composite. A session object is used to provide execution context (see Variants).

## **References**

[1] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc..

[2] Fullerton, M., & Fernandez, E. B. (2007, May). An analysis pattern for customer relationship management (CRM). In *Proc. of the 6th Latin American Conference on Pattern Languages of Programming* (pp. 80-90).

[3] Kodituwakku, S. R., Bertok, P., & Zhao, L. (2001). APLRAC: A Pattern Language for Designing and Implementing Role-Based Access Control. In *EuroPLoP* (Vol. 1, p. 2001).

[4] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[5] Sandhu, R. S., Coyne, E. J., Feinstein, H. L., & Youman, C. E. (1996). Role-based access control models. *Computer*, *29*(2), 38-47.

[6] Fenster, L. (2006). *Effective use of Microsoft Enterprise Library: building blocks for creating enterprise applications and services*. Pearson Education.

[7] Ferraiolo, D. F., Sandhu, R., Gavrila, S., Kuhn, D. R., & Chandramouli, R. (2001). Proposed NIST standard for role-based access control. *ACM Transactions on Information and System Security (TISSEC)*, *4*(3), 224-274.

[8] Fernandez, E. B., Ballesteros, J., Desouza-Doucet, A. C., & Larrondo-Petrie, M. M. (2007, July). Security patterns for physical access control systems. In *IFIP Annual Conference on Data and Applications Security and Privacy* (pp. 259-274). Springer, Berlin, Heidelberg.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
