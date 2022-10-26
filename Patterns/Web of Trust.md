# **Web of Trust**

## **Intent**
Two parties can establish a secure communication channel by using public key cryptography based on certificates for authenticating each communication participant, by relying on the concept of trust.

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
- No trusted third party: the participants do not want to trust a single third party.

## **Solution**
Use public key cryptography based on certificates for authenticating each communication participant. After successful authentication, a session key is generated to secure the further communication.

Trust in a key is gained by manually verifying the fingerprint. Thus, no central CA is necessary to prove the individuals identity.

Each signature can be marked with a confidence level which indicates how the authenticity of the key owner was proved. The basic concept is illustrated in Figure 95a. As illustrated, Bob trusts John and Caro directly since he personally checked the identity of them (e.g., via a face-to-face meeting). Further, Bob indirectly trusts Alice since she is trusted by Caro. In difference to the CA structure John does not trust Alice because there is no direct path of trust leading to her. This is different in the CA case illustrated in Figure 95b. John will always trust Alice because they have the same root CA.

![](./Images/web_of_trust_solution.png)

*Figure 95: Differences of the CA structure compared to the WEB OF TRUST structure*

## **Structure**

## **Dynamics**

## **Implementation**

## **Example Resolved**

## **Consequences**
The benefits of the pattern are: 

- Through the use of certificates, Alice and Bob can assure that they are talking to each other and not to a Man-In-The-Middle. 
- No single entity is used to prove the identity of a specific person / system.

The liabilities are: 

- The usability is reduced since every communication party needs to prove the identity of the other parties. 
- The Certificates are based on public key cryptography which can be time-consuming when operated on small embedded systems.

## **Known Uses**
- Pretty Good Privacy (PGP). Trust is managed by the individual communication participants by manually verifying the fingerprints of received public keys / certificates. The user can decide if a key is fully trusted, partially trusted, or untrusted. These levels can be used to define the behavior of “indirect trust”. This means, that a key which is signed by “n” fully trusted parties or “m” partially trusted parties may be accepted as trusted automatically. 
- GNU Privacy Guard (GnuPG). A free and open-source implementation of PGP.

## **See Also**
The presented pattern makes use of the ASYMMETRIC ENCRYPTION pattern and the DIGITAL SIGNATURE WITH HASHING pattern to authenticate both parties. Alice encrypts the session key with Bob’s public key, adds a digital signature with her private key and sends it to Bob. Bob can then verify the digital signature with Alice’s public key and decrypt the session key with his private key. Note that the fingerprint of the public keys still needs to be verified, for example with a face-to-face meeting. Thus, a MITM attack can be prevented. 

The SYMMETRIC ENCRYPTION pattern can be used in case of symmetric session keys.

## **References**

## **Sources**
Sinnhofer, A. D., Oppermann, F. J., Potzmader, K., Orthacker, C., Steger, C., & Kreiner, C. (2016, July). Patterns to establish a secure communication channel. In Proceedings of the 21st European Conference on Pattern Languages of Programs (pp. 1-21).