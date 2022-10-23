# **Password-Based Key Exchange**

## **Intent**
Two communication parties use a shared secret password that protects randomly generated session key pairs which are then used to establish a secure channel.

## **Example**
One example could be a server which is operated by multiple users and need to be accessible from anywhere at any time. This requires that the operators may access the server from their private devices (like computers or smart phones) or via shared public available infrastructure (like public access points or internet coffee shops).

## **Context**
A system wants to establish a secure channel with another party via an insecure channel. The connectivity of the device is limited such that it is not possible to securely load or store long private keys on the device (i.e., a user needs to manually enter it).

## **Problem**
A user needs to manually enter a password to start the communication. In a realistic scenario, such passwords are “short” and designed to be easy to remember. Using only these passwords would result in an insecure system since dictionary attacks / brute force attacks could be performed by a MITM to break the password. Further the MITM is able to resent previously eavesdropped packages (replay attack).

The following forces need to be considered: 

- Limited access: Symmetric key files, certificates or WEB OF TRUST are not suitable since it is not possible to securely load / store a private key file on the device. 
- User experience: The security controls should not negatively influence the functional behavior of the system. 
- Eavesdropping: Each sent communication package may be eavesdropped or replaced by a malicious third party

## **Solution**
Both communication parties (abbreviated as Alice and Bob) need to share a secret password. This password is used to protect randomly generated session key pairs which are used to further secure the communication.

## **Structure**


## **Dynamics**
Since Alice or Bob wants to transmit high confidential data using a weak (easy to remember) symmetric key they need to issue a password based key agreement process. Asymmetric cryptography has been proven in that context [1,2,3]. Thus, Alice and Bob need to agree on a shared secret which is incorporated into the key agreement protocol such that the used public key scheme is authenticated using this shared secret. The principal sequence of steps is displayed in Figure 6.

1. Before starting a secure communication, Bob is generating a new random key pair (Public Key: PB; Private Key: PprivB).
1. To start a communication, Bob sends a message to Alice containing the newly generated public key which is encrypted using the shared secret password. 
1. Alice receives the message and generates a new random key pair (Public Key: PA; Private Key: PprivA). The fact that Alice and Bob are both generating new key pairs is essential to increase the overall strength of the protocol. By doing so, it is getting more difficult to achieve offline brute-force attacks (see [2]).
1. Alice retrieves Bob’s public key PB by decrypting the received message with the shared secret. Alice encrypts her generated public key PA with the retrieved public key PB (abbreviated as C). Further, the resulting cryptogram is encrypted with the shared secret password before sent to Bob. 
1. Bob can retrieve PA by first decrypting the received message with the shared secret and by decrypting the result with his private key PprivB. After that, Alice and Bob are able to start a secure communication based on the generated key pairs. For large amount of data, it is advisable to start a session key agreement to establish a strong symmetric key (i.e., AES-256). 
1. The next steps illustrated in Figure 6 describes a basic challenge-response protocol to verify the validity of the generated cryptographic keys. This is done in most cryptographic protocols to ensure that Alice and Bob are able to decrypt messages which were encrypted with the according public key PA/PB. 
   1. Bob generates a random number R1 which he sends encrypted with the retrieved public key PA to Alice. 
   1. Alice retrieves the generated random number R1 by decrypting the received message. She generates a new random number R2 which she concatenates with R1. Alice uses the public key PB to encrypt the concatenated random numbers and sends the cryptogram to Bob. 
   1. Bob decrypts the received message and verifies that R1 is part of the received message. Further he retrieves the random number R2 and sends it encrypted with PA back to Alice. 
   1. Alice verifies the correctness of R2 
1. Both parties need to agree on a session key to encrypt all future messages. This can be achieved by using dedicated key derivation functions (e.g., by using a Diffie-Hellman key agreement where a symmetric session key is generated based on the used key-pairs [4]) or by randomly generating a new key. This generated key has to be sent encrypted to Alice / Bob using their according public key. Usually, symmetric session keys are chosen due to performance reasons.

