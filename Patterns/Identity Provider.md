# **Identity Provider**

## **Intent**
The IDENTITY PROVIDER pattern allows the centralization of the administration of subjects’ identity information for a security domain.

## **Example**
Having applied the CIRCLE OF TRUST pattern, we can trust the identity providers who are members of our federation, but we still have a variety of identities.

## **Context**
One or several resources, such as web services, CORBA services, applications and so on that are accessed by a predetermined set of subjects. The subjects and resources are typically from the same organization.

## **Problem**
Each application or service may implement its own code for managing subjects’ identity information, leading to an overloading of implementation and maintenance costs that may lead to inconsistencies across the organization’s units.

## **Solution**
The management of the subjects’ information for an organization is centralized in an IDENTITY PROVIDER, which is responsible for storing and propagating parts of the subjects’ information (that form their identity) to the applications and services that need it. 

We define a security domain as the set of resources whose subjects’ identities are managed by the IDENTITY PROVIDER. Typically, the IDENTITY PROVIDER issues a set of credentials to each subject that will be verified by the accessed resources. Notice that the security domain is a special kind of CIRCLE OF TRUST within an organization.

## **Structure**
Figure 127 shows a UML class diagram describing the structure of the solution. This pattern combines the CIRCLE OF TRUST, making explicit its Resources and specializing ServiceProvider to the IdentityProvider. Subjects are identified using CREDENTIALs given by a specific identity provider.

![](./Images/identity_provider_structure.png)

*Figure 127: Class diagram for the IDENTITY PROVIDER pattern*

## **Dynamics**

## **Implementation**

## **Example Resolved**
Now that we can trust the identity providers who are members of our federation and have a centralized identity managed by a specific identity provider, we do not need multiple identities.

## **Consequences**
The IDENTITY PROVIDER pattern offers the following benefits: 

- Maintenance costs are reduced.
- The system is consistent in terms of its users.

It also suffers from the following liability:

- The IDENTITY PROVIDER can be a bottleneck in the organization’s network.
  
## **Known Uses**
- Identity management products, such as IBM Tivoli [1], Sun One Identity Server [2], Netegrity’s SiteMinder and WSO2 [3]. 
- SAP NetWeaver Identity Management [4] manages user access to applications by providing a central mechanism for provisioning users in accordance with their current business roles. It also supports related processes, such as password management, self-service, and approvals workflow. 
- The Empower Identity Manager is oriented to cloud computing [5].

## **See Also**
IDENTITY FEDERATION uses this pattern, as well as the CREDENTIAL and CIRCLE OF TRUST patterns.

## **References**
[1] IBM. (n.d.). Tivoli Federated Identity Manager. <http://www306.ibm.com/software/tivoli/products/federated-identity-mgr/>

[2] Sun. (n.d.). Java System Access Manager. <http://www.sun.com/software/products/access_mgr/>

[3] WSO2. (n.d.). Identity Server. <http://wso2.com/products/identity-server>

[4] SAP. (n.d.). Netweaver Identity Manager. <http://www.sap.com/platform/netweaver/components/idm/index.epx>

[5] Empower Identity Manager. (n.d.). <http://www.identitymanagement.com/?_kk=identity%20management&_kt=d37d8c67-315a4919-abfc-41011051bd9e&gclid=CNrgw7ylnq8CFcNa7Aod90Shaw> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
