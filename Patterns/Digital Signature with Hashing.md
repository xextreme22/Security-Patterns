# **Digital Signature with Hashing**

## **Intent**
The DIGITAL SIGNATURE WITH HASHING pattern allows a principal to prove that a message was originated from it. It also provides message integrity, by indicating whether a message was altered during transmission.

## **Example**
Alice in the sales department wants to send a product order to Bob in the production department. The product order does not contain sensitive data such as credit card numbers, so it is not important to keep it secret. However, Bob wants to be certain that the message was created by Alice, so he can charge the order to her account. Also, because this order includes the quantity of items to be produced, an unauthorized modification to the order will make Bob manufacture the wrong quantity of items. Eve is a disgruntled employee who can intercept the messages and may want to attempt this kind of modification to hurt the company.

## **Context**
People or systems often need to exchange documents or messages through insecure networks and need to prove their origin and integrity. Stored legal documents need to be kept without modification and with indication of their origin. Software sent by a vendor through the Internet is required to prove its origin. 

We assume that those exchanging documents have access to a public key system where a principal possesses a key pair: a private key that is secretly kept by the principal, and a public key that is in a publicly accessible repository. We assume that there is a mechanism for the generation of these key pairs and for the distribution of public keys; that is, a public key infrastructure (PKI).

## **Problem**
In many applications we need to verify the origin of a message (message authentication). Since an imposter may assume the identity of a principal, how can we verify that a message came from a particular principal? Also, messages that travel through insecure channels can be captured and modified by attackers. How can we know that the message or document that we are receiving has not been modified? 

The solution to this problem must resolve the following forces:

- For legal or business reasons we need to be able to verify who sent a particular message. Otherwise, we may not be sure of its origin, and the sender may deny having sent it (repudiation). 
- Messages may be altered during transmission, so we need to verify that the data is in its original form when it reaches its destination. 
- The length of the signed message should not be significantly larger than the original message, otherwise we would waste time and bandwidth. 
- Producing a signed message should not require large computational power or take a long time

## **Solution**
Apply properties of public key cryptographic algorithms to messages in order to create a signature that will be unique for each sender [1]. The message is first compressed (hashed) to a smaller size (digest), then encrypted using the sender’s private key. When the signed message arrives at its target, the receiver verifies the signature using the sender’s public key to decrypt the message. If it produces a readable message, it could only have been sent by this sender. The receiver then generates the hashed digest of the received message and compares it with the received hashed digest: if it matches, the message has not been altered.

This approach uses public key cryptography in which one key is used for encryption and the other for decryption. To produce a digital signature (SIG), we encrypt (E) the hash value of a message (H(M)) using the sender’s private key (PrK):

SIG = EPrK (H(M))

We recover the hash value of the message (H(M)) by applying decryption function D to the signature (SIG) using the sender’s public key (PuK). If this produces a legible message, we can be confident that the sender created the message, because they are the only one who has access to their private key. Finally, we calculate the hash value of the message as:

H(M) = DPuK (SIG)

If this value is the same as the message digest obtained when the signature was decrypted, then we know that the message has not been modified.

It is clear that the sender and receiver should agree to use the same encryption and hashing algorithms.

## **Structure**
Figure 120 shows the class diagram for the DIGITAL SIGNATURE WITH HASHING pattern. A Principal may be a process, a user or an organization that is responsible for sending or receiving messages. This Principal may have the roles of Sender or Receiver. A Sender may send a plain Message and/or a SignedMessage to a receiver.

![](./Images/digital_signature_with_hashing_structure.png)

*Figure 120: shows the class diagram for the DIGITAL SIGNATURE WITH HASHING pattern.*

The KeyPair entity contains two keys, public and private, that belong to a Principal. The public key is registered and accessed through a repository, while the private key is kept secret by its owner. PublicKeyRepository is a repository that contains public keys. The PublicKeyRepository may be located in the same local network as the Principal, or on an external network.

The Signer creates the SignedMessage that includes the Signature for a specific message. On the other side, the Verifier checks that the Signature within the SignedMessage corresponds to that message. The Signer and Verifier use the DigestAlgorithm and SignatureAlgorithm to create and verify a signature respectively. The DigestAlgorithm is a hash function that condenses a message to a fixed length called a hash value, or message digest. The SignatureAlgorithm encrypts and decrypts messages using public/private key pairs.

