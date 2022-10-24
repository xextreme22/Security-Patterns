# **Actor and Role Lifecycle**

## **Intent**
The ACTOR AND ROLE LIFECYCLE pattern depict the most elementary steps, processes, and actions to be established in order to govern the authorization rights. The pattern is based on the life cycle of an actor.

## **Example**
Suzan is currently working for a high-tech company for two years. So, her employee life cycle for this company did start two years ago. From the start her role is secretary of the CEO. On the first of April, Suzan will move to the administrative department where she will be in charge of the invoicing team.

What does this mean for her actor and role life cycles?

Looking at the current role of Suzan, she is an actor in the electronic agenda information system with the role “Secretary of CEO” which gives her the authority to update the agenda of the CEO. In the new role of Suzan in the invoicing team, Suzan needs to be able to work as an actor in the ERP information system with the role “team leader invoicing”.

If we apply the actor and role life cycle pattern to this example, then the pattern will tell us that two security procedures need to be triggered just before the first of April:

- Deregister role “Secretary of CEO”. E.g., Suzan will still be an actor in the electronic agenda information system because she needs to be able to access her own agenda. 
- Register actor in the ERP system. E.g., before Suzan her role can be entered into the ERP system, she must first be an actor within this system. 
- Register role “team leader invoicing” for Suzan in the ERP - system

## **Context**
This pattern is applicable in almost every environment where access rights have to be maintained. It can be used in designing the processes, but also as a control or audit tool.

## **Problem**
The processes necessary to govern the access rights to information systems are hardly ever explicitly established, although they are quite standard. Based on the actors of a system and their life cycle we can easily depict the necessary processes. By using this pattern, we do not only guarantee the establishment of the granting of the access rights, but also on the withdrawal of these rights.

Apart from the problems in establishing the right access rights to the right people, the right actors (see the other patterns in this document), one of the well-known problems is also the management of the access rights. Therefore, we need to establish some processes.

The following forces apply:

- Employees, or people in general, strive to a maximum set of access rights. Having access to is associated with having power. Power to do something, power to see information. Aiming at a maximum set of access rights is not in line with security where we normally grant access rights based on the “need to know” principle. This hampers controlling access rights.
- When people change roles within an organization, this is usually associated with promotion. In general, promotions are associated with more power, better income, and more rights. It’s not appreciated when promotions result in limiting access rights, especially when people were used to having these access rights in their previous role.
- In order to do their job, people need access to information and computer systems. They will complain if they can’t do their job because of lacking access rights. They will hardly ever complain if they have to many access rights for doing their job. Sometimes they won’t even notice having to many rights, sometimes they think it might be convenient and sometimes they deliberately won’t mention it.
- Access rights are usually granted when a person starts a new job or role. Due to small changes in job roles, the job to be performed or small changes in the information systems, in time the granted access rights may delineate from the necessary rights, but nobody notices.

These forces all plea for having structured processes in place to grant and withdraw access rights.

## **Solution**
Starting from the actor life cycle we can determine the essential business processes related to the management of the access rights. These are not only based on granting access, but also on the withdrawal of the access rights. That’s the advantage of the life-cycle principle.

### **Role, actor, and employee life cycle**
When a human (e.g., employee or student) joins an organization, he (or she) will leave the same organization in the end. This could be called the “employee life-cycle”. Since there is no obligation that every human also has to be an actor on any information system, the granting of access rights is no part of the employee life cycle. Therefore, we will not consider this life cycle in the remainder. When a human becomes an actor on any information system, the “Actor Life-cycle” starts. During this life cycle the actor can perform several roles, for which access rights have to be controlled. Within the actor life cycle there can be several roles to be performed, where each role can have its own access rights to be controlled. Sometimes an actor can change roles almost instantaneously; sometimes a role can take weeks or months.

According to his role, each actor can perform certain business processes. This is the elementary level where the actor is identified and authorized to perform the business processes. Figure 72 and 73 illustrates the above.

![](./Images/actor_and_role_lifecycle_solution_1.png)

*Figure 72: Processes necessary in controlling access rights.*

