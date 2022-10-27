# **Transport Layer Security**

## **Intent**
The TRANSPORT LAYER SECURITY pattern describes how to provide a secure channel between a client and a server by which application messages are communicated over the transport layer of the Internet. The client and the server are mutually authenticated, and the integrity of their data is preserved.

## **Example**
A bank customer may want to check their account balance online. The bank uses the transport layer to transfer its confidential data. We need to protect this communication, as this confidential data is vulnerable to attack. The customer also has to ensure that the transactions are with the bank and not with an imposter, while the bank may need to verify that access is by a legitimate customer

## **Context**
Users using applications that exchange sensitive information, such as web browsers for e-commerce or similar activities. The transport layer in TCP/IP provides end-to-end communication services for applications within a layered architecture of network components and protocols, and specifically convenient services such as connection-oriented data stream support, flow control and multiplexing.

## **Problem**
The messages communicated between applications and servers on the transport layer are vulnerable to attack by intruders, who may try to read or modify them. Either the server or the client may be imposters. 

The solution to this problem must resolve the following forces: 

- Confidentiality and integrity. The data transferred in the transport layer between the client and the server could be intercepted and read or modified illegally. 
- Authenticity. Either the server or the client could be an imposter, which may allow security breaches. A ‘man-in-the-middle’ attack is also possible, in which an attacker poses both as the client to the server and as the server to the client. 
- Flexibility. Security protocol should be flexible and configurable, to be able to handle new attacks. 
- Transparency. The security measures of the protocol should be transparent to users. 
- Configurability. The protocol should allow users to select different degrees of security. 
- Overhead. The overhead should be minimal, or users will not want to use the protocol.

## **Solution**
Establish a cryptographic secure channel between the client and the server using algorithms that can be negotiated between the client and the server. Provide the means for client and server to authenticate each other. Provide a way to preserve the integrity of messages.

## **Structure**
Figure 158 shows a class diagram for the basic architecture of the TRANSPORT LAYER SECURITY pattern. A Client requests some Service from the Server. The TLSProtocol controller conveys this request using an Authenticator to mutually authenticate the Server and the Client and creates a Secure Channel between them. AUTHENTICATOR and Secure Channel are patterns.

![](./Images/transport_layer_security_structure.png)

*Figure 158: Class diagram for the TRANSPORT LAYER SECURITY pattern*

## **Dynamics**
We describe the dynamic aspects of the TRANSPORT LAYER SECURITY pattern using a sequence diagram for the following use case:

|Use Case:|Request a Service – Figure 159|
| :- | :- |
|Summary|A Client requests a service and the TLSProtocol authenticates the request and creates a secure channel.|
|Actors|Client, Server.|
|Precondition|The security parameters of the secure exchange have been predefined.|
|Description|<p>1. The Client makes a service request to the Server. </p><p>2. The TLSProtocol authenticates the Server to the Client and the Client to the Server. </p><p>3. The TLSProtocol creates a secure channel between the Server and the Client</p>|
|Alternate Flow|<p>- The authentication can fail. </p><p>- The creation of a secure channel can fail.</p>|
|Postcondition|The Server accepts the request and grants the service.|

![](./Images/transport_layer_security_dynamics.png)

*Figure 159: Sequence diagram for the use case ‘Request a service’*

## **Implementation**
One of the protocols that is dominant today for providing security at the transport layer is Secure Sockets Layer (SSL). The SSL protocol is a transport layer security protocol that was proposed and developed Netscape Communications in the 1990s. Transport Layer Security (TLS) is an IETF version of the SSL protocol, which has become a standard [1]. Much implementation advice can be found in [2].

The TLS protocol is partitioned into two main protocol layers, the TLS Record Protocol, and the TLS Handshake Protocol, executing above the TCP transport layer protocol, as shown in Figure 160 [3, 4]. There are other minor protocols at the handshake protocol layer, such as the Cipher Change Protocol, Alert Protocol and Application Protocol.

![](./Images/transport_layer_security_implementation_1.png)

*Figure 160: TLS layers*

- Record Protocol. The TLS Record Protocol provides encryption and message authentication for each message. A connection is created using symmetric cryptography data encryption. The keys for this SYMMETRIC ENCRYPTION are generated uniquely for each connection and are based on a secret negotiated by another protocol (such as the TLS Handshake Protocol). Messages include a message integrity check using a keyed message authentication code (MAC), computed using hash functions [5]. 
- Handshake Protocol. A TLS handshake supplies the authentication and key exchange operations for the TLS protocol. The security state agreed upon in the handshake is used by the TLS Record Protocol to provide session security. This protocol allows the server and client to authenticate each other and to negotiate an encryption algorithm and cryptographic keys before the application protocol transmits or receives any data. The TLS Handshake Protocol provides connection security where the peers’ identities can be authenticated using asymmetric cryptography. This authentication can be made optional but is generally required for at least one of the peers.

