# **Identity Federation**

## **Intent**
The IDENTITY FEDERATION pattern allows the formation of a dynamically created identity within an identity federation consisting of several service providers. Therefore, identity and security information about a subject can be transmitted in a transparent way for the user among service providers from different security domains.

## **Example**
Having a centralized identity is not much good unless we can use it in many places. We need some structure to allow this behavior.

## **Context**
We have several security domains in a distributed environment that trust each other. A security domain is a set of resources (web services, applications, CORBA services and so on) in which administration of security is performed by a unique entity, which typically stores identity information about the subjects known to the domain. Subjects can perform actions in one or more security domains.

## **Problem**
There may be no relationship between some of the security domains accessed by a subject. Thus, subjects may have multiple unrelated identities within each security domain. Consequently, they may experience multiple and cumbersome registrations, authentications, and other identity related tasks prior to accessing the services they need. 

How can we avoid the inconvenience of multiple registrations and authorizations across security domains? 

The solution to this problem must resolve the following forces:

- The identity of a user can be represented in a variety of ways in different domains. 
- Parts of a subject’s identity within a security domain may include sensitive information that should not be disclosed to other security domains. 
- The identity and security-related information in transit between two security domains should be kept confidential, so that eavesdropping, tampering or identity theft cannot be realized. 
- A subject may want to access a security domain’s resources in an anonymous way.

## **Solution**
Service providers, which are normally part of a security domain in which the local identity of subjects is managed by an IDENTITY PROVIDER, form identity federations by developing offline operating agreements with other service providers from other security domains. In particular, they can agree about their privacy policies. 

In a security domain, the local identity associated with a user consists of a set of attributes. Some of those attributes can be marked as confidential and should not be passed to other security domains. A federated identity is created gradually and transparently by gathering some of a subject’s attributes from its local identities within an identity federation. Therefore, identity and security information about a subject can be transmitted between service providers from the same identity federation transparently to the user. In particular, its authentication status can be propagated to perform single sign-on within the identity federation.

Figure 123 illustrates an example of how security domains and identity federations can coexist: a security domain is typically a circle of trust within an organization, whereas an identity federation is a circle of trust whose members can come from different organizations.

![](./Images/identity_federation_solution.png)

*Figure 123: Federation and domains*

## **Structure**
Figure 124 shows a UML class diagram with an OCL constraint describing the structure of the solution. An IdentityFederation consists of a set of ServiceProviders which provide services to Subjects. A Subject has multiple LocalIdentities with some ServiceProviders. A LocalIdentity can be described as a set of Attributes of the Subject.

![](./Images/identity_federation_structure.png)

*Figure 124: Class diagram for the IDENTITY FEDERATION pattern*

A Subject can have several FederatedIdentities. This FederatedIdentity is composed of a union of attributes from the LocalIdentities of the IdentityFederation. An IdentityProvider is responsible for managing the LocalIdentities within a SecurityDomain and can authenticate any Subjects on behalf of any ServiceProvider of the IdentityFederation. A Subject has been issued a set of Credentials that collect information about its authentication status and its identity within a SecurityDomain.

## **Dynamics**
We illustrate the dynamic aspects of the IDENTITY FEDERATION pattern by showing sequence diagrams for two use cases: ‘Federate two local identities’ (Figure 125) and ‘Single sign on’ (Figure 126). The first use case describes how a local entity invites another to join, and their MUTUAL AUTHENTICATION. The second use case shows how a subject, after receiving a credential from an IDENTITY PROVIDER, uses it to access a new domain.

![](./Images/identity_federation_dynamics_1.png)

*Figure 125: Sequence diagram for the use case ‘Federate two local identities’*

![](./Images/identity_federation_dynamics_2.png)

*Figure 126: Sequence diagram for the use case ‘Single sign on’*

## **Implementation**
The identity federation can be structured hierarchically or in a peer-topeer manner. The most basic federation could be based on bilateral agreements between two service providers. In the LIBERTY ALLIANCE IDENTITY FEDERATION pattern [1], an Identity Federation has an IDENTITY PROVIDER responsible for managing the federated identity, whereas Shibboleth [2] defines a club as a set of service providers that has reached some operating agreements. 

In the LIBERTY ALLIANCE IDENTITY FEDERATION model, the IDENTITY PROVIDER proposes incentives for other service providers to affiliate with them and federate their local identities. Furthermore, no attribute can be classified as private: privacy is achieved by letting the user provide their consent each time its identity is federated.

## **Example Resolved**
Having implemented an IDENTITY FEDERATION, we can visit different services using the same identity for all of them.

## **Consequences**
The IDENTITY FEDERATION pattern offers the following benefits: 

- Subjects can access resources within the identity federation in a seamless and secure way without reauthenticating in each new domain. 
- Many different representations of the identity of a user can be consolidated under the same federated identity. 
- Subjects can classify some of their attributes as private. Therefore, an IDENTITY PROVIDER can identify which attributes it should transmit to other parties and which it should not. 
- Parts of the security credentials issued about a subject can be encrypted, so that the subject’s privacy can be protected. 
- The security credential can be signed, so that its integrity and authenticity is protected, and attackers cannot forge security tokens or change some of the subject’s attributes. 
- A subject can access a security domain’s resources in an anonymous way, since not all attributes from a local identity (such as a name) are required to be federated. 

The pattern also has some potential liabilities: 

- Service providers need to have some kind of agreement before their identities can be federated. They have to exchange credentials through some external channel to trust and recognize each other. 
- Even when a subject’s sensitive information is classified as private, a security domain can still disclose the subject’s private information secretly to other parties, thus violating the subject’s privacy. 
- A security token can be stolen and presented by an attacker, resulting in identity theft. This is alleviated by the use of expiration dates and unique IDs for credentials. 
- In spite of an expiration date and the unique ID feature of a credential, which guarantees its freshness, the unconditional revocation of a credential is not addressed in this solution.

## **Known Uses**
- WS-Federation [3] is a proposed standard allowing web services to federate their identities. 
- Liberty Alliance is a standard that allows services to federate into Identity Federations [4]. 
- Microsoft Account (previously Microsoft Wallet, Microsoft Passport, .NET Passport, Microsoft Passport Network and Windows Live ID) is an identity federation that lets users log in to a variety of web sites using only one account [5]. 
- Shibboleth is an open solution for realizing identity federation among enterprises [2].

## **See Also**
- LIBERTY ALLIANCE IDENTITY FEDERATION is a specialization of this pattern [1]. 
- The CREDENTIAL pattern. 
- A Single Sign On pattern is shown in [6]. 
- A formal description of identity management patterns is given in [7].

## **References**
[1] Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.

[2] InCommon. (n.d.-b). Shibboleth Project. http://shibboleth.internet2.edu/

[3] Ajaj, O., & Fernandez, E. B. (n.d.). A pattern for the WS-Federation standard for web services. in preparation

[4] Liberty Alliance Project. (n.d.). <http://www.projectliberty.org/>

[5] Microsoft Architecture Journal. (n.d.). Identity and Access. Journal 16

[6] Steel, C., & Nagappan, R. (2006). *Core Security Patterns: Best Practices and Strategies for J2EE", Web Services, and Identity Management*. Pearson Education India.

[7] Nizamani, H. A., & Tuosto, E. (2011). Patterns of Federated Identity Management Systems as Architectural Reconfigurations. *Electronic Communications of the EASST*, *31*.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