## **Dynamics**
We describe the dynamic aspects of the DIGITAL SIGNATURE WITH HASHING pattern using sequence diagrams for the use cases ‘Sign a message’ and ‘Verify a signature’.

|Use Case:|Sign a Message – Figure 121|
| :- | :- |
|Summary|A Sender wants to sign a message before sending it.|
|Actors|A Sender.|
|Precondition|A Sender has a public/private pair key.|
|Description|<p>1. A Sender sends the message and its private key to the Signer. </p><p>2. The Signer calculates the hash value of the message (digest) and returns it to the Sender. </p><p>3. The Signer encrypts the hash value using the Sender’s private key with the SignatureAlgorithm. The output of this calculation is the digital signature value. </p><p>4. The Signer creates the Signature object that contains the digital signature value. </p><p>5. The Signer creates the SignedMessage that contains the original message and the Signature.</p>|
|Postcondition|A SignedMessage object has been created.|

![](./Images/digital_signature_with_hashing_dynamics_1.png)

*Figure 121: Sequence diagram for the use case ‘Sign a message’*

|Use Case:|Verify a Signature – Figure 122|
| :- | :- |
|Summary|A Receiver wants to verify that the signature corresponds to the received message.|
|Actors|A Receiver.|
|Precondition|None.|
|Description|<p>1. A Receiver retrieves the Sender’s public key from the PublicKeyRepository. </p><p>2. A Receiver sends the signed message and the Sender’s public key to the Verifier. </p><p>3. The Verifier decrypts the signature using the Sender’s public key with the SignatureAlgorithm. </p><p>4. The Verifier calculates the digest value of the message. </p><p>5. The Verifier compares the outputs from step 3 and 4. </p><p>6. The Verifier sends an acknowledgement to the Receiver that the signature is valid.</p>|
|Alternate Flows|- The outputs from steps 3 and 4 are not the same. In this case, the verifier sends an acknowledgement to the receiver that the signature failed.|
|Postcondition|The signature has been verified.|

![](./Images/digital_signature_with_hashing_dynamics_2.png)

*Figure 122: Sequence diagram for the use case ‘Verify a signature’*

## **Implementation**
- Use the Strategy pattern [2] to select different hashing and signature algorithms. The most widely used hashing algorithms are MD5 and SHA1. These and others are discussed in [1]. 
- A good hashing algorithm produces digests that are very unlikely to be produced by other meaningful messages, meaning that it is very hard for an attacker to create an altered message with the same hash value. The message digest should be encrypted after being signed to avoid man-in-the-middle attacks, where someone who captures a message could reconstruct its hash value. 
- Two popular digital signature algorithms are RSA [3,4] and Digital Signature Algorithm (DSA) [1,5]. 
- The designer should choose strong and proven algorithms to prevent attackers from breaking them. The cryptographic protocol aspects, for example key generation, are as important as the algorithms used. 
- The sender and receiver should have a way of agreeing on the hash and encryption algorithms used for a specific set of messages. (XML documents indicate which algorithms they use, and pre-agreements are not necessary in this case.) 
- Access to the sender’s public key should be available from a public directory or from certificates presented by the signer. 
- Digital signatures can be implemented in different applications, such as in e-mail communication, distribution of documents over the Internet, or web services. For example, it is possible to sign an email’s contents, or any other document’s content, such as a PDF. In both cases, the signature is appended to the e-mail or document. When digital signatures are applied in web services, they are also embedded within XML messages. However, these signatures are treated as XML elements, and they have additional features, such as signing parts of a message, or external resources, which can be XML or any other data type. 
- When certificates are used to provide the sender’s public key, there must be a convenient way to verify that the certificate is still valid [6]. 
- There should be a way of authenticating the signer software [7], as an attacker who gains control of a user’s computer could replace the signing software with their own software.

## **Example Resolved**
Alice and Bob agree on the use of a digital signature algorithm, and Bob has access to Alice’s public key. Alice can then send a signed message to Bob. When the message is received by Bob, he verifies that the signature is valid using Alice’s public key and the agreed signature algorithm. If the signature is valid, Bob can be confident that the message was created by Alice. If the hash value is correct, Bob also knows that Eve has not been able to modify the message.

## **Consequences**
The DIGITAL SIGNATURE WITH HASHING pattern offers the following benefits: 

