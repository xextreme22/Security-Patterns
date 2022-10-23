# **Third Party-Based Authentication**

## **Intent**
Two parties can establish a secure communication channel by using public key cryptography based on certificates for authenticating each communication participant, by relying on a trusted third party who proves the authenticity of each participant.

## **Example**
One example could be that a company X is developing a server hosting a web-shop. If a customer wants to buy something from the shop, he needs to send his credit card information to the server. The customer is not interested in sending his credit card information in plain and hence a secure channel has to be established. Further, the customer is not interested in starting a symmetric key exchange process via a second secure channel (i.e., a key shipment using trusted couriers), because he wants to perform the operation now and not after some days when he finally received the symmetric key.

## **Context**
A system wants to establish a secure channel with another party via an insecure channel. Both communication parties are not limited in terms of computational power or resources (e.g., a personal computer / smart phone communicates with a server) but may not know each other in advance. A MITM can eavesdrop or change every sent message package. Further the MITM is able to resent previously eavesdropped packages (replay attack).

## **Problem**
Since both communication parties are not known to each other (i.e., no shared secret knowledge, nor a known public key) a mechanism is required such that trust is gained. No secret data shall be exposed to a third party listening or intercepting the communication. No previously recorded message package should trigger an unwanted action (e.g., a payment transaction).

The following forces need to be considered: 

- User experience: The security controls should not negatively influence the functional behavior (e.g., real-time capabilities) of the system. 
- Computational complexity: Public key cryptography is more time consuming compared with symmetric cryptography. 
- Eavesdropping: Each sent communication package may be eavesdropped or replaced by a malicious third party.

## **Solution**
Use public key cryptography based on certificates for authenticating each communication participant (abbreviated as Alice and Bob in the following). After successful authentication, a session key is generated to secure the further communication.

The solution of this problem is to have a trusted third party who proves the authenticity of each participant. This is usually called a Certificate Authority which proves the identity of the communication parties and signs their public keys. 

## **Structure**

## **Dynamics**
The principal sequence of steps for certificate based secure channel establishment is illustrated in Figure 4 and described in the following:

1. Before a communication is possible, Alice and Bob need to request a certificate issued by a trusted third party (CA). The CA proves the identity of Alice and Bob (e.g., by verifying the passport) and issues the certificate (i.e., signs the public key of Alice and Bob).
1. To start the communication, Bob sends a message to Alice including a random number for the authentication process. A time stamp should be incorporated to harden the system against replay attacks. 
1. Alice answers the request with her certificate. Bob extracts the public key of Alice and verifies the certificate by using the public key of the CA. 
1. If the verification was successful, Bob sends his certificate to Alice such that she can verify the properness of Bob’s certificate and to retrieve his public key. 
1. Until that point, no encryption is needed at all since the certificates are public anyway. In principle, these steps could have been performed by a malicious third party which has Bob’s certificate. Thus, Bob sends the hash of all previous messages which is signed with his private key so that Alice can prove that Bob has the corresponding private key of the sent certificate. This is important since a malicious third party – which only owns the certificate – cannot compute the signature of the hash. As a result, Alice will terminate the communication if she cannot verify the signature. 
1. Both parties need to agree on a session key to encrypt all future messages. This can be achieved by using dedicated key derivation functions (e.g., by using a Diffie-Hellman key agreement where a symmetric session key is generated based on the used key-pairs or by randomly generating a new key. This generated key has to be sent encrypted to Alice / Bob using their according public key. Usually, symmetric session keys are chosen due to performance reasons.

The presented solution covers the case that Bob is authenticated to Alice. For some applications this may not be sufficient. To ensure MUTUAL AUTHENTICATION, Bob has to send a challenge to Alice. In the simple case, this can be achieved by repeating step 5 where Alice is calculating the hash and Bob is verifying the signature of the message.

Each certificate has a specific date at which it gets automatically invalid. In some cases, it may happen that the owner of the certificate or the CA want to revoke the certificate earlier (i.e., the private key was broken / leaked, it was improperly issued, etc.). Therefore, so called revocation lists were introduced containing a list of invalid certificates. To ensure that such certificates are not used, each communication participant needs to consult the CA to retrieve this list. Thus, an online connection is necessary to regularly update these lists.

![](./Images/third_party-based_authentication_dynamics.png)

*Figure 4:  Certificate based authentication*

## **Implementation**

## **Example Resolved**

## **Consequences**
The benefits of the pattern are: 

- Through the use of certificates, Alice and Bob can assure that they are talking to each other and not to a Man-In-The-Middle. 
- There are open-source libraries supporting such authentication processes (e.g., OpenSSL)

The liabilities are: 

- The Certificates are based on public key cryptography which can be time-consuming when operated on small embedded systems. 
- If the Man-In-The-Middle is cooperating with the Certificate issuer, a Man-In-The-Middle attack cannot be detected / prevented, since he is able to manipulate the certificates of Alice and Bob. Such “hacks” cannot be prevented since the CA deals as a root of trust. 
- Requires an online connection or a frequent update of revocation lists to ensure that no blacklisted certificate is accepted as valid.

## **Known Uses**
- Secure Sockets Layer (SSL), Transport Layer Security (TLS) [1]. This system uses the process illustrated in Figure 4 using symmetric session keys.
- Secure key agreement algorithms like the extended Diffie-Hellman key exchange [2]. This process implements the presented pattern with a bi-directional challenge response protocol to ensure that Alice and Bob are the owner of the corresponding private keys.

## **See Also**
The ASYMMETRIC ENCRYPTION pattern is used multiple times in the presented pattern: The signature operation performed by Bob and / or Alice is an asymmetric encryption operation using the private key and a specific message. During the session key agreement asymmetric encryption can be used to send the session key. 

The SYMMETRIC ENCRYPTION pattern can be used in case of symmetric session keys.

This pattern uses the MUTUAL AUTHENTICATION pattern.

## **References**

[1] Das, M. L., & Samdaria, N. (2014). On the security of SSL/TLS-enabled applications. *Applied Computing and informatics*, *10*(1-2), 68-81.

[2] Yooni, E. J., & Yoo, K. Y. (2010, June). A new elliptic curve diffie-hellman two-party key agreement protocol. In *2010 7th International Conference on Service Systems and Service Management* (pp. 1-4). IEEE.

## **Sources**
Sinnhofer, A. D., Oppermann, F. J., Potzmader, K., Orthacker, C., Steger, C., & Kreiner, C. (2016, July). Patterns to establish a secure communication channel. In Proceedings of the 21st European Conference on Pattern Languages of Programs (pp. 1-21).