A TLS session is an association between a client and a server, created by the handshake protocol. Sessions define a set of cryptographic security parameters, which can be shared among multiple connections. Sessions are used to avoid the expensive negotiation of new security parameters for each connection. 

A session state is defined by the following parameters:

- Session identifier. This is generated by the server to identify a session with a chosen client. 
- Peer certificate. The X.509 certificate of the peer. 
- Compression method. A method used to compress data prior to encryption. 
- Algorithm specification or CipherSpec. Specifies the encryption algorithm that encrypts the data and the hash algorithm used during the session. 
- Master secret. 48-byte data, being a secret shared between the client and server. 
- ‘resumable’. A flag indicating whether the session can be used to initiate new connections. 

The handshake protocol consists of the following four phases:

1. In the first phase, an initial connection is established to start the negotiation. The client and server exchange ‘hello’ messages that are used to establish security parameters used in the TLS session, and settings used during the handshake, such as the key exchange algorithm. 
1. During the second phase, authentication, the server sends a certificate message to the client: this may include a server certificate when an RSA key exchange is used, or Diffie-Hellman parameters when a Diffie-Hellman key exchange is used. The server may also request a certificate from the client, using the certificateRequest message. 
1. During the third phase, the client, if asked, may send its certificate to the server in a certificate message, along with a certificate Verify message, so that the server can verify certificate ownership (if the server requested a client certificate during the second phase). This phase includes the establishment of the security parameters such as the encryption key. The client must send either a pre-master secret encrypted using the server’s public key, or public Diffie-Hellman parameters in the clientKeyExchange message, so that the client and server can compute a shared master secret. 
1. In the fourth phase, the client and server finish the handshake, which implies that the client and server are mutually authenticated and have completed the required key exchange operations.

The other minor protocol layers from Figure 160 are discussed below: 

- Cipher Change Protocol. This protocol signals transitions in cipherSpec, which is a session parameter explained above. 
- Alert Protocol. This protocol raises alerts for the communication. This record should normally not be sent during normal handshaking or application exchanges. However, this message can be sent at any time during the handshake and up to the closure of a TLS session. If this record is used to signal a fatal error, the session will be closed immediately after sending the record. If the alert level is flagged as a warning, the remote partner can decide whether or not to close the session. 
- Application Protocol. Now the handshake is completed, and the application protocol is enabled. This marks the start of data exchange between the server and the client.

### **Structure of the Handshake Protocol**
The structure of the handshake protocol is shown in the class diagram in Figure 161. The Client requests a service from the Server at the Transport layer. The TLSHandshakeProtocolController uses client and server certificates to mutually authenticate the Client and the Server, then performs the clientKeyExchange.

![](./Images/transport_layer_security_implementation_2.png)

*Figure 161: Class diagram for the TLS handshake protocol.*

### **Dynamics of the Handshake Protocol**
We describe the dynamic aspects of the TLS handshake using the sequence diagram shown in Figure 162.

|Summary|A TLS handshake supplies the authentication and key exchange operations for the TLS protocol.|
| :- | :- |
|Actors|Client, Server.|
|Precondition|The Client has made a request for a service from the host Server and an initial connection has already been established. The Client and Server need to have a digital certificate, issued by some Certificate Authority.|
|Description|<p>1. The Client and Server exchange initial Hello messages. </p><p>2. The ProtocolController requests the certificate from the Server and the Server sends the certificate. </p><p>3. The server certificate is verified. </p><p>4. The Server requests the certificate from the Client (optional). </p><p>5. If asked, the Client sends the certificate to the Server. </p><p>6. The client certificate is verified. </p><p>7. The Client sends the predefined secret encrypted using the Server’s public key which is the client key exchange. </p><p>8. The Client and the Server complete mutual handshake and the initial encryption parameters.</p>|
|Alternate Flow|<p>- Authentication of the Server or Client can fail. A certificate can be expired or outdated. </p><p>- The Client could lose the encryption key while exchanging with the Server</p>|
|Postcondition|Client and Server can start exchanging data at the transport layer.|

![](./Images/transport_layer_security_implementation_3.png)

*Figure 162: Sequence diagram for the TLS handshake use case*

## **Example Resolved**
When a request is made to the bank’s server by an online client at the transport layer, the bank’s server is authenticated to the customer, the customer is authenticated to the server and a secure channel is created between them. Now the client knows that online bank transactions are secure.

## **Consequences**
The TRANSPORT LAYER SECURITY pattern offers the following benefits: 

- Confidentiality and integrity. A secure channel is established between the server and the client, which can provide data confidentiality and integrity for the messages sent. We could add a logging system for the client at its endpoint for future audits. 
- Authenticity. Both client and server can be mutually authenticated. Man-in-the-middle attacks can be prevented by MUTUAL AUTHENTICATION. 
- Flexibility. We can easily change the algorithms for encryption and authentication protocols. 
- Transparency. The users don’t need to perform any operation to establish a secure channel. 
- Configurability. Users can select algorithms to obtain different degrees of security. 

