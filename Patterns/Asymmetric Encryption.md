# **Asymmetric Encryption**

## **Intent**
ASYMMETRIC ENCRYPTION provides message confidentiality by keeping information secret in such a way that it can only be understood by intended recipients who have the access to the valid key. In ASYMMETRIC ENCRYPTION, a public/private key pair is used for encryption and decryption respectively.

## **Example**
Alice wants to send a personal message to Bob. They have not met each other to agree upon a shared key. Alice wants to keep the message secret, since it contains personal information. Eve can intercept Alice’s messages and may try to obtain the confidential information.

## **Context**
Applications that exchange sensitive information over insecure networks.

## **Problem**
Applications that communicate with external applications interchange messages that may contain sensitive information. These messages can be intercepted and read by imposters during transmission. How can we send sensitive information securely over insecure channels? 

The solution to this problem must resolve the following forces:

- Confidentiality. Messages may be captured while they are in transit, so we need to prevent unauthorized users from reading them by hiding the information the message contains. Hiding information also makes replaying of messages by an attacker harder to perform. 
- Reception. The hidden information should be revealed conveniently to the receiver. 
- Protocol. We need to apply the solution properly, or it will not be able to withstand attacks (there are several ways to attack a method of hiding information). 
- Performance. The time to hide and recover the message should be acceptable. 
- Key distribution. Two parties may want to communicate to each other, but they have not agreed on a shared key: we need a way to send messages without establishing a common key.

## **Solution**
Apply mathematical functions to a message to make it unreadable to those that do not have a valid key. 

This approach uses a key pair: private and public key. The sender encrypts (E) the message (M) using the receiver’s public key (PuK), which is accessible by anyone. The result of this process is cipher text (C):

*C = EPuk (M)*

On the other side, the receiver decrypts (D) the cipher text (C) using their private key (PrK) to recover the plain message (M):

*M = DPrk (C)*

## **Structure**
Figure 107 shows the class diagram for the ASYMMETRIC ENCRYPTION pattern. A Principal may be a user or an organization that is responsible for sending or receiving messages. The principal may have the roles of Sender or Receiver. A Sender may send a Message and/or an EncryptedMessage to a Receiver with which it shares a secret key.

![](./Images/asymmetric_encryption_structure.png)

*Figure 107: Class diagram for the ASYMMETRIC ENCRYPTION pattern*

A Principal has one or more KeyPairs that are composed of a private key, kept secret by its owner, and a public key. which is publicly published. PublicKeyRepository is a repository that contains a list of public keys where users can register and/or access public keys. These two keys are mathematically related, so while one encrypts, the other decrypts. However, it is not feasible to deduce a private key from its corresponding public key.

The Encryptor creates the EncryptedMessage that contain the cipher text using the public key of the Receiver provided by the sender, while the Decryptor deciphers the encrypted data into its original form using its private key. Both the Encryptor and Decryptor use the same Algorithm to encipher and decipher a message.

## **Dynamics**
We describe the dynamic aspects of the ASYMMETRIC ENCRYPTION pattern using sequence diagrams for the following use cases: ‘Encrypt a message’ and ‘Decrypt a message’.

|Use Case:|Encrypt a Message – Figure 108|
| :- | :- |
|Summary|A Sender wants to encrypt a message.|
|Actors|A Sender.|
|Precondition|The Sender has access to the Receiver’s public key. Both Sender and Receiver have access to a repository of algorithms. The message has already been created by the Sender.|
|Description|<p>1. A Sender sends the message, the Receiver’s public key, and the algorithm identifier to the Encryptor. </p><p>2. The Encryptor ciphers the message using the algorithm specified by the Sender. </p><p>3. The Encryptor creates the EncryptedMessage that includes the cipher text.</p>|
|Postcondition|The message has been encrypted and sent to the Sender.|

![](./Images/asymmetric_encryption_dynamics_1.png)

*Figure 108: Sequence diagram for the use case ‘Encrypt a message’*

|Use Case:|Decrypt an Encrypted Message – Figure 109|
| :- | :- |
|Summary|A Receiver wants to decrypt an encrypted message from a Sender.|
|Actors|A Receiver.|
|Precondition|Both the Sender and Receiver have access to a repository of algorithms.|
|Description|<p>1. A Receiver sends the encrypted message and their private key to the Decryptor. </p><p>2. The Decryptor deciphers the encrypted message using the Receiver’s public key. </p><p>3. The Decryptor creates the Message that contains the plain text obtained from the previous step. </p><p>4. The Decryptor sends the plain text Message to the receiver.</p>|
|Alternate Flow|- If the key used in step 2 is not mathematically related to the key used for encryption, the decryption process fails.|
|Postcondition|The encrypted message has been deciphered and delivered to the Receiver.|

