# **Credential**

## **Intent**
The CREDENTIAL pattern provides a secure means of recording authentication and authorization information for use in distributed systems.

## **Example**
Suppose we are building an instant messaging service to be used by members of a university community. Students, teachers, and staff of the university may communicate with each other, while outside parties are excluded. Members of the community may use computers on school grounds, or their own systems, so the client software is made available to the community and is installed on the computers of their choice. Any community member may use any computer with the client software installed. 

The client software communicates with servers run by the university in order to locate active participants and to exchange messages with them. In this environment, it is important to establish that the user of the client software is a member of the community, so that communications are kept private to the community. Further, when a student graduates, or an employee leaves the university, it must be possible to revoke their communications rights. Each member needs to be uniquely and correctly identified, and a member’s identity should not be forgeable.

## **Context**
Systems in which the users of one system may wish to access the resources of another system, based on a notion of trust shared between the systems.

## **Problem**
In centralized computer systems, the authentication and authorization of a principal can be handled by that system’s operating system, middleware and/or application software; all attributes of the principal’s identity and authorization are created by and are available to the system. With distributed systems this is no longer the case. A principal’s identity, authentication, and authorization on one system does not carry over to another system. If a principal is to gain appropriate access to another system, some means of conveying this information must be introduced. 

More broadly, this is a problem of exchanging data between trust boundaries. Within a given trust boundary, a single authority is in control, and can authenticate and make access decisions on its own. If the system is to accept requests from outside its own authority/trust boundary, the system has no inherent way of validating the identity or authorization of the entity making that request. How then do we allow external users to access some of our resources? 

The solution to this problem must resolve the following forces:

- Privacy. The user must provide enough information to grant authorization, without being exposed to intrusive data mining. 
- Persistence. The information must be packaged and stored in a way that survives travel between systems, while allowing the data to be kept private. 
- Authentication. The data available must be sufficient for identifying the principal to the satisfaction of the accepting system’s requirements, while disallowing others from accessing the system. 
- Authorization. The data available must be sufficient for determining what actions the presenting principal is permitted to take within the accepting system, while also disallowing actions the principal is not permitted to take. 
- Trust. The system accepting the credential must trust the system issuing the credential. 
- Generation. There must be entities that produce the credentials such that other domains recognize them. 
- Tamper freedom. It should be very difficult to falsify the credential. 
- Validity. The credential should have an explicit temporal validity. 
- Additional documents. It might be necessary to use the credential together with other documents. 
- Revocation. It should be possible to revoke the credential conveniently.

## **Solution**
Store authentication and authorization data in a data structure external to the systems in which the data is created and used. When presented to a system, the data (credential) can be used to grant access and authorization rights to the requester. For this to be a meaningful security arrangement, there must be an agreement between the systems which create the credential (credential authority) and the systems which allow their use, dictating the terms and limitations of system access.

## **Structure**
In Figure 117 the principal is an active entity such as a person or a process. The principal possesses a Credential, representing its identity and its authorization rights. A Credential is a composite describing facts about the rights available to the principal. The Attribute may flag whether it is presently enabled, allowing principal control over whether to exercise the right implied by the Credential. An expiration date allows control over the duration of the rights implied by the attribute.

![](./Images/credential_structure.png)

*Figure 117: Class diagram for the CREDENTIAL pattern*

A Credential is issued by an Authority and is checked by an AUTHENTICATOR or an Authorizer. Specialization of a Credential is achieved through setting Attribute names and values. 

Some specializations of Attributes are worth mentioning. Identity, created by setting an attribute name to, say, ‘username’ and the value to the appropriate username instance, shows that the subject has been authenticated and identified as a user known to the AUTHENTICATOR. Privilege, named after the intended privilege, implies some specific ability granted to the subject. Group and Role can be indicated in a similar fashion to Identity.

## **Dynamics**
There are four primary use cases: 

