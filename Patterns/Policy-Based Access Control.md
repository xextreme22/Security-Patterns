# **Policy-Based Access Control**

## **Intent**
The POLICY-BASED ACCESS CONTROL pattern describes how to decide whether a subject is authorized to access an object according to policies defined in a central policy repository.

## **Example**
Consider a financial company that provides services to its clients. Their computer systems can be accessed by clients, who send orders to the company for buying or selling commodities (stocks, bonds, real estate, art and so on) via e-mail or through their website. Brokers employed by the company can carry out the clients’ orders by sending requests to the systems of various financial markets, or by consulting information from financial news websites. A government auditor visits periodically to check for compliance with laws and regulations.

All of these activities are regulated by policies with various granularities within the company. For example, the billing department can have the rule ‘Only registered clients whose account status is in good standing may send orders’, the technical department can decide that ‘E-mails with attachments bigger than x Mb won’t be delivered’, the company security policy can state that ‘Only employees with a broker role can access the financial market’s web services’ and that ‘Only the broker or custodian of a client can access its transaction information’, whereas the legal department can issue the rule that ‘Auditors can access all transaction information’, and so on.

All of these policies are enforced by different components of the company’s computer system (e-mail server, file system, web service access control component, and financial application). This approach has several problems: the policies may be described in different syntaxes, and it is difficult to have a global view of which policies apply to a specific case. Moreover, two policies can be conflicting, and there is no way to combine them in a clear way. In summary, this approach could be error prone and complex to manage.

## **Context**
Consider centralized or distributed systems with a large number of resources (objects). A large number of subjects may access those objects. Rules are defined to control access to objects. The rules defined by the organization are typically designed by different actors (technical, organizational, legal, and so on), and each set of rules designed by a specific policy designer can concern overlapping sets of objects and/or subjects.

## **Problem**
Enforcing these rules for a particular access request may be complex, and thus error-prone, because there is no clear view of which rules to apply to a request. 

How can we enforce access control according to the predefined rules in a consistent way? 

The solution to this problem must resolve the following forces: 

- Objects may be frequently added or removed. 
- The solution should be able to implement a wide variety of access control models, such as access matrix, RBAC. 
- Malicious users can attempt unauthorized access to objects. 
- There should be no direct access to objects: every request must be mediated.

## **Solution**
Most access control systems are based on the AUTHORIZATION pattern, in which the access of a subject to an object depends only on the existence of a positive applicable rule. If no such rule exists, the access is denied.

In our case, the situation is more complicated: the existence of a positive applicable rule should not necessarily imply that access should be granted. All the rules must be considered, and a final decision must be made from the set of applicable rules and some meta-information about the way they should be combined. Part of that meta-information is located in a policy object. This policy object aggregates a set of rules and specifies how those rules must be combined. For more flexibility about the combination of rules, a composite object regroups the rules into policies and policy sets. Policy sets aggregate policies and include information about how to combine rules from different policies. To be able to select all applicable rules easily, they should be stored in a unique repository for the organization and administered in a centralized way.

At access time, all requests are intercepted by policy enforcement points (PEPs), a specific type of REFERENCE MONITOR. The repository is accessed by a unique policy decision point (PDP), which is responsible for computing the access decision by cooperating with a policy information point (PIP), which may provide information about the subject, or the resource accessed. The rules and policies are administered through a unique policy administration point (PAP).

Finally, because rules and policies are designed by different teams, possibly for the same objects and subjects, this scheme does not guarantee that a conflict between rules in different policy components would never occur. In that case, the PDP may have a dynamic policy conflict resolver to resolve the conflict, which would need to use meta-rules. A complementary static policy conflict resolver may be a part of the PAP and should detect conflicts between rules at the time they are entered into the repository.

## **Structure**
Figure 156 illustrates the solution. A Subject’s access requests to particular objects are intercepted by PEPs, which are a part of the security infrastructure that is responsible for enforcing the organization Policy about this access. PEPs query another part of the security infrastructure, the PDP, which is responsible for computing an access decision. To compute the decision, the PDP uses information from a PIP, and retrieves the applicable Policy from the unique PolicyRepository, which stores all of the PolicyRules for the organization.

![](./Images/policy-based_access_control_structure.png)

*Figure 156: Class diagram for the POLICY-BASED ACCESS CONTROL pattern*

