# **Discretionary Access Control**

## **Intent**
The DISCRETIONARY ACCESS CONTROL (DAC) pattern enforces access control based on user identities and the ownership of objects. The owner of an object may grant permission to another user to access the object, and the granted user may further delegate the permission to a third person.

## **Example**
Consider an environment where access control is solely managed by the security administrator. A problem in such an environment is that it requires much effort for the administrator to maintain access control for every single user and to deal with daily-basis requests of permission changes. Another problem related to confidentiality is that the administrator may give unreasoned permission to a person who is not supposed to be authorized. For example, in a medical care system, access to patient information should be kept confidential and limited to only the doctor who handles the case, or other doctors who have permission given by the handling doctor. If the security administrator receives a fallacious request for permission given to a clerk, obviously it should not be allowed. In regard to availability, such an environment may cause a situation where no one can access information. For example, in the medical care system above, if DoctorA who handles CaseFile1 has left for a vacation, without making a permission request for other doctors to access the file, no one can access the file in case of emergency. Figure 44 illustrates these problems.

![](./Images/discretionary_access_control_example.png)

*Figure 44: A Motivation Example*

## **Context**
Development of access control systems that allow user-controlled administration of access rights to objects.

## **Problem**
In an environment where access control is managed solely by administrators, unreasoned permission may be given to a person who should not be authorized, or no access may be allowed for anyone. In such cases, it may be desired to leave access control decisions to the discretion of the owner of the respective resources. Use the DAC pattern:

- Where users own objects.
- When permission delegation is necessary. 
- When a resolution for conflicting privileges is needed. For instance, a user may be allowed to access an object as a member of a group, but not allowed with individual permissions. 
- When a security mechanism is needed in a heterogeneous environment for controlling access to different kinds of resources. 
- Where multi-user relational database is used.

## **Solution**
The DAC pattern can address the above problems by using the concept of “permission delegation” which allows a user of an object to give away permission to other users to access the object at her/his discretion without the intervention of the administrator. Using the DAC pattern, the burden on the administrator is shared with the users of objects who are capable of delegating permission. The DAC pattern mitigates the confidentiality problem above by granting permission directly to related people in the area. Also, the availability problem above can be addressed by delegating permission at the discretion of object users.

Figure 45 shows an ACCESS CONTROL LIST (ACL) that implements the DAC pattern. In the ACL, a delegating user is represented in user::rw and a named user to which permission is delegated is represented in user:namedUser::rw. For example, DoctorA is a user of CaseFile1, and has read and write access to the file, and DoctorB is a named user who is granted access to CaseFile1 by DoctorA and has read and write access to the file.

![](./Images/discretionary_access_control_solution.png)

*Figure 45: DAC Solution*

An inconsistency between the permission given as an individual and the permission given as a member of a group to which the user belongs can be resolved by evaluating permission in an order. The evaluation stops either when all requested access rights have been granted by one or more permission entries, or when any one of the requested access rights has been denied by one of the permission entries.

## **Structure**
Figure 46 shows the solution structure of the DAC pattern. 

- User represents a user or group who has access to an object, or a named user or group who are granted access to the object by the user or group. The owner or owning group of an object has full access to the object and can grant or revoke permission to other users or groups at their discretion. 
- Object represents any information resource (e.g., files, databases) to be protected in the system. 
- Operation represents an action invoked by a user.
- Subject represents a process acting on behalf of a user in a computer-based system. It could also be another computer system, a node, or a set of attributes. 
- Permission represents an authorization to carry out an action on the system. In general, ACCESS CONTROL LISTs (ACLs) are used to describe DAC policies for its ease in reviewing. An ACL shows permissions in terms of objects, users, and access rights. 
- ReferenceMonitor checks permission for an access request based on DAC policies. If the user has permission to the object, the requested operation may be performed.

![](./Images/discretionary_access_control_structure.png)

*Figure 46: DAC Solution Structure*

## **Dynamics**
Figure 47 shows the collaboration of the DAC pattern for requesting an operation. When an operation is requested for an object, the REFERENCE MONITOR intercepts the request and checks for permission based on DAC policies which is typically described in ACLs. If permission is found, the operation is performed on the object, otherwise, the request is denied.

![](./Images/discretionary_access_control_dynamics.png)

*Figure 47: DAC Collaboration*

## **Variants**
Based on the underlying DAC concepts above, there are several variants [1] which are different by the degree of the strictness in owner’s discretion. DAC variants can be categorized into strict DAC, liberal DAC, and DAC with change of ownership. Strict DAC is the strictest form in which only the owner of an object can grant access to the object. Liberal DAC allows the owner to delegate access granting authority to other users. Liberal DAC can be further divided into one level grant, two-level grant, and multilevel grant, depending on the level at which access granting authority can be passed on. DAC with change of ownership allows the owner to delegate ownership to other users.

## **Implementation**

## **Example Resolved**

## **Consequences**
The DAC pattern has the following advantages: 

- Users can self-manage access privileges. 
- The burden of security administrators is significantly reduced, as resource users and administrators jointly manage permission. 
- Per-user granularity for individual access decisions as well as coarse-grained access for groups are supported. 
- It is easy to change privileges. 
- Supporting new privileges is easy. 

The DAC pattern has the following disadvantages: 

- It is not appropriate for multilayered systems where information flow is restricted. 
- There is no mechanism for restricting rights other than revoking the privilege. 
- It becomes quickly complicated and difficult to maintain access rights as the number of users and resources increases. 
- It is difficult to judge the “reasonable rights” for a user or group. 
- Inconsistencies in policies are possible due to individual delegation of permission. 
- Access may be given to users that are unknown to the owner of the object. This is possible since the user granted authority by the owner can give away access to other users.

## **Known Uses**
Standard Oracle9i [2] uses the DAC pattern to mediate user access to data via database privileges such as SELECT, INSERT, UPDATE and DELETE. The TOE [3], a sensitive data protection product developed by The Common Criteria Evaluation and Validation Scheme (CCEVS), uses the DAC pattern to mediate access to cryptographic keys to prevent unauthorized access. Windows NT implements the DAC pattern to control generic access rights such as No Access, Read, Change, and Full Control for different types of groups (e.g., Everyone, Interactive, Network, Owner).

## **See Also**
The AUTHORIZATION pattern which addresses accessing objects by subjects. The AUTHORIZATION pattern has the concept of delegation as in the DAC pattern. However, unlike the DAC pattern, the access request may need not specify a particular object in the rule. It may be implied by the existing objects being protected.

## **References**

[1] Sandhu, R., & Munawer, Q. (1998, October). How to do discretionary access control using roles. In *Proceedings of the third ACM workshop on Role-based access control* (pp. 47-54). 

[2] Oracle9i. (n.d.) http://www.oracle.com/technology/products/oracle9i/datasheets/ols/OLS9iR2ds.html. 

[3] TOE. (n.d.) http://niap.nist.gov/cc/-scheme/st/ST VID4048.html. 

## **Source**
Kim, D. K., Mehta, P., & Gokhale, P. (2006, October). Describing access control models as design patterns using roles. In *Proceedings of the 2006 conference on Pattern languages of programs* (pp. 1-10).