1. Issue credential, by which a credential is granted to the principal by an authority. 
1. Principal authentication, where an authenticator accepts a credential provided to it by a principal and makes an access decision based on the credential. 
1. Principal authorization, in which the principal is allowed access to specific items. 
1. Revoke credential, in which a principal’s credential are invalidated.

### **Issue Credential – Figure 118**
The principal presents itself and any required documentation of its identity to an Authority. Based upon its rules and what it ascertains about the Principal, the Authority creates and returns a Credential. The returned data may include an identity credential, group and role membership credential attribute, and privilege credential attributes. As a special case, the Authority may generate a defined ‘public’ Credential for Principals not previously known to the system. This Credential is made available to Authenticators which reference this Authority.

![](./Images/credential_dynamics_1.png)

*Figure 118: Sequence diagram for the use case ‘Issue credential’*

### **Principal Authentication – Figure 119**
The principal requests authentication at an Authenticator, supplying its name and authentication Credential. The Authenticator checks the Credential and makes an access decision. There are different phases and strengths of check that may be appropriate for this step, discussed in the Implementation section. It is necessary for the Authenticator to be established in conjunction with the original authority. Not shown in the sequence diagram, but it is also optionally possible to forward the authentication request and credentials to the authority for verification.

![](./Images/credential_dynamics_2.png)

*Figure 119: Sequence diagram for the use case ‘Principal authentication’*

### **Revoke Credential**
If it is determined that a given principal should no longer have access to the system, or that a principal’s credentials have been stolen or forged, the authority can issue a revocation message to each authenticator and authorizer. Once this message has been received, the authentication and authorization subsystems reject future requests from the affected credentials. If the principal is still authorized to use the system, new credentials must be issued.

## **Implementation**
The most significant factor in implementing the CREDENTIAL pattern is to determine the nature of the agreement between the participating systems. This begins with consideration of the functions to be provided by the system to which credentials will give access, the potential users of those functions, and the set of rights that are required for each user to fulfill its role. Once these are understood, a clear representation of the subjects, objects and rights can be developed. This representation forms the basis for storing credentials in some persistent medium and sets the terms of authentication and authorization. It also forms the basis for portability, as persisted data may be placed on portable media for transmission to the location(s) of its use.

The problem with a clear representation of security rights is that potential attackers can read them as well as valid participants in the systems in question. In the physical world, anti-forgery devices for credentials use techniques such as embedding the credential data in media that is too expensive to be worth forging for the benefit received: drivers’ licenses and other ID cards, passports and currency all are based on the idea that it is too complex and costly for the majority of users to create realistic fakes.

In the digital world, however, copies are cheap. There are two common means of addressing this. One is to require that credentials be established and used within a closed context, and encrypting the communications channels used in that context. The other is to encrypt the credentials when they are issued, and to set up matching decryption on the authenticating system. This further subdivides into ‘shared secret’ systems, in which the issuing and accepting systems share the cryptographic keys necessary to encrypt and decrypt credentials, and ‘public key’ systems, in which participating systems can establish means for mutual encryption/decryption without prior sharing. These design choices are part of the terms set by the authority agreement under which the credentials apply. The authenticator must use the same scheme as the authority. Kerberos tokens and X.509 certificates are examples of this that require more specific approaches [1].

As a simple example of ‘shared secret’ systems, consider a typical online banking authority and authentication setup; at sign-up, the customer verifies their identity to the bank, the authority. As part of the bank’s processing, it creates customer data on its website, and allows the customer to create a username and password to gain access to the account. This data is stored on the bank’s web server, which serves as the authenticator. The customer later presents their credentials through a browser to the web server, which authenticates under the authority of the bank.

When implementing the ‘principal authentication’ use case, there are different phases and strengths of check that may be appropriate. For example, when entering my local warehouse club, I need only show a card that looks like a membership card to the authenticator standing at the door. When it comes time to make a purchase, however, the membership card is checked for validity, expiration date and ownership of the person presenting it. In general, the authenticator is responsible for checking the authenticity of the credentials themselves (anti-forgery), whether they belong to their bearer, and whether they constitute valid access to the requested object(s).

