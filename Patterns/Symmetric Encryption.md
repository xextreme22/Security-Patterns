# **Symmetric Encryption**

## **Intent**
Encryption protects message confidentiality by making a message unreadable to those that do not have access to the key. Symmetric encryption uses the same key for encryption and decryption.

## **Example**
Alice in the purchasing department regularly sends purchase orders to Bob in the distribution office. A purchase order contains sensitive data such as credit card numbers and other company information, so it is important to keep it secret. Eve can intercept her messages and may try to read them to get the confidential information. As part of her work Alice needs to communicate with only a few employees in the company.

![](./Images/symmetric_encryption_example.png)

## **Context**
Applications that exchange sensitive information over insecure channels and where the number of users and applications is not very large.

## **Problem**
Applications that communicate with external applications interchange sensitive data that may be read by unauthorized users while they are in transit. Clearly, if we send sensitive information, we are exposing confidential information and we may be risking the privacy of many individuals. How can we protect messages from being read by intruders? 

The solution to this problem must resolve the following forces: 

- Confidentiality. Messages may be captured while they are in transit, so we need to prevent unauthorized users from reading them by hiding the information in the message. 
- Convenient reception. The hidden information should be revealed conveniently to the receiver. 
- Protocol. We need to apply the solution properly, or it will not be able to withstand attacks (there are several ways to attack a method of hiding information). 
- Performance. The time to hide and recover the message should be acceptable. 
- Security. In some cases, we need to have a very high level of security.

## **Solution**
We can prevent unauthorized users from reading messages by hiding the information in the message using symmetric cryptographic encryption. Symmetric encryption transforms a message in such a way that it can only be understood by the intended receiver after applying the reverse transformation using a valid key. The transformation process at the sender’s end is called encryption, while the reverse transformation process at the receiver’s end is called decryption.

The sender applies an encryption function (E) to the message (M) using a key (k); the output is the cipher text (C):

*C = Ek (M)*

When the cipher text (C) is delivered, the receiver applies a decryption function (D) to the cipher text using the same key (k) and recovers the message:

*M = Dk (C)*

## **Structure**
Figure 170 shows the class diagram for the SYMMETRIC ENCRYPTION pattern. A Principal may be a user or an organization that is responsible for sending or receiving messages. This Principal may have the roles of Sender or Receiver. A Sender may send a Message and/or an EncryptedMessage to a Receiver with which it shares a secret Key.

![](./Images/symmetric_encryption_structure.png)

*Figure 170: Class diagram for SYMMETRIC ENCRYPTION pattern*

The Encryptor creates the EncryptedMessage that contain the cipher text using the shared Key provided by the sender, while the Decryptor deciphers the encrypted data into its original form using the same Key. Both the Encryptor and Decryptor use the same Algorithm to encipher and decipher a message.

## **Dynamics**
We describe the dynamic aspects of the SYMMETRIC ENCRYPTION pattern using sequence diagrams for the use cases ‘Encrypt a message’ and ‘Decrypt a message’.

|Use Case:|Encrypt a Message – Figure 171|
| :- | :- |
|Summary|A Sender wants to encrypt a message.|
|Actors|A Sender.|
|Precondition|Both Sender and Receiver have a shared key and access to a repository of algorithms. The message has already been created by the Sender.|
|Description|<p>1. A Sender sends the message, the shared key, and the algorithm identifier to the Encryptor. </p><p>2. The Encryptor ciphers the message using the algorithm specified by the Sender. </p><p>3. The Encryptor creates the EncryptedMessage that includes the cipher text.</p>|
|Postcondition|The message has been encrypted and is ready to send.|

![](./Images/symmetric_encryption_dynamics_1.png)

*Figure 171: Sequence diagram for the use case ‘Encrypt a message’*

|Use Case:|Decrypt an Encrypted Message – Figure 172|
| :- | :- |
|Summary|A Receiver wants to decrypt an encrypted message from a Sender.|
|Actors|A Receiver.|
|Precondition|Both the Sender and Receiver have a shared key and access to a repository of algorithms.|
|Description|<p>1. A Receiver sends the encrypted message and the shared key to the Decryptor. </p><p>2. The Decryptor deciphers the encrypted message using the shared key. </p><p>3. The Decryptor creates the Message that contains the plain text obtained from the previous step. </p><p>4. The Decryptor sends the plain text Message to the receiver.</p>|
|Alternate Flow|- If the key used in step 2 is not the same as the one used for encryption, the decryption process fails.|
|Postcondition|The encrypted message has been deciphered and delivered to the Receiver.|

![](./Images/symmetric_encryption_dynamics_2.png)

*Figure 172: Sequence diagram for the use case ‘Decrypt an encrypted message’*