![](./Images/asymmetric_encryption_dynamics_2.png)

*Figure 109: Sequence diagram for the use case ‘Decrypt an encrypted message’*

## **Implementation**
- Use the Strategy pattern [1] to select different encryption algorithms. 
- The designer should choose well-known algorithms such as RSA [2]. 
- Encryption can be implemented in different applications, such as in e-mail communication, distribution of documents over the Internet, or web services. In these applications we are able to encrypt an entire document. However, in web services we can encrypt parts of a message. 
- Both the sender and the receiver have to previously agree what cryptographic algorithms they support. 
- A good key pair generator is very important. It should generate key pairs for which the private key cannot be deduced from the public key.

## **Example Resolved**
Alice now can look up Bob’s public key and encrypt the message using this key. Since Bob keeps his private key secret, he is the only one who can decrypt Alice’s message. Eve cannot understand the encrypted data since Eve does not have access to Bob’s private key.

## **Consequences**
The asymmetric encryption pattern offers the following benefits: 

- Asymmetric encryption does not require a secret key to be shared among all the participants. Anyone can look up the public key in the repository and send messaged to the owner of the public key. 
- Only recipients that possess the corresponding private key can make the encrypted message readable again. 
- The strength of a cryptosystem is based on the secrecy of a long key [Sta06]. The cryptographic algorithms are known to the public, so the private key should be kept protected from unauthorized users. 
- It is possible to select from several encryption algorithms the one suitable for the application’s needs. 
- Encryption algorithms that take an acceptable time to encrypt messages exist. 

The pattern also has the following potential liabilities: 

- Cryptography operations are computationally intensive and may affect the performance of the application. Asymmetric encryption is slower than SYMMETRIC ENCRYPTION. It is best to use a combination of both algorithms: asymmetric encryption for key distribution, and SYMMETRIC ENCRYPTION for message exchange. 
- Encryption does not provide data integrity. The encrypted data can be modified by an attacker: other means, such as hashing, are needed to verify that a message has not been changed.
- Encryption does not prevent a replay attack, because an encrypted message can be captured and resent without being decrypted. It is recommended to use another security mechanism, such as timestamps or Nonces, to prevent this attack. 
- This pattern assumes that a public key belongs to the person who they claim to be. How can we know that this person is not impersonating another? To confirm that someone is who they say they are, we can use certificates issued by a certification authority (CA). If the CA is not trustworthy, we may lose security.

## **Known Uses**
ASYMMETRIC ENCRYPTION has been widely used in different products. 

- GNuPG [3] is free software that secures data from eavesdroppers.
- Java Cryptographic Extension [4] supports a variety of algorithms, including asymmetric encryption. 
- The .NET framework [5] provides several classes to perform asymmetric encryption and decryption. 
- XML Encryption [6] is one of the foundation web services security standards that defines the structure and process of encryption for XML messages. This standard supports both types of encryption: symmetric and asymmetric encryption. 
- Pretty Good Privacy (PGP) uses asymmetric encryption and decryption as one of its process to secure e-mail communication [7].

## **See Also**
- The SECURE CHANNELS pattern supports the encryption/decryption of data. This pattern describes encryption in more general terms: it does not distinguish between asymmetric and symmetric encryption. 
- The Strategy pattern [1] describes how to separate the implementation of related algorithms from the selection of one of them. This pattern can be used to select an encryption algorithm dynamically. 
- Predicate-based encryption is a family of public key encryption schemes; patterns for them are described in [8].

## **References**

[1] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[2] Rivest, R. L., Shamir, A., & Adleman, L. M. (2019). A method for obtaining digital signatures and public key cryptosystems. In *Secure communications and asymmetric cryptosystems* (pp. 217-239). Routledge.

[3] GnuPG. (n.d.). The GNU Privacy Guard. from <http://www.gnupg.org/>

[4] Sun Microsystems Inc. (n.d.). Java Cryptography Extension (JCE). From <http://java.sun.com/j2se/1.4.2/docs/guide/security/jce/JCERefGuide.html>

[5] Microsoft Corporation. (2007, November). .NET Framework Class Library. from <http://msdn.microsoftwarecom/enus/library/ms229335.aspx>

[6] W3C. (2002, December 10). XML Encryption Syntax and Processing. From <http://www.w3.org/TR/xmlenc-core/>

[7] Wikipedia contributors. (n.d.). Pretty Good Privacy. Wikipedia. from <https://en.wikipedia.org/wiki/Pretty_Good_Privacy> 

[8] de Muijnck-Hughes, J., & Duncan, I. (2012, June). Thinking towards a pattern language for predicate based encryption crypto-systems. In *2012 IEEE Sixth International Conference on Software Security and Reliability Companion* (pp. 27-32). IEEE.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
