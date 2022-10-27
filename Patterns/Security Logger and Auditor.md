# **Security Logger and Auditor**

## **Intent**
The SECURITY LOGGER AND AUDITOR pattern describes how to keep track of users’ actions in order to determine who did what and when. It logs all security-sensitive actions performed by users and provides controlled access to records for audit purposes.

## **Example**
A hospital uses RBAC to define the rights of its employees. For example, doctors and nurses can read and write medical records and related patient information (lab tests and medicines). When a famous patient came to the hospital, one of the doctors, who was not treating him, read his medical record and leaked this information to the press. When the leak was discovered, there was no way to find out which doctor had accessed the patient’s records.

## **Context**
Any system that handles sensitive data, in which it is necessary to keep a record of access to data.

## **Problem**
How can we keep track of users’ actions in order to determine who did what and when? The solution to this problem must resolve the following forces:

- Accuracy. We should faithfully record what a user or process has done with respect to the use of system resources. 
- Security. Any information we use to keep track of what the users have done must be protected. Unauthorized reading may reveal sensitive information. Tampering may erase past actions. 
- Forensics. When a misuse of data occurs, it may be necessary to audit the access operations performed by users to determine possible unauthorized actions, and maybe trace the attacker or understand how the attack occurred. 
- System improvement. The same misuses may keep occurring; we need to learn from past attacks. 
- Compliance. We need a way to verify and to prove to third parties that we have complied with institution policies and external regulations. 
- Performance. We need to minimize the overhead of logging.

## **Solution**
Each time a user accesses some object, we record this access, indicating the user identifier, the type of access, the object accessed and the time when the access happened. 

The database of access entries must have authentication and authorization systems, and possibly an encryption capability.

## **Structure**
In Figure 164 User operations are logged by the LoggerAuditor. The LoggerAuditor keeps the Log of user accesses, in which each access is described by a LogEntry. The security administrator (SecAdmin) activates or deactivates the Log. The Auditor can read the Log to detect possible unauthorized actions.

![](./Images/security_logger_and_auditor_structure.png)

*Figure 164:  Class diagram of the SECURITY LOGGER AND AUDITOR pattern*

## **Dynamics**
Possible use cases include ‘Log user access’, ‘Audit log’, ‘Query log database’.

A sequence diagram for the use case ‘Log user access’ is shown in Figure 165. The User performs an operation to apply an access type on some object: operation (accessType, object). The LoggerAuditor adds an entry with this information, and the name of the user, to the Log. The Log creates a LogEntry, adding the time of the operation.

![](./Images/security_logger_and_auditor_dynamics.png)

*Figure 165: Sequence diagram for the use case ‘Log user access’*

## **Implementation**
The class diagram shown in Figure 164 provides a clear guideline for implementation, since its classes can be directly implemented in any object-oriented language. We need to define commands to activate or deactivate logging, apply filters, indicate devices to be used, allocate amount of storage, and select security mechanisms. One can filter some logging by selecting users, events, importance of events, times, and objects in the filters. Administrative security actions, for example account creation/deletion, assignment of rights and others, must also be logged.

Logging is performed by calling methods on the LoggerAuditor class. Every non-filtered user operation should be logged. Logged messages can have levels of importance associated with them.

Audit is performed by an auditor reading the log. This can be complemented with manual assessments that include interviewing staff, performing security vulnerability scans, reviewing application and operating system access controls, and analyzing physical access to the systems [1]. The Model-View-Controller pattern can be used to visualize the data using different views during complex statistical analysis of the log data.

## **Example Resolved**
After the incident, the hospital installed a SECURITY LOGGER AND AUDITOR, so in the future such violations can be discovered.

## **Consequences**
The SECURITY LOGGER AND AUDITOR pattern offers the following benefits: 

- Security. It is possible to add appropriate security mechanisms to protect recorded data, for example access control and/or encryption. 
- Forensics. The pattern enables forensic auditing of misused data objects. Records of access can be used to determine whether someone has maliciously gained access to data. This pattern can also be used to log access to data objects by system processes. For example, malicious code planted in the system can be tracked by finding processes that have misused objects. 
- System improvement. By studying how past attacks happened, we can improve the system security. Compliance. Auditing a log can be used to verify and prove that institutional and regulatory policies have been followed. 
- Performance. We can reduce overhead by parallel or background logging. We can also not log some events not considered significant. Finally, we can merge this log with the recovery log, needed for possible rollback. 

The pattern has the following potential liabilities: 

- It can incur significant overhead since each object access has to be logged. 
- A decision must be made by software designers as to the granularity at which objects are logged. There is a trade-off between security and performance. 
- It is not easy to perform forensic analysis, and specialists are required. 
- Protecting the log adds some overhead and cost.

## **Variants**
Most systems have a system logger, used to undo/rollback actions after a system crash. That type of logger has different requirements, but sometimes is merged with the security logger [2]. System logs are of interest to system and database administrators, while security logs are used by security administrators, auditors, and system designers. 

Another variant could include the automatic raising of alarms by periodic examination of the log, searching records that match a number of rules that characterize known violations. For example, intrusion detection systems use this variant. 

We can also add logging for reliability, to detect accidental errors.

## **Known Uses**
- Most modern operating systems, including Microsoft Windows [3], AIX [4], Solaris and others have security loggers. 
- SAP uses both a security audit log and a system log [2].

## **See Also**
- The Secure Logger is a pattern for J2EE [5]. It defines how to capture application-specific events and exceptions to support security auditing. This pattern is mostly implementation-oriented and does not consider the conceptual aspects discussed in our pattern. It should have been called a ‘security logger’ because it does not include any mechanisms to protect the logged information. 
- Martin Fowler has an Audit Log analysis pattern [6] for tracking temporal information. The idea is that whenever something significant happens, you write some record indicating what happened and when it happened. 
- Patterns for authentication: how can we make sure that a subject is who they say they are? 
- AUTHORIZATION describes how we can control who can access to which resources, and how, in a computer system.

## **References**

[1] Wikipedia contributors. (n.d.). Information security audit. Wikipedia. <https://en.wikipedia.org/wiki/Information_security_audit> 

[2] Software AG. (2009). webMethods Audit Logging Guide. <http://documentation.softwareag.com/webmethods/wmsuites/wmsuite8_ga/Cross_Product/8-0SP1_Audit_Logging_Guide.pdf> 

[3] Smith, F. R. (n.d.). Auditing Users and Groups with the Windows Security Log. <http://www.windowsecurity.com/articles/auditing-users-groups-windows-security-log.html> 

[4] AIX. (n.d.). System Security Auditing. <http://www.aixmind.com/?p=1019> 

[5] Steel, C., Nagappan, R., Lai, R. (2006). Securing the Web Tier: Design Strategies and Best Practices (Chapter 9). Sun.

[6] Fowler, M. (n.d.). Audit Log. <http://martinfowler.com/ap2/auditLog.html> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