The PolicyRepository is also responsible for retrieving the applicable rules by selecting those rules whose subjectDescriptor, resourceDescriptor and environmentDescriptor match the information about the subject, the resource and the environment obtained from the PIP, and whose accessType matches the required accessType from the request. The PAP is a unique point for administering the rules. In case the evaluation of the Policy leads to a conflict between the decisions of the applicable Rules, a part of the PDP, the DynamicPolicyConflictResolver, is responsible for producing a uniquely determined access decision. Similarly, a StaticPolicyConflictResolver is a part of the PAP and is responsible for identifying conflicting rules within the PolicyRepository.

## **Dynamics**
Figure 157 shows a sequence diagram describing the most commonly used case of ‘Request access to an object’. The Subject’s request for accessing an Object is intercepted by a PEP, which forwards the request to the PDP. The PDP can retrieve information about the Subject, the Object, and the current environment from the PIP. This information is used to retrieve the applicable Rules from the PolicyRepository.

The PDP can then compute the access decision by combining the decisions from the Rules forming the applicable policy and can finally send this decision back to the PEP. If the access has been granted by the PDP, the PEP forwards the request to the Object.

![](./Images/policy-based_access_control_dynamics.png)

*Figure 157: Sequence diagram for the use case ‘Request access to an object’*

## **Implementation**

## **Example Resolved**
Use of the POLICY-BASED ACCESS CONTROL pattern allows the company to centralize its rules. Now the billing department, as well as the technical department, the legal department and the corporate management department can insert their rules in the same repository, using the same format. The different components of the computer system that used to enforce policies directly (that is, e-mail server, file system, web service access control component and financial application) just need to intercept the requests and redirect them to the central policy decision point. To do that, each of them runs a policy enforcement point, which interfaces with the main policy decision point.

The rules could be grouped in the following way: a unique company policy set might include all other policies and express the fact that all policies coming from the corporate management should dominate all other policies. Each department would have their own policy, composed of rules from that department, and combined according to each department’s policy.

Finally, a simple dynamic conflict resolver could be configured to enforce a closed policy in case of conflict. The rules can be managed easily, since they are written to the same repository, conflicts can be resolved, and there is a clearer view of the company’s security policy.

## **Consequences**
The POLICY-BASED ACCESS CONTROL pattern offers the following benefits: 

- Since the access decisions are requested in a standard format, an access decision becomes independent of its enforcement. A wide variety of enforcement mechanisms can be supported and can evolve separately from the policy decision point. 
- The pattern can support the access matrix, RBAC or multilevel models for access control. 
- Since every access is mediated, illegal accesses are less likely to be performed. 

The pattern also has some potential liabilities: 

- It could affect the performance of the protected system since the central PDP/PolicyRepository/PIP subsystem may be a bottleneck in the system. 
- Complexity. 
- We need to protect the access control information.

## **Known Uses**
- XACML (eXtensible Access Control Markup Language), defined by OASIS, uses XML for expressing authorization rules and for access decision following this pattern. 
- Symlabs’ Federated Identity Access Manager Federation is an identity management that implements identity federation. Its components include a PDP and PEPs. 
- Components Framework for Policy-Based Admission Control, a part of the Internet 2 project, is a framework for the authentication of network components. It is based on five major components: Access Requester (AR), Policy Enforcement Point (PEP), Policy Decision Point (PDP), Policy Repository (PR) and the Network Detection Point (NDP). 
- XML and Application firewalls [1] also use policies. 
- SAML (Security Assertion Markup Language) is an XML standard defined by OASIS for exchanging authentication and authorization data between security domains. It can be used to transmit the authorization decision.

## **See Also**
- XACML patterns [1,2] is an implementation of this pattern. 
- The ACCESS CONTROL LIST pattern and CAPABILITY patterns are specific implementations of this pattern. 
- The PEP is just a REFERENCE MONITOR. 
- This pattern can implement the Access Matrix [3] and ROLE-BASED ACCESS CONTROL patterns. 

A general discussion of security policies, including some IETF models that resemble patterns, is given in [4].

## **References**

[1] Delessy, N., & Fernandez, E. B. (2005, September). Patterns for the eXtensible Access Control Markup Language. In Proceedings of the 12th Pattern Languages of Programs Conference (PLoP2005). Monticello. Illinois. Retrieved September 18, 2011, from [http://hillside.net/plop/2005/proceedings/PLoP2005_ndelessyandebfernan dez0_1.pdf](http://hillside.net/plop/2005/proceedings/PLoP2005_ndelessyandebfernan%20dez0_1.pdf) 

[2] Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.

[3] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc.

[4] Sloman, M., & Lupu, E. (2002). Security and management policy specification. *IEEE network*, *16*(2), 10-19.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