## **Implementation**
- Use the Strategy pattern [1] to select different encryption algorithms. Selection could be based on speed, computational resources, key length, or memory constraints. The selection could happen when instantiating the pattern in an application, or dynamically according to environmental parameters. 
- The designer should choose well-known algorithms such as AES (Advanced Encryption Standard) [2] and DES (Data Encryption Standard) [3]. Books such as [4] describe their features and criteria for selection. 
- Encryption can be implemented in different applications, such as in e-mail communication, distribution of documents over the Internet, or web services. In these applications we may need to encrypt an entire document or just its body. However, in web services we may want to encrypt specific elements of a message. 
- Both the sender and the receiver have to previously agree what cryptographic algorithms they support, and they both must have the same key. This is the key distribution problem, which can be handled in several ways. 
- A key management strategy is needed, including key generator, storage, and distribution. This strategy should generate keys that are as random as possible, or an attacker who captures some messages might be able to deduce the key. The key should be properly protected, or an attacker who penetrates the operating system might be able to get it. Timely and secure key distribution is obviously very important. 
- A long encryption key should be used (at least 64 bits). Only brute force is known to work against the DES and AES algorithms, for example: using a short key would let an attacker generate all possible keys. Of course, this might change, and the repertoire of algorithms may need to be updated.

## **Example Resolved**
Alice now encrypts the purchase orders she sends to Bob. The purchase order’s sensitive data is now unreadable by Eve. Eve can try to apply to it all possible keys, but if the algorithm has been well-chosen and well implemented, Eve cannot read the confidential information. Since Alice only needs to communicate with a few people within the company, key distribution is rather easy.

![](./Images/symmetric_encryption_example_resolved.png)

## **Consequences**
The SYMMETRIC ENCRYPTION pattern offers the following benefits: 

- Only receivers who possess the shared key can decrypt a message, transforming it into a readable form. A captured message is unreadable to the attacker. This also makes attacks based on modifying a message very hard. 
- The strength of a cryptosystem is based on the secrecy of a long key [4]. The cryptographic algorithms are publicly known, so the key should be kept protected from unauthorized users. 
- It is possible to select from several encryption algorithms the one suitable for the application’s needs. 
- Encryption algorithms that take an acceptable time to encrypt messages exist. 

The pattern also has the following potential liabilities: 

- The pattern assumes that the shared key is distributed in a secure way. This may not be easy for large groups of nodes exchanging messages. Asymmetric cryptography can be used to solve this problem. 
- Cryptographic operations are computationally intensive and may affect the performance of the application. This is particularly important for mobile devices. 
- Encryption does not provide data integrity. The encrypted data can be modified by an attacker: other means, such as hashing, are needed to verify that the message was not changed. 
- Encryption does not prevent a replay attack, because an encrypted message can be captured and resent without being decrypted. It is better to use another security mechanism, such as time stamps or Nonces, to prevent this attack.

## **Known Uses**
SYMMETRIC ENCRYPTION has been widely used in different products. 

- GNuPG [5] is free software that secures data from eavesdroppers. 
- OpenSSL [6] is an open-source toolkit that encrypts and decrypts files. 
- Java Cryptographic Extension [7] provides a framework and implementations for encryption. 
- The .NET framework [8] provides several classes to perform encryption and decryption using symmetric algorithms. 
- XML Encryption [9] is one of the foundation web services security standards that defines the structure and process of encryption for XML messages. 
- Pretty Good Privacy (PGP), a set of programs used mostly for e-mail security, includes methods for symmetric encryption and decryption [10].
 
## **See Also**
- The Strategy pattern [1] describes how to separate the implementation of related algorithms from the selection of one of them. This pattern can be used to select an encryption algorithm dynamically. 
- ASYMMETRIC ENCRYPTION is commonly used to distribute keys. 

## **References**

[1] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[2] Federal Information Processing Standards Publication. (2001). Advanced Encryption Standard (AES). <http://csrc.nist.gov/publications/fips/fips197/fips-197.pdf>

[3] Federal Information Processing Standards Publication. (1999). Data Encryption Data (DES). [http://csrc.nist.gov/publications/fips/fips46- 3/fips46-3.pdf](http://csrc.nist.gov/publications/fips/fips46-%203/fips46-3.pdf)

[4] Stallings, W. (2006). Cryptography and Network Security (4th edition). Pearson Prentice Hall

[5] GnuPG. (n.d.). The GNU Privacy Guard. <http://www.gnupg.org/>

[6] OpenSSL. (n.d.). The OpenSSL Project. <http://www.openssl.org/>

[7] Sun Microsystems Inc. (n.d.). Java Cryptography Extension (JCE) <http://java.sun.com/j2se/1.4.2/docs/guide/security/jce/JCERefGuide.html>

[8] Microsoft Corporation. (2007, November). .NET Framework Class Library. <http://msdn.microsoftwarecom/enus/library/ms229335.aspx>

[9] W3C. (2002, December 10). XML Encryption Syntax and Processing. <http://www.w3.org/TR/xmlenc-core/>

[10] Wikipedia contributors. (n.d.). Pretty Good Privacy. Wikipedia. https://en.wikipedia.org/wiki/Pretty\_Good\_Privacy

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
