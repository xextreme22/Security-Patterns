# **Authenticator**

## **Intent**
When a subject identifies itself to the system, the AUTHENTICATOR pattern allows verification that the subject intending to access the system is who or what it claims to be.

## **Example**
The computer system at Melmac State University has legitimate users who use it to host their files. However, there is no way to ensure that a user who is logged in is a legitimate user. When Melmac was a small school and everybody knew everybody, this was acceptable and convenient. Now the school is bigger and there are many students, faculty members and employees who use the system. Some incidents have occurred in which users impersonated others and gained illegal access to their files.

## **Context**
Computer systems that contain resources that may be valuable because they include information about business plans, user medical records and so on. We only want subjects that have some reason to gain access to our system to be able to do so.

## **Problem**
How can we prevent imposters from accessing our system? A malicious attacker could try to impersonate a legitimate user to gain access to their resources. This could be particularly serious if the user impersonated has a high level of privilege. How do we verify that a user intending to access the system is legitimate? 

The solution to this problem must resolve the following forces:

- Flexibility. A variety of users require access to the system and a variety of system units exist with differently sensitive assets. We need to be able to handle all this variety appropriately, or we risk security exposures. 
- Dependability. We need to authenticate users in a reliable and secure way. This means using a robust protocol and a way to protect the results of authentication. Otherwise, users may skip authentication, or illegally modify its results, exposing the system to security violations. 
- Cost. There are trade-offs between security and cost: more secure systems are usually more expensive. 
- Performance. If authentication needs to be performed frequently, performance may become an issue. 
- Frequency. We should not make subjects authenticate frequently.

## **Solution**
Use a single point of access to receive the interactions of a subject with the system and apply a protocol to verify the identity of the subject. The protocol used may be simple or complex, depending on the needs of the application.

## **Structure**
Figure 110 shows the class diagram for this pattern. A Subject requests access to the system. The Authenticator receives this request and applies a protocol using some AuthenticationInformation. If the authentication is successful, the Authenticator creates a ProofOfIdentity, which is assigned to the subject to indicate that they are legitimate.

![](./Images/authenticator_structure.png)

*Figure 110: Class diagram for the AUTHENTICATOR pattern*

## **Dynamics**
Figure 111 shows the dynamics of the authentication process. A subject User requests access to the system through the Authenticator. The Authenticator applies some authentication protocol, for example by verifying some information presented by the subject, and as a result a ProofOfIdentity is created and assigned to the subject.

![](./Images/authenticator_dynamics.png)

*Figure 111: Sequence diagram for the use case ‘Authenticate subject’*

## **Implementation**
In centralized systems, the operating system controls the creation of a session in response to the request by a subject, typically a user. The authenticated user (represented by processes running on their behalf) is then allowed to access resources according to their rights. 

Some database systems have their own authentication systems, even when running on top of an operating system. There are many ways to implement authentication protocols, described in specific authentication patterns, for example in REMOTE ATTESTATION. Sensitive resource access, for example data in financial systems, requires more elaborate process authentication than systems that handle lower-value assets. Distributed systems may implement this function in specialized servers. 

## **Example Resolved**
Melmac State University implemented an authentication system and now only legitimate users can access their system. Illegal access to files has stopped.

## **Consequences**
The AUTHENTICATOR pattern offers the following benefits: 

- Flexibility. Depending on the protocol and the authentication information used, we can handle any types of users and we can authenticate them in diverse ways. 
- Dependability. Since the authentication information is separated, we can store it in a protected area, to which all subjects may have at most read-only access. 
- Cost. We can use a variety of algorithms and protocols of different strength for authentication. The selection depends on the security and cost trade-offs. Three varieties include something the user knows (passwords), something the user has (ID cards), or something the user is (biometrics). 
- Authentication can be performed in centralized or distributed environments. 
- Performance. We can produce a proof of identity to be used in lieu of further authentication. This improves performance. 
- Frequency. Using a token means that we do not need to authenticate users.

The pattern also has the following potential liabilities: 

- The authentication process takes some time: the overhead depends on the protocol used. 
- The general complexity and cost of the system increase with the level of security. 
- If the protocol is complex, users waste time and get annoyed.

## **Variants**
Single Sign On. Single Sign On (SSO) is a process whereby a subject verifies their identity, and the results of this verification can be used across several domains and for a given amount of time [1]. The result of the authentication is an authentication token, used to qualify all future accesses by the user

## **Known Uses**
- Commercial operating systems use some form of authentication, typically passwords, to authenticate their users. 
- RADIUS (Remote Authentication Dial-In User Service) provides a centralized authentication service for network and distributed systems [2, 3].
- The SSL authentication protocol uses a PKI arrangement for authentication. 
- SAML, a web services standard for security, defines one of its main uses as a way to implement authentication in web services. 
- E-commerce sites such as eBay and Amazon.

## **See Also**
- The Distributed Filtering and Access Control framework includes authentication [4]. 
- CREDENTIAL pattern provides a secure means of recording authentication and authorization information for use in distributed systems. 
- The SINGLE ACCESS POINT pattern.
- The CHECK POINT pattern.

## **References**

[1] Dalton, C., & King, C. (2001). *Security Architecture: Design, Deployment and Operations. osborne-McGraw-Hill*.

[2] Garfinkel, S., & Spafford, G. (2002). *Web security, privacy & commerce*. " O'Reilly Media, Inc.".

[3] Hassell, J. (2002). *RADIUS: securing public access to private resources*. " O'Reilly Media, Inc.".

[4] Hays, V., Loutrel, M., & Fernandez, E. B. (2000). The object filter and access control framework. In *Procs. Pattern Languages of Programs (PLoP2000) Conference*.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