- Because a principal’s private key is used to sign the message, the signature can be validated using its public key, which proves that the sender created and sent the message. 
- When a signature is validated using a principal’s public key, the sender cannot deny that they created and sent the message (nonrepudiation). If a message is signed using another private key that does not belong to the sender, the validity of the signature fails. 
- If the proper precautions are followed, any change in the original message will produce a digest value that will be different (with a very high probability) from the value obtained after decrypting the signature using the sender’s public key. 
- A message is compressed into a fixed length string using the hash algorithm before it is signed. As a result, the process of signing is faster, and the signed message is much shorter. 
- The available algorithms that can be used for digital signatures do not require very large amounts of computational power and do not take large amounts of time. 

The pattern also has the following potential liabilities: 

- We need a well-established public key infrastructure that can provide reliable public keys. Certificates issued by a certification authority are the most common way to obtain this [1]. 
- Both the sender and the receiver have to previously agree what signature and hashing algorithms they support. (This is not necessary in XML documents, because they are self-describing.) 
- Cryptographic algorithms create some overhead (time, memory, computational power), which can be reduced but not eliminated. 
- The required storage and computational power may not be available, for example in mobile devices. 
- Users must implement the signature protocol properly. 
- There may be attacks against specific algorithms or implementations [7]. These are difficult to use against careful implementations of this pattern. 
- This solution only allows one signer for the whole message. A variant or specialization, such as the XML SIGNATURE pattern [8], allows multiple signers. 
- Digital signatures do not provide message authentication, and replay attacks are possible [6]. Nonces or time stamps could prevent this type of attack.

## **Known Uses**
Digital signatures have been widely used in different products. 

- Adobe Reader and Acrobat [9] have an extended security feature that allows users to digitally sign PDF documents. 
- CoSign [10] digitally signs different types of documents, files, forms, and other electronic transactions. 
- GNuPG [11] digitally signs e-mail messages. 
- The Java Cryptographic Architecture [12] includes APIs for digital signature. 
- Microsoft .NET [13] includes APIs for asymmetric cryptography such as digital signature. 
- XML Signature [14] is one of the foundation web services security standards that defines the structure and process of digital signatures in XML messages.

## **See Also**
- ASYMMETRIC ENCRYPTION pattern. 
- Generation and distribution of public keys [15]. 
- Certificates are issued by a certificate authority (CA) that digitally signs them using its private key. A certificate carries a user’s public key and allows anyone who has access to the CA’s public key to verify that the certificate was signed by the CA. 
- The Strategy pattern [2] describes how to separate the implementation of related algorithms from the selection of one of them.

## **References**

[1] Stallings, W. (2006). Cryptography and network security principles and practices 4th edition.

[2] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[3] Rivest, R. L., Shamir, A., & Adleman, L. M. (2019). A method for obtaining digital signatures and public key cryptosystems. In *Secure communications and asymmetric cryptosystems* (pp. 217-239). Routledge.

[4] RSA. (n.d.). RSA Security, PKCS #1: RSA Cryptography Standard. from <http://www.rsa.com/rsalabs/node.asp?id=2125>

[5] NIST. (2000, January 27). Federal Information Processing Standard, Digital Signature Standard. from <http://csrc.nist.gov/publications/fips/fips186-2/fips186-2-change1.pdf> 

[6] W3C. (2001, February). SOAP Security Extensions: Digital Signature. W3C NOTE 06. <http://www.w3.org/TR/SOAP-dsig/>

[7] Wikipedia contributors. (n.d.). Digital signature. Wikipedia. <https://en.wikipedia.org/wiki/Digital_signature> 

[8] Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.

[9] Adobe. (n.d.). About certificate signatures in Adobe Acrobat. https://helpx.adobe.com/acrobat/kb/certificate-signatures.html

[10] Arx. (n.d.). Digital Signature Solution (Standard Electronic Signatures). <http://www.arx.com/products/cosign-digital-signatures.php> 

[11] The GNU Privacy Guard. (n.d.). GnuPG. https://www.gnupg.org/

[12] Sun Microsystems Inc. (n.d.). Java SE Security. <http://java.sun.com/javase/technologies/security/>

[13] Microsoft Corporation. (2007, November). .NET Framework Class Library. <http://msdn.microsoftwarecom/enus/library/ms229335.aspx>

[14] W3C Working Group. (2008). XML Signature Syntax and Processing (2nd edition). <http://www.w3.org/TR/xmldsig-core>

[15] Lehtonen, S., & Pärssinen, J. (2002, June). Pattern Language for Cryptographic Key Management. In *EuroPLoP* (pp. 245-258).

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