The illustrated sequence used two times the shared secret password to encrypt a message (encryption of PB and C). In most cases, one of these encryption operations can or should be omitted. As stated in [2], the most obvious reason to skip one of the encryption operation is, that the message which shall be encrypted has to be indistinguishable from a random number. Otherwise, an attacker has knowledge about the principle structure of the plain text. Thus, an attacker could perform more sophisticated attacks against the shared secret password to break it.

The problem which remains is how to choose or how to establish the initial shared secret. This needs to be done using a different channel which can either be considered as secure, or the way the data is sent via this channel can be considered as being secure. For example, a password-based secret can be used which is sent to Bob in a secure way. This could be achieved via a key shipment split on multiple key shares using trusted couriers (see [5,6,7] for recommendations).

![](./Images/password-based_key_exchange_dynamics.png)

*Figure 6: Shared secret authenticated public key establishment.*

## **Implementation**


## **Example Resolved**


## **Consequences**
The benefits of the pattern are: 

- Using a “weak” shared secret produces a good channel protection against a Man-In-The-Middle attack since an attacker can only hack the system by brute-forcing the randomly generated public key (which should be of reasonable length). 
- I do not need a third party claiming the authenticity of a communication partner.

The liabilities are: 

- Establishing a shared secret without using certificate based public key cryptography is hard. 
- Both parties need to agree on another channel via which the shared secret is established which reduces the overall usability of this pattern.

## **Known Uses**
Use-Cases are mainly found in the domain of password-authenticated key agreements like:

- Encrypted Key Exchange protocols (e.g. [1,2,3]). These are based on the process illustrated in Figure 6 but provides slightly adaptations for different public key schemes (like RSA, ECC). The reason for this is to reduce the complexity of the overall process due to security properties of the different schemes. 
- Dragonfly (see [8]). This protocol uses EC based cryptography in combination with a pseudo-random key derivation function to diversify the shared secret at the beginning of each session.

## **See Also**
The presented pattern makes use of the ASYMMETRIC ENCRYPTION pattern in combination with a secret knowledge in form of a password. 

The shared knowledge is required to ensure that Alice and Bob are who they claim to be by proofing their knowledge about the shared secret. 

The ASYMMETRIC ENCRYPTION pattern is used to protect the messages sent between Alice and Bob to establish a secure channel. Further, it can be used for the session key agreement. 

The SYMMETRIC ENCRYPTION pattern can be used in case of symmetric session keys.

## **References**

[1] Bellovin, S. M., & Merritt, M. (1993). *U.S. Patent No. 5,241,599*. Washington, DC: U.S. Patent and Trademark Office.

[2] Bellovin, S. M., & Merritt, M. (1993, December). Augmented encrypted key exchange: A password-based protocol secure against dictionary attacks and password file compromise. In *Proceedings of the 1st ACM Conference on Computer and Communications Security* (pp. 244-250).

[3] Bellovin, S. M., & Merritt, M. J. (1995). *U.S. Patent No. 5,440,635*. Washington, DC: U.S. Patent and Trademark Office.

[4] Rescorla, E. (1999). Diffie-hellman key agreement method.

[5] NIST. (2016). *Special Publication 800-57-1:* *Recommendation for key management: Part 1: General*. Gaithersburg, MD, USA: National Institute of Standards and Technology, Technology Administration.

[6] Industry, P. C. (2016). Data Security Standard: Requirements and security assessment procedures. *Version 3.2*, *1*.

[7] International Organization for Standardization (ISO). (2006). ISO/IEC 11770-4: Information Technology - Security Techniques - Key Management - Part 4: Mechanisms Based On Weak Secrets.

[8] Harkins, D., & Zorn, G. (2010). *Extensible authentication protocol (EAP) authentication using only a password* (pp. 1-40). RFC 5931, August.

## **Sources**
Sinnhofer, A. D., Oppermann, F. J., Potzmader, K., Orthacker, C., Steger, C., & Kreiner, C. (2016, July). Patterns to establish a secure communication channel. In Proceedings of the 21st European Conference on Pattern Languages of Programs (pp. 1-21).
