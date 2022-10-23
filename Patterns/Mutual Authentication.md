# **Mutual Authentication**

## **Intent**
In a grid environment, parties may need to prove to each other that they are who they say they are, before proceeding with their actual business. This is known as MUTUAL AUTHENTICATION and is accomplished by the parties exchanging their credentials.

## **Example**
A user at site A requests to a simulation service to run a simulation at site B. The simulation may access sensitive date on the user’s behalf. Before any simulation actions, the user and the simulation service must first establish two-way trust between each other, since typically, these two sites are in different security domains.

If we allowed the simulation service to serve any user anonymously, we run the risk that simulations could be executed by users with no authorization to access sensitive data at site B, either maliciously or by accident. Similarly, the user wants to first authenticate the simulation service before entrusting it with her simulation request.

## **Context**
A grid environment in which two parties need to establish mutual trust between each other. CREDENTIAL pattern is used as a precondition to identity the parties.

## **Problem**
Two parties must establish mutual trust between each other, before any further operations can happen. Refer to the two parties as Alice and Bob. Alice requests a service from Bob, assuming she is authorized by Bob to do so. The initial mutual trust must be established. Then Alice is sure that she is communicating with Bob who can provide the service she is requesting, and Bob ensures that it in fact is Alice attempting access and not a third party not authorized to the service.

One solution would be for Bob and Alice to authenticate each other with a username and password as an identity. But this would also require Alice and Bob to know each other’s passwords prior to authentication, and somehow keep a copy of the secret password. This copy leaves a security, because once an attacker obtains the passwords, they can steal the identities of Alice and Bob. Thus, the more entities with whom a party communicates, the less secure its identity. Another disadvantage of this solution is the overhead caused by managing copies of the passwords. Therefore, a precondition of this pattern is CREDENTIAL. More specifically, X.509 certificates from the public key infrastructure (PKI) can be used to implement the CREDENTIAL pattern.

To solve this problem, the following forces must be balanced: 

- The solution should be easy to manage, but to gain this ease of use we cannot sacrifice protection against identity thieves and eavesdroppers. 
- The identity of each entity should be persistent in all sessions initiated by it.

## **Solution**
Each entity (verifier) verifies the other entity's (verified) identity by validating the encryption of the verified with her public certificate. Based on public key technology, an entity holds a pair of public/private keys. The public key is contained in the entity’s certificate and the private key is only accessible to the entity. If a message is encrypted with a private key, only the corresponding public key can decrypt it.

The parties share a randomly generated message. At either side, the verified encrypts the message with its private key, and the verifier decrypts the encryption using the public key in the certificate of the verified. If the decrypted message matches the random message on hand, the verifier can authenticate the verified. The certificate signed by a certificate authority (CA) represents who the verified is. The CA is the trust anchor of the verifier. As long as the verifier trusts the CA, it would trust the identity of the verified represented by her certificate in the authentication.

## **Structure**
Figure 5 below shows the structure of this pattern. Alice and Bob want to mutually authenticate each other. Alice holds a certificate Cert A which identifies her. Cert A is signed by certificate authority CA A. Bob also holds a certificate Cert B as his identity signed by certificate authority CA B. Alice verifies Cert B to be able to trust Bob, based on her trust to CA B. Bob verifies Cert A to be able to trust Alice, also based on his trust to CA A. MUTUAL AUTHENTICATION is then established after the two-way trust.

![](./Images/mutual_authentication_structure.png)

*Figure 5: Structure of the MUTUAL AUTHENTICATION pattern*

## **Dynamics**


## **Implementation**


## **Example Resolved**
The user has a X.509 certificated issued and signed by a CA. So does the simulation. The user and the simulation can both verify each other’s certificate and mutual authenticate as described above. Through the authentication, the simulation ensures the user’s identity and can further check if she is authorized to access the data. Meanwhile, the user is also sure the simulation service is the one she is requesting.

## **Consequences**
The following benefits may be expected from applying this pattern: 

- The implementation of public key cryptography makes the authentication reliable against adversaries. 
- X.509 certificates are widely used in many organizations. The existing public key frameworks such as PKI have taken good care of the certificate issuing and management work. 
- X.509 certificates are distinct based on their Subject Name and/or serial number, and no two certificate holders can be authenticated as the same entity, so the certificates are persistent and uniform in all sessions. Therefore, authentication and authorization policies can be defined on them for granting access. 
- No one can forge the certificates since the CA private key for signing them is only kept secret for the CA.

The following liabilities may arise from applying this pattern: 

- It is significantly more expensive to implement than a lightweight mechanism based on passwords. 
- To authenticate an entity, the user must trust the CA that signs its certificate. This “cold start” problem can be solved by using a CA hierarchy. A root CA signs the certificates of all the involved CAs, so parties now only need to both trust the root CA. There can also be several trusted root CAs. 
- MUTUAL AUTHENTICATION is slower compared to one-side only authentication and anonymity.

## **Known Uses**
MUTUAL AUTHENTICATION appears before grid computing emerges, so it is not limited to grids. It is used on client/server applications where clients hold certificates as identities. In Large Hadron Collider (LHC) Computing Grid (LCG) project [1], the authentication layer is based on Globus Grid Security Infrastructure (GSI). Each grid user (and grid service) is in possession of an X509 format certificate. Certificates are issued by a network of Certification Authorities (CA) approved by the project. LCG enables services to operate on behalf of the user (such as for batch job execution when the user is not present, or for provisioning of data from one storage location to another), and MUTUAL AUTHENTICATION is required between the services and the user before delegation relationships are established [2].

## **See Also**
MUTUAL AUTHENTICATION refines Known Partner pattern. CREDENTIAL pattern is used to identify the parties who want to mutual authenticate each other.

## **References**
[1] LCG. (n.d.). homepage. LHC Computing Grid Project. <https://wlcg.web.cern.ch/LCG/> 

[2] Neilson, I. & LCG Deployment Group. (2004). CERN. Jisc. <http://www.jisc.ac.uk/uploaded_documents/CERN_%20PositionPaper.doc> 

## **Source**

Lu, M., & Weiss, M. (2008). Patterns for grid security. In *Mini-PLoP at conference on object-oriented programming systems, languages, and applications, OOPSLA*.