![](./Images/actor_and_role_lifecycle_solution_2.png)

*Figure 73: Role, Actor and Employee Lifecycle.*

It’s not easy to use an activity diagram since normally the activities follow each other without any delay. Since an employee life cycle or a role life cycle can take years, we are still searching for improvements or other diagramming techniques.

![](./Images/actor_and_role_lifecycle_solution_3.png)

*Figure 74: Life cycles in an Activity Diagramming technique*

### **Authorization Processes during the life cycle**
Starting with the actor life cycle and thinking about processes that have to be in place in order to control the access rights of the actor, we find the following. 

1. Register Actor is the process to register the actors that will have access to a specific system. We do not specify how this registration is done, although automated registration is preferred. At least this process should register the combination of the identity of the employee, the actor he will be and the information system upon which he will act. 
1. De-register Actor is the counterpart to registration. This process builds on the registration from the previous process and should register at least the date at which the employee was no longer associated with the specific actor. Preferably the process should also register the date of deregistration for auditing purposes.

During the role life cycle the following authorization processes have to be in place. 

1. Determine role of actor 
1. Grant access rights 
1. Withdraw access rights 6. Change role

![](./Images/actor_and_role_lifecycle_solution_4.png)

*Figure 75: Authorization processes during life cycle.*

Acting in his role, the actor will perform business processes. In order to control the access to these processes, identification, authentication, and authorization has to be in place. Since these are regular security controls, they are out of scope for this document.

## **Structure**

## **Dynamics**

## **Implementation**
Implementation issues:

- To administrate access rights sometimes package-based solutions are used. Sometimes it is quite difficult to pinpoint the described processes in these package-based solutions. 
- When auditing or reporting tools are used to detect incidents on the use of access rights, be aware that the distinction between employee, actor and role is not always clear or adjustable in these tools. 
- Be aware that the implementation of this pattern might be influenced by the security policy and classification schemes. 
- Make sure that the business owners and information owners check the authorizations on a regular basis. 
- In highly volatile environments this pattern needs to be complemented with other security controls to keep a grip on the dynamics of the environment.

## **Example Resolved**

## **Consequences**
Consequences in implementing this pattern might be the following.

Some benefits: 

- By paying explicit attention to easy to forget processes like the withdrawal of access rights or deregistration of actors, these processes will be implemented and therefore access rights are better controlled. 
- The notion of life cycle implicitly makes clear that access rights are only granted for a certain period. 
- The distinction between actor and role will give insight in the specific access rights needed for a certain role. 

Some pitfalls: 

- Although this pattern makes clear that these processes have to be in place, it is not possible to determine where and when these processes have to be in place. It can still be a tough job finding the right moment for withdrawal of access rights. 
- In environments where actors seldom change role or where employees seldom change to another actor, implementing this pattern can lead to rigorous administration that will be outdated very soon. 
- Cultural aspects like lack of discipline are known pitfalls to implement the procedures related to this pattern. Solutions: Automate as much as possible and produce special security reports to check the authorizations on a regular basis.

## **Known Uses**
This pattern was used by the authors in a previous project where security administration was developed and implemented in an Internet banking environment. Another use of this pattern is for an extranet security architecture for a high-tech company.

## **See Also**
In general, all patterns regarding role-based access are related, especially: 

- The ACTOR-BASED ACCESS RIGHTS pattern. 
- The processes described in this pattern are applicable to the User-Role-Privilege relationship in Role [1]. 
- The activities related to managing the rights in Role Based Access Control are part of the processes described in this pattern. 
- The activities related to managing the rights in ADMINISTRATOR OBJECT pattern are part of the processes described in this pattern.
- The SECURITY SESSION pattern.
 
## **References**

[1] Yoder, J., & Barcalow, J. (1997, September). Architectural patterns for enabling application security. In *Proceedings of the 4th Conference on Patterns Language of Programming (PLoP’97)* (Vol. 2, p. 30).

## **Source**
Hofman, A., & Elsinga, B. (2002). Control the Actor-Based Access Rights. In *EuroPLoP* (pp. 233-244).
