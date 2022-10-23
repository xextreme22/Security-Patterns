# **Content-Dependent Authorizer**

## **Intent**
How can we restrict subjects (users, systems, parties) to access (read, write) only data with specific values? Filter the values obtained from applying authorization rules to the data according to a predicate (condition) that selects specific values.

## **Example**
A hospital uses RBAC to protect its patient database. The role “doctor” gives access to all patient records. A doctor found out that a famous singer was in the hospital. Although he was not treating him, he got his medical information and sold it to a newspaper. The singer sued the hospital and collected a large sum of money. Another event like this and the hospital could go out of business.

## **Context**
Some institutions need to keep large amounts of sensitive information. In particular, some institutions keep sensitive information about individuals, other institutions, or about their own businesses. Users can make requests for data after being identified and authenticated (AUTHENTICATOR pattern). We have mechanisms to determine the values of data items. The Authorizer pattern implements the Access Matrix model (s,o,t), which describes access by subjects s (actors, entities) to protected objects o (data/resources) in specific ways t (access types), as represented in Figure 19. The association class Right indicates the access type that can be used by a subject to access an object. The operation “checkRights” can be used to find the objects accessible to a subject or the subjects that can access an object.

![](./Images/content-dependent_authorizer_context.png)

*Figure 19: Class diagram for the Authorizer (Access Matrix) pattern [Fer13].*

## **Problem**
How can we restrict subjects (users, systems, parties) to access only data which is relevant to their functions or concerns them? Without some control subjects may access too much information which may lead to misuses. 

The solution to this problem is constrained by the following forces: 

- Policy—we should be able to express authorization policies that restrict users to access (read or write) only data relevant to their functions or about themselves. 
- Overhead—evaluating access decisions should take a reasonably low time. 
- Security information protection—the information used to decide access should be protected or unauthorized accesses could be possible. 
- Transparency—the users should not need to do any special actions to access the data they want; they should only get security violation warnings in case they ask for data they should not access. 
- Scalability—the number of subjects and objects should be scalable. 
- Accountability—because we are dealing with sensitive data, we should keep track of accesses for future auditing.

## **Solution**
Add to each authorization rule a predicate to constrain the records that the subject can get; now the authorization rule becomes (s, o, t, p), where s is a subject, o is the object, t is a access type, and p is a predicate that evaluates to True or False. The records where the predicate is True are returned to the requester.

## **Structure**
Figure 20 shows the class diagram of this pattern. The Subject represents an active entity able to request access to records. The Object is the class that describes the type of records requested. Right represents the access type allowed to the subject.

The REFERENCE MONITOR OCL constraint is:

if Ǝ r = (s, o, t, p) IN Auth\_Rules • p(s, o, t) = True 

then • records (p=True) s can apply t 

else securityViolation

![](./Images/content-dependent_authorizer_structure.png)

*Figure 20: Class diagram of the CONTENT-DEPENDENT AUTHORIZER pattern.*

## **Dynamics**
Figure 21 is the sequence diagram for the use case “Request some information”.

![](./Images/content-dependent_authorizer_dynamics.png)

*Figure 21: UC: Request some information by value.*

The predicate can refer to a system variable, e.g., day of the week, alarm condition, or similar.

## **Implementation**
- An operating system access control for files is not enough; we need a DBMS so we can select records to return to the requester by checking the value of specific fields in records. 
- If the number of records is high, it may take a long time to find the matching records. 
- Record selection can be accelerated by using compile-time evaluation of queries.

## **Example Resolved**
The affected patient was not a patient of the doctor who leaked his information. The hospital now uses CONTENT-DEPENDENT AUTHORIZER and restricts doctors to access only the information of their own patients. This will not totally eliminate the problem because doctors can still leak information about their own patients but will significantly reduce it.

## **Consequences**
This pattern has the following advantages: 

- Policy—we can express any access control policy that depends on data values or combinations of them. 
- Overhead—evaluating access decisions takes a reasonably low time if the number of records is not too high or we use some acceleration approach. 
- Security information protection—the information used to decide access can be protected in the same way (put authorization rules in tables of the relational database). 
- Transparency—the users do not need to do any special actions, except to execute queries, they may get security violation warnings. 
- Scalability—the number of subjects and objects can increase significantly. Specific implementations can limit the number of records to be retrieved. 
- Accountability—we can keep track of accesses for future auditing using the SECURITY LOGGER AND AUDITOR together with this pattern. 

Its liabilities include: 

- Overhead—if many records or relational joins are involved, the overhead can be considerable.

## **Known Uses**
Almost all DBMS products can enforce this type of authorization: 

- Oracle [1]. The Oracle DBMS includes authentication, authorization, and logging. The authorization mechanism provides content-dependent access control. 
- DB2 [2]—a family of relational database servers. In recent years some products have been extended to support object-relational features and non-relational structures, in particular XML. 
- Microsoft DB Server [3]. 
- MySQL [4].

## **See Also**
- AUTHENTICATOR: When a user or system (subject) identifies itself to the system, how do we verify that the subject intending to access the system is who it says it is? 
- Authorizer: A.K.A. Access Matrix. Describes who is authorized to access specific resources in a system, in an environment in which we have resources whose access needs to be controlled. It indicates for each active entity, which resources it can access, and what it can do with them. 
- REFERENCE MONITOR: Enforces the authorization rules defined by different types of authorizers. 
- SECURITY LOGGER AND AUDITOR: How can we keep track of user’s actions in order to determine who did what and when? Log all security-sensitive actions performed by users and provide controlled access to records for Audit purposes. 
- FINE-GRAIN ACCESS CONTROL pattern restrict access to specific fields of objects.

## **References**

[1] Oracle. (2012, July). *Security and Compliance*. Oracle Database. Retrieved February 28, 2014, from https://www.oracle.com/technetwork/database/security/index.html

[2] IBM. (n.d.). IBM Db2 – Data Management Software. https://www.ibm.com/analytics/db2

[3] Wikipedia contributors. (2014). Microsoft SQL Server. Wikipedia. https://en.wikipedia.org/wiki/Microsoft\_SQL\_Server

[4] MySQL. (n.d.). Security in MySQL. Retrieved April 2, 2014, from https://dev.mysql.com/doc/mysql-security-excerpt/5.1/en/

## **Sources**
Fernández, E. B., Monge, A. R., Carvajal, R., Encina, O., Hernández, J., & Silva, P. (2014, July). Patterns for content-dependent and context-enhanced authorization. In *Proceedings of the 19th European Conference on Pattern Languages of Programs* (pp. 1-10).
