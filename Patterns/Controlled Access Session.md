# **Controlled Access Session**

## **Intent**
The CONTROLLED ACCESS SESSION pattern describes how to provide a context in which a subject (user, system) can access resources with different rights without need to reauthenticate every time they access a new resource.

## **Example**
Lisa is a secretary in a medical organization who sometimes helps with patient tests in the laboratory. As a secretary she has access to patients’ information such as name, address, social security number and so on. This is necessary so that she can bill them and their insurance companies. In the lab she has access to anonymized patient test results. Combining the accesses provided by her two jobs in one window allows her to associate test results and patients’ names, which violates patient privacy.

## **Context**
Any environment in which we need to control access to computing resources and in which users can be classified according to their jobs, groups, departments, assignments, or tasks.

## **Problem**
A given user may be authorized to access a system because they need to perform several functional activities. However, for a particular access, only those privileges should be active that are necessary to perform the intended task. This is an application of the principle of least privilege and is necessary to prevent the user from misusing the system, either intentionally, accidentally by performing an error, or without knowledge and tricked to do so, for example through a Trojan Horse attack. Additionally, it potentially restricts damage in the case of session hijacking: a successful attack process would not have all the privileges of a user available, only the active subset. 

The solution to this problem must resolve the following forces:

- Subjects may have many rights directly or indirectly through the execution contexts they need for their tasks. Using all of them at one time may result in conflicts of interest and security violations. We need to restrict the use of those rights depending on the application or task the subject is performing. 
- In the context of an interaction, we can make access to some functions implicit, thus facilitating the use of the system and preventing errors that may result in vulnerabilities. For example, some editors or other tools could be implicitly available in some sessions. 
- It is not convenient to make subjects reauthenticate every time they request a new resource. Once the subject is authenticated, this condition should remain valid during the whole session.

## **Solution**
Define a unit of interaction, a session, which has a limited lifetime, for example between login and logoff of a user, or between the beginning and the end of a transaction. When a user logs on and after authentication, the session activates some execution contexts with only a subset of the authorizations they possess. This should be the minimum subset that is needed for the user or transaction to perform the intended task. Only those rights are available within the session. A subject can be in several sessions at the same time; however, in every session only the necessary rights are active.

## **Structure**
Figure 115 shows the class model of the CONTROLLED ACCESS SESSION pattern. The classes Subject and Session have obvious meanings. The class ExecutionContext contains the set of active rights that the subject may use within the session.

![](./Images/controlled_access_session_structure.png)

*Figure 115: Class model for the CONTROLLED ACCESS SESSION pattern*

## **Dynamics**
Figure 116 shows the use case ‘Open (activate) a session’. A subject logs on and the logon interface authenticates it. The box with a double arrow indicates some authentication dialog or protocol. After the subject is authenticated, the interface creates a session object and returns a handle to the subject.

![](./Images/controlled_access_session_dynamics.png)

*Figure 116: Sequence diagram for the use case ‘Open a session’*

## **Implementation**
Based on institution and application policies, define which contexts (implying specific rights) should be used in each task and grant them to the corresponding subject. The rights should be selected using the least privilege principle, and there should be no contexts with excessive rights; for example, the administrator rights should be divided into smaller sets.

## **Example Resolved**
Lisa can log on a secretary or as a lab assistant, but she cannot combine these activities in one session. Now she cannot relate test results to patients’ names.

## **Consequences**
The CONTROLLED ACCESS SESSION pattern offers the following benefits:

- We can give only the necessary rights to each execution context, according to its function, and we can invoke in a session only those contexts that are needed for a given task. 
- We can exclude combinations of contexts that might result in possible access violations or conflicts of interest. 
- Any functions can be made implicit in a session. 
- Once a subject starts a session it doesn’t have to be reauthenticated: its status is kept by the session. 

The pattern also has the following potential liabilities: 

- If we need to apply fine-grained access, it might be inefficient to include many contexts in order to perform complex activities. 
- Using sessions may be confusing to the users.

## **Known Uses**
- Session Access is part of the RBAC standard proposal by NIST, which has been adopted by the American National Standards Institute, International Committee for Information Technology Standards (ANSI/INCITS) as ANSI INCITS 359-2004 [1]. 
- Multics [2] used execution contexts (based on projects) to limit access rights. 
- Session Access is implemented in the security module CSAP [3] of the Webocrat system in conjunction with an RBAC policy. 
- Views in relational databases can be used to define sets of rights. Controlling the use of views by users can control their use of rights in sessions. This is done for example in Oracle and DB2, where SQL can be used to define restricted views [4].

## **See Also**
- The Access Session pattern is used in the SESSION-BASED ROLEBASED ACCESS CONTROL pattern and Attribute-Based Access Control patterns. 
- The SECURITY SESSION pattern intents to prevent a user from having to be reauthenticated every time they access a new object. 
- Abstract Session [5] is a pattern with a similar objective to SECURITY SESSION: when an object’s services are invoked by clients, the server object may have to maintain state for each client. The server creates a session object that encapsulates state information for the client and returns a pointer to the session object. 

However, none of these patterns considers limitation of rights. Our pattern is an extension of these patterns, concentrating all its security functions and emphasizing the function of a session as a limiter of rights.

See ACTOR AND ROLE LIFECYCLE pattern.

## **References**
[1] Ferraiolo, D. F., Sandhu, R., Gavrila, S., Kuhn, D. R., & Chandramouli, R. (2001). Proposed NIST standard for role-based access control. *ACM Transactions on Information and System Security (TISSEC)*, *4*(3), 224-274.

[2] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc..

[3] Dridi, F., Fischer, M., & Pernul, G. (2003, May). CSAP—An Adaptable Security Module for the E-Government System Webocrat. In *IFIP International Information Security Conference* (pp. 301-312). Springer, Boston, MA.

[4] Elmasri, R., & Navathe, S. (2003). Fundamentals of Database Systems. 4th edition. Addison-Wesley.

[5] Pryce, N. (2001). Abstract session: An object structural pattern. In *Design patterns in communications software* (pp. 191-208).

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
