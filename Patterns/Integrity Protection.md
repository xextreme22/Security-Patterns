# **Integrity Protection**

## **Intent**
A system protects its integrity by preventing harmful actions. This is done by the interception of defined action classes and identifying their impact on the system’s integrity. If a specific action would violate the integrity, the system refuses its execution.

## **Example**


## **Context**
A computer system runs a set of software modules. Different types of adversaries may benefit from altering existing modules to act maliciously or to add completely new modules that act in their behalf since the system may protect critical information or resources. The system may be a server, a PC, any kind of embedded system or a virtual machine. However, it runs autonomously (i.e., with little human interaction) most of the time.

## **Problem**
An adversary tries to tamper with the software modules on the system. A software module that is modified without authorization possibly leaks critical information or resources or harms the functionality of the overall system. Therefore, the integrity of running software modules has to be ensured.

The following forces apply:

- Prevent Execution of Malicious Software: Execution of malicious software modules harms the integrity of the overall system. After the modules are executed, the integrity violation can be located in any subsystem and may be hard to find. Therefore, the execution of the malicious module should be prevented in the first place.
- Integrity without Monitoring: The system might not be accessible, and it is not possible to monitor it all the time. Though, the integrity of the system should be ensured at any time. 
- Updatability: The solution should provide the possibility to do software updates or install additional software modules without changing the underlying system. 
- No Mutation: The solution should not require changes in existing modules.

## **Solution**
The system either ensures that all loaded software modules are known and verified before, or it ensures that no module is able to violate the integrity of other modules or the whole system.

## **Structure**
As shown in Figure 15, the system is running a set of SoftwareModules. Calls to the SystemAPI are redirected to the DecisionUnit. In order to decide whether a specific call is allowed or not, the DecisionUnit performs an identification of the request via the RequestIdentificationUnit and compares the result to an IntegrityPolicy.

![](./Images/integrity_protection_structure.png)

*Figure 15: INTEGRITY PROTECTION: The system intercepts privileged system requests of all software modules to prevent integrity violations.*

## **Dynamics**
Figure 16 illustrates the basic procedure: Whenever SoftwareModule calls a function of the SystemAPI, the DecisionUnit intercepts this call and triggers the RequestIdentificationUnit. This module gathers information about the request and the result is compared to the IntegrityPolicy. Based on this policy, the DecisionUnit either forwards the call to the actual SystemAPI or returns an error.

![](./Images/integrity_protection_dynamics.png)

*Figure 16: INTEGRITY PROTECTION: The decision unit intercepts system requests and only allows them if they conform to the policy (i.e., would not violate system’s integrity).*

## **Implementation**
The implementation of the DecisionUnit can be done in different ways. Systems that implement the SECURE BOOT pattern verify a software module by hashing its binary and comparing it to a known value prior to execution. On the other hand, systems based on the SANDBOX pattern are verifying all calls to the SystemAPI and decide whether it is allowed for the specific SoftwareModule or not.

## **Example Resolved**


## **Consequences**
Benefits:

- Prevent Execution of Malicious Software: Assuming that the IntegrityPolicy is complete, the integrity checks prevent malicious software from being executed on the system. 
- Integrity without Monitoring: No external supervisor is needed. The system enforces its integrity autonomously. 
- No Mutation: There is no need to alter the software modules itself. —(Updatability): Based on the chosen implementation, little (e.g., sign the new module) or no effort is needed to add additional non-harmful modules to the system. 

Liabilities:

- Maintenance: In systems with very volatile and heterogeneous configurations, the maintenance of the IntegrityPolicy is potentially hard. 
- Performance: INTEGRITY PROTECTION may reduce the performance significantly. Especially on lightweight devices, this might be a problem. 
- Updatability: Adding software that does not conform to the IntegrityPolicy is not possible. Similar problems may arise with software updates.

## **Known Uses**
Since there exist a variety of implementations of both, the SANDBOX pattern [1,2] and the SECURE BOOT pattern [3], the INTEGRITY PROTECTION pattern is implemented in many systems. Some systems, like Android, combine both implementation patterns to ensure integrity. Another example is a shadow stack (example, ROPDefender [4]). Here redundancy is used to protect software against stack overflow attacks. A shadow copy of the actual stack is held and, on each return, the system checks whether the return addresses of the stack and the shadow stack are equal. If not, the stack integrity is properly harmed.

## **See Also**
- STATIC INTEGRITY PROPERTIES and DYNAMIC INTEGRITY PROPERTIES are specializations of this pattern.

## **References**

[1] Loscocco, P., & Smalley, S. (2001). Integrating flexible support for security policies into the Linux operating system. In *2001 USENIX Annual Technical Conference (USENIX ATC 01)*.
[2] Cowan, C., Beattie, S., Kroah-Hartman, G., Pu, C., Wagle, P., & Gligor, V. (2000). {SubDomain}: Parsimonious Server Security. In *14th Systems Administration Conference (LISA 2000)*.
[3] Safford, D., & Zohar, M. (2005). Trusted computing and open source. *Information Security Technical Report*, *10*(2), 74-82.
[4] Davi, L., Sadeghi, A. R., & Winandy, M. (2011, March). ROPdefender: A detection tool to defend against return-oriented programming attacks. In *Proceedings of the 6th ACM Symposium on Information, Computer and Communications Security* (pp. 40-51).

## **Sources**
Rauter, T., Höller, A., Iber, J., & Kreiner, C. (2015, July). Patterns for software integrity protection. In *Proceedings of the 20th European Conference on Pattern Languages of Programs* (pp. 1-10).