## **Example Resolved**
The university created a credential authority, ‘IM Registration’. It gave it the responsibility of verifying identity and granting a username and password, in the form of an ID card, to university community members when they join the university community. This login embodies the authority of the granting agency and that of the identity of the subject as verified by the agency. The university defined policies such that members were encouraged to keep their login information private.

The client software is coded to implement an authenticator when someone wants to start a session. Access is granted or denied based on the results of the authentication. Checks on the servers ensure that the member’s credential is not expired.

## **Consequences**
The CREDENTIAL pattern offers the following benefits: 

- Fine-grained authentication and authorization information can be recorded in a uniform and persistent way. 
- A credential from a trusted authority can be considered proof of identity and of authorization. 
- It is possible to protect credentials using encryption or other means. 

The pattern has the following potential liabilities: 

- It might be difficult to find an authority that can be trusted. This can be resolved with chains (trees) of credentials, by which an authority certifies another authority. 
- Making credentials tamper-resistant incurs extra time and complexity. 
- Storing credentials outside of the systems that use them leaves system authentication and authorization mechanisms open to offline attack.

## **Known Uses**
This pattern is a generalization of the concepts embodied in X.509 Certificates, CORBA Security Service’s Credentials [2], Windows security tokens [3], SAML assertions [4] and the Credential Tokenizer pattern [5]. Capabilities, as used in operating systems, are another implementation of the idea. 

Passports are a non-technical example of the problem and its solution. Countries must be able to distinguish between their citizens, citizens of nations friendly and unfriendly to them, trading partners, guests, and unwanted people. There may be different rules for how long visitors may stay, and for what they may engage in while they are in the country. Computer systems share some of these traits: they must be able to distinguish between members and non-members of their user community; non-members may be eligible or ineligible to gain system access or participate in transactions.

## **See Also**
- Metadata-Based Access Control [6] describes a model in which credentials can be used to represent subjects. 
- The CREDENTIAL pattern complements SECURITY SESSION by giving an explicit definition of that pattern’s ‘session object’, as extracted from several existing platforms. 
- The AUTHENTICATOR pattern describes types of authenticators. 
- An Authorizer is a concrete version of the abstract concept of REFERENCE MONITOR. 
- Delegation of credentials is discussed in [7]. 
- [5] describes a Session Object pattern that ‘abstracts encapsulation of authentication and authorization credentials that can be passed across boundaries’. They seem to be talking about access rights rather than credentials: credentials abstract authentication and authorization rights. 
- The CIRCLE OF TRUST pattern allows the formation of trust relationships among service providers in order for their subjects to access an integrated and more secure environment. CREDENTIALs can be used for identification in a circle of trust.

## **References**

[1] Lopez, J., Oppliger, R., & Pernul, G. (2004). Authentication and authorization infrastructures (AAIs): a comparative survey. *Computers & Security*, *23*(7), 578-590.

[2] Anderson, R. (2001). CORBA Security Service Specification. OMG. from <http://www.omg.org/docs/formal/02-03-11.pdf>

[3] Brown, K. (2005). The .NET Developer’s Guide to Windows Security. Addison-Wesley.

[4] Hughes, J., & Maler, E. (2005). Security assertion markup language (saml) v2. 0 technical overview. *OASIS SSTC Working Draft sstc-saml-tech-overview-2.0-draft-08*, *13*.

[5] Steel, C., & Nagappan, R. (2006). *Core Security Patterns: Best Practices and Strategies for J2EE", Web Services, and Identity Management*. Pearson Education India.

[6] Priebe, T., Fernández, E. B., Mehlau, J. I., & Pernul, G. (2004). A pattern system for access control. In *Research Directions in Data and Applications Security XVIII* (pp. 235-249). Springer, Boston, MA.

[7] Weiss, M. (2006). Credential delegation: Towards grid security patterns. In *Proceedings of the Nordic Conference on Pattern Languages of Programs* (pp. 65-70).

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