The pattern also has the following potential liabilities: 

- Overhead. As seen from Figure 162, the overhead is significant for short sessions: many messages are needed. 
- SSL/TLS is a two-party protocol; it is not designed to handle multiple parties. However, the MTLS variant can handle multiple parties

## **Variants**
- WTLS. A modified version of TLS, called WTLS (Wireless TLS protocol) has been used in mobile systems. WTLS is based on TLS and is similar in some respects [6]. WTLS has been superseded in the WAP (Wireless Application Protocol) 2.0 standard by the End-toEnd Transport Layer Security specification. 
- MultipleTLS (MTLS). This is an application-level protocol running over the TLS Record protocol. The MTLS provides application multiplexing over a single TLS session. Therefore, instead of associating a TLS session with each application, this protocol allows several applications to protect their communication over a single TLS session [7]. Some different versions of TLS are given below. 
- TLS 1.0. TLS 1.0 is an upgrade of SSL Version 3.0 and is an IETF version of SSL. The differences between this protocol and SSL 3.0 are not large, but they are significant enough that TLS 1.0 and SSL 3.0 do not interoperate. TLS 1.0 does include a means by which a TLS implementation can downgrade the connection to SSL 3.0, but this weakens security. 
- TLS 1.1. TLS 1.1 is an update of TLS version 1.0. Significant differences include:
  - Added protection against cipher block chaining (CBC) attacks. In CBC mode, each block of plaintext is XORed with the previous cipher text block before being encrypted. 
  - The implicit Initialization Vector (IV) was replaced with an explicit IV. 
  - Change in handling of padding errors. 
  - Support for registration of parameters.
- TLS 1.2. This is a revision of the TLS 1.1 protocol, which contains improved flexibility, particularly for negotiation of cryptographic algorithms. The major changes are: 
  - The MD5/SHA-1 combination in the pseudorandom function (PRF) has been replaced with cipher-suite-specified PRFs. All cipher suites in this document use P\_SHA256. 
  - The MD5/SHA-1 combination in the digitally signed element has been replaced with a single hash. Signed elements now include a field that explicitly specifies the hash algorithm used. 
  - Substantial cleanup to the client’s and server’s ability to specify which hash and signature algorithms they will accept. 
  - Addition of support for authenticated encryption with additional data modes. 
  - Tightening up of a number of requirements. 
  - Verification of data length now depends on the cipher suite (default is still 12) [8].
- EAP-TLS is a wireless authentication protocol for TLS [9].

## **Known Uses**
- Mozilla Firefox versions 2 and above support TLS 1.0 [10]. 
- Internet Explorer (IE) 8 in Windows 7 and Windows Server 2008 support TLS 1.2 [11]. 
- Presto 2.2, used in Opera 10, supports TLS [12].

## **See Also**
- The AUTHENTICATOR pattern describes how to mutually authenticate a client and a server. 
- The SECURE CHANNEL pattern describes a cryptographic channel used to communicate secure data.

## **References**

[1] Yasinsac, A., & Childs, J. (2005). Formal analysis of modern security protocols. *Information Sciences*, *171*(1-3), 189-211.

[2] Seltzer, L. (2012). Best Practices and Applications of TLS/SSL. white paper. Symantec Corporation.

[3] Elgohary, A., Sobh, T. S., & Zaki, M. (2006). Design of an enhancement for SSL/TLS protocols. *computers & security*, *25*(4), 297-306.

[4] Stallings, W., Brown, L., Bauer, M. D., & Howard, M. (2012). *Computer security: principles and practice* (Vol. 2). Upper Saddle River: Pearson.

[5] Stallings, W. (2003). Cryptography and Network Security: Principles and Practice (3rd edition), Prentice-Hall

[6] Badra, M., Serhrouchni, A., & Urien, P. (2004). A lightweight identity authentication protocol for wireless networks. *Computer Communications*, *27*(17), 1738-1745.

[7] Badra, M., & Hajjeh, I. (2009, April). (D)TLS Multiplexing. Internet-Draft. <http://tools.ietf.org/html/draft-badra-hajjeh-mtls-05>

[8] Dierks, T., & Rescorla, E. (2008). The transport layer security (TLS) protocol version 1.2.

[9] Wikipedia contributors. (n.d.). Extensible Authentication Protocol. Wikipedia. Retrieved November 6, 2012, from <https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol> 

[10] Mozilla Newsgroup. (n.d.). mozilla.dev.tech.crypto. <http://www.mozilla.org/projects/security/pki/nss/ssl/>

[11] Microsoft. (n.d.). What is TLS/SSL. <http://technet.microsoftwarecom/en-us/library/cc784450(v=WS.10).aspx>

[12] Wikipedia. (2009). GNU Free Documentation License. <http://mjrutherford.org/files/2009-Spring-COMP-4704-TLS-Wikipedia.pdf\> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
