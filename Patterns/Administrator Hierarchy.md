# **Administrator Hierarchy**

## **Intent**
Many attacks come from the unlimited power of administrators. The ADMINISTRATOR HIERARCHY pattern allows the power of administrators to be limited, by defining a hierarchy of system administrators with rights controlled using a ROLE-BASED ACCESS CONTROL (RBAC) model and assigns them rights according to their functions.

## **Example**
UNIX defines a superuser who has all possible rights. This is expedient: for example, when somebody forgets a password, but allows hackers to totally control the system through a variety of implementation flaws. Through gaining access to the administrator rights, an individual can create new administrator and user accounts, restrict their privileges and quotas, access their protected areas, or remove their accounts.

## **Context**
An operating system with a variety of users, connected to the Internet. There are some commands and data that are used for system administration, and access to them needs to be protected. This control is usually applied through special interfaces. There are at least two roles required to properly manage privileges, Administrator and User.

## **Problem**
Usually, the administrator has rights such as creating accounts and passwords, installing programs and so on. This creates a series of security problems. For example, a rogue administrator can perform all the usual functions, and even erase the log to hide their tracks. A hacker that takes over administrative power can do similar things. How can we curtail the excessive power of administrators to control rogue administrators or hackers?

The solution to this problem must resolve the following forces: 

- Administrators need to use commands that permit management of the system, for example define passwords for files, define quotas for files, and create user accounts. We cannot eliminate these functions. 
- Administrators need to be able to delegate some responsibilities and privileges to manage large domains. They also need the right to take back these delegations, otherwise the system is too rigid. 
- Administrators should have no control of system logs, or no valid auditing would be possible. 
- Administrators should have no access to the operational data in the users’ applications. Or, if they do, their accesses should be logged.

## **Solution**
Separate the different administrative rights into several hierarchical roles. The rights for these roles allow the administrators to perform their administrative functions and no more. Critical functions may require more than one administrative role to participate. Use the principle of separation of duty, where a user cannot perform critical functions unless in conjunction with other users.

## **Structure**
Figure 152 shows a hierarchy for administration roles. This follows the Composite pattern [1]; that is, a role can be simple or composed of other roles, defining a tree hierarchy. The top-level Administrator can add or remove administrators of any type and initialize the system but should have no other functions. Administrators in the second level control different aspects, for example security, or use of resources. Administrators can further delegate their functions to lower-level administrators. Some functions may require two administrators to collaborate.

![](./Images/administrator_hierarchy_structure.png)

*Figure 152: Class diagram for the ADMINISTRATOR HIERARCHY pattern*

## **Dynamics**

## **Implementation**
1. Define a top-level administrative role with only the functions of setting up and initializing the system. This includes definition of administrative roles, assignment of rights to roles and assignment of users to roles. 
1. Separate the main administrative functions of the system and define an administrative role for each one of them. These define the second level of the hierarchy. 
1. Define further levels to accommodate administrative units in large systems, or for breaking down rights into functional sets.

Figure 153 shows a class diagram describing a typical administrator hierarchy. Here the SystemAdministrator starts the system and does not perform further actions. The second-level administrators can perform set up and other functions; the SecurityAdministrator defines security rights. SecurityDomainAdministrators define security in their domains.

![](./Images/administrator_hierarchy_implementation.png)

*Figure 153: A typical administration hierarchy*

## **Example Resolved**
Now the superuser only starts the system. During normal operation the administrators have restricted powers. If a hacker takes over their functions, they can do only limited damage.

## **Consequences**
The ADMINISTRATOR HIERARCHY pattern offers the following benefits: 

- If an administrative role is compromised, the attacker gets only limited privileges, so the potential damage is limited. 
- The reduced rights also reduce the possibility of misuse by the administrators. 
- The hierarchical structure allows taking back control of a compromised administrative function. 
- The advantages of the RBAC model apply simpler and fewer authorization rules, flexibility for changes, and so on. 
- This structure is useful not only for operating systems, but also for servers, databases systems or any systems that require administration. 

The pattern also has the following potential liabilities: 

- Extra complexity for the administrative structure. 
- Less expediency: performing some functions may involve more than one administrator. 
- Many attacks are still possible: if someone misuses an administrative right, this pattern only limits the damage. Logging can help misuse detection.
 
## **Known Uses**
- AIX [2] reduces the privileges of the system administrator by defining five partially ordered roles: superuser, security administrator, auditor, resource administrator and operator. 
- Windows NT uses four roles for administrative privileges: standard, administrator, guest, and operator. A user manager has procedures for managing user accounts, groups, and authorization rules. 
- Trusted Solaris [3]. This operating system is an extension of Solaris 8, using the concept of trusted roles with limited powers. 
- Argus Pitbull [4]. In this operating system, least privilege is applied to all processes, including the superuser. The superuser is implemented using three roles: systems security officer, system administrator and system operator.

## **See Also**
- This pattern applies the principles of least privilege and separation of duty, which some people consider also to be patterns. Each administrator role is given only the rights it needs to perform its duties and some functions may require collaboration. 
- Administrative rights are usually organized according to a ROLE-BASED ACCESS CONTROL model.
- This pattern is the application of the ROLE HIERARCHIES pattern on the ADMINISTRATOR OBJECT pattern.

## **References**

[1] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[2] Camillone, N. A., Steves, D. H., & Witte, K. C. (1990). AIX operating system: A trustworthy computing system. *IBM RISC System/6000 Technology*, 168-172.

[3] (n.d.). Trusted Solaris Operating System. <http://www.sun.com/software/solaris/trustedsolaris/>

[4] Argus Systems Group. (n.d.). Trusted OS security: Principles and Practice. Retrieved October 27, 2012, from <http://www.argus-systems.com/Products/products.html> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
