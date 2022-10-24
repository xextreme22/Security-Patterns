# **Morphed Representation**

## **Intent**
Change the representation of the data when it is passing through an anonymity providing node so that outgoing data cannot be linked with incoming data.

## **Example**
The unsuccessful mix. Alice, Bob, and Carol are using a mix-based system to communicate over the Internet. Alice sends her data through a node in the network where it gets mixed with the data coming from other sources (e.g., Bob, Carol, etc.). Figure 54 shows that Mallory is a passive observer of the mix network, and she wants to find out all the correspondences of Alice.

![](./Images/morphed_representation_example.png)

*Figure 54: The unsuccessful Mix network*

Alice encrypts her data with the public key of the recipient (in this case server M) to keep it confidential. The mix network receives Alice’s packet, waits for other packets to arrive, and then releases a bunch of packets together. However, the incoming and outgoing packets have the same data fields, hence it is easy for Mallory to find out who is sending which packet. Mallory can profile everyone’s messaging habit very easily.

## **Context**
You are designing a mix-based system to protect the privacy of the users. You want to have sender anonymity and sender and receiver unlinkability for the communicating parties. The mix-based system can be a mix-based filter in the Internet messaging domain that is used for email messaging or web browsing.

## **Problem**
Mix networks combine the data from a sender with the data coming from multiple other sources and send them together. The incoming data packets carry the data content. They also have meta characteristics associated like the time of packet generation, ingress order of packets etc. The mix network obfuscates these meta characteristics by adding random delays to the ingress packets, or by batching a number of ingress packets before releasing the packets. However, if the mix concepts do not obfuscate the data content, the incoming and outgoing data packets have the same representation, and they can be correlated trivially. This compromises sender anonymity as the packets can be linked to the packet generator if an adversary has enough capability to trace the packet path back to the sender. Also, the packets can be traced to the recipient and sender and receiver unlinkability is compromised.

How can the representation of the data be obfuscated?

This pattern addresses the following forces. 

- Packet Characteristics. The size and content of the data packet separates one packet from another. It is highly improbable that the size and content of two packets would be the same because timestamps are associated with packets. Even if the data inside the packet is protected by encryption, the encrypted content and size of packets reveal the correlation of the outgoing packets with the incoming packets. 
- Scalability. The Mix networks should be scalable. A PKI infrastructure should be established between the participating nodes (i.e., mix nodes and end nodes) such that SYMMETRIC ENCRYPTION keys can be exchanged during data transfer. 
- Confidentiality of Data. Data f ow in the network is encrypted to retain confidentiality of content. 
- Data Corruption. The mix nodes should not change the data content, only change the representation of the data content to achieve unlinkability. 
- Type of Adversary. A global, passive adversary monitors the data traffic in the network and does not manipulate the data content. Active adversaries may control the mix nodes. Mix nodes can also be controlled by passive (or semi-honest) adversaries that adhere to the mix protocol but only monitor the data content passing through the node. 
- Performance. The privacy retaining operations should not become a performance bottleneck. For anonymous messaging, the system should be usable in low latency usage scenarios, like web browsing.

## **Solution**
Change the representation of the incoming data packet such that the outgoing packets look different from incoming packets. The incoming packets are encrypted using a key shared between the sending node and the mix node. At the mix node, decrypt the packet and then re-encrypt it with the key shared between the mix node and the subsequent node.

Figure 55 shows the message transfer between Alice and Server N. Alice encrypts her packet with a shared key between her and the mix node. The mix node decrypts the packet and then re-encrypts it with the shared key between the mix and server M. Mallory is monitoring the network and she cannot identify the packets because the packet does not look the same.

Use symmetric keys for encryption to avoid the expensive primary key operations. The nodes set up a key share with all its neighbors during the setup phase.

![](./Images/morphed_representation_solution.png)

*Figure 55: Data packet morphing at a Mix node*

## **Structure**

## **Dynamics**

## **Implementation**
**Key Sharing.** The nodes in the network have to establish a symmetric key share with their neighbors. This symmetric key share can be established using public key certificates. However, the deployment of a global PKI infrastructure is an additional overhead for the scheme to be successful. To avoid the use of public key based key share establishment, lightweight secret sharing schemes like Diff e-Hellman key exchange [1] can be used.

**End-to-end encryption.** Since the packets are decrypted and re-encrypted in the mix nodes, confidentiality might be compromised if the data is in plaintext after the decryption. In that case even a semi-honest adversary running the mix node can compromise the privacy of sender and recipient and the confidentiality of the data. To avoid this, data has to be encrypted end-to-end. The sender encrypts the plaintext content with the public key of the ultimate recipient and then uses the symmetric key share to route it through the intermediaries.

**Size of Neighbor Set.** The scheme depends on all the nodes keeping a symmetric key share with their neighbors. A large list of neighbors (i.e., a large ANONYMITY SET) would ensure better anonymity because the node has many options to choose from for the next hop. However, a large list would add maintenance overhead of key share tables.

## **Example Resolved**

## **Consequences**
The pattern has the following benefits. 

- Privacy. The sender enjoys improved privacy because the representation of the data changes at every intermediate node. A single adversary can break the anonymity if he can observe the network globally which is fundamentally infeasible. The mechanism is also safe from colluding adversaries unless they are distributed globally and control the whole network. As long as there is one honest mix, it will obfuscate the correlation between input and output data traffic.

The pattern has the following liabilities. 

- Performance Overhead. Performance overhead comes from two things - overhead of creating symmetric key shares and overhead of cryptographic operations at each mix node. The key sharing is often done beforehand to avoid the overhead during data transfer. There are several trade-offs to consider determining the lifetime of the symmetric key share. If the symmetric keys are used for a long time, then the system becomes vulnerable to brute force attack on the key. If the keys have a short lifetime, then the key setup overhead would be considerably high. Public keys can be durable, but they would involve a high computational overhead.
- Denial of Service. The active adversaries controlling the mix can drop the packets and create a denial-of-service scenario. Without the presence of a network management component, it would be very difficult to find the misbehaving node.

## **Known Uses**
The Mix based networks [2] are based on the idea of mixing the incoming data traffic from one user with the data traffic coming from other users. To hide the correlation, the incoming data in the mix network is decrypted and then re-encrypted so that the egress traffic and the ingress traffic cannot be matched. Remailers based on the mix network principle, e.g., the Cypherpunks mailing list [3], Mixmaster [4], Babel [5], Mixminion [6] etc., follow this pattern to hide the correlation between incoming and outgoing packets.

Onion Routing [7, 8] systems are based on the concept of mixes, but they have better latency values than mix networks and are therefore more applicable in a web browsing scenario. Private web browsing systems for peer-to-peer communication that provide anonymity following this pattern include Morphmix [9], Tarzan [10] etc.

## **See Also**
MORPHED REPRESENTATION is used with ANONYMITY SET to hide the correlation between incoming and outgoing traffic. Sometimes the data traffic is encrypted with LAYERED ENCRYPTION so that MORPHED REPRESENTATION does not compromise data confidentiality.

## **References**

[1] Diffie, W., & Hellman, M. E. (1976). New Directions in Cryptography. *IEEE TRANSACTIONS ON INFORMATION THEORY*, *22*(6).

[2] Chaum, D. L. (1981). Untraceable electronic mail, return addresses, and digital pseudonyms. *Communications of the ACM*, *24*(2), 84-90.

[3] APAS Anonymous Remailer Use [FAQ 3/8]: Remailer Basics. (2001, December). Computer Cryptology. http://www.faqs.org/faqs/privacy/anon-server/faq/use/part3/

[4] Cotrell, L. (1995). Mixmaster & remailer attacks. <http://riot.eu.org/anon/doc/remailer-essay.html>.

[5] Gulcu, C., & Tsudik, G. (1996, February). Mixing E-mail with Babel. In *Proceedings of Internet Society Symposium on Network and Distributed Systems Security* (pp. 2-16). IEEE.

[6] Danezis, G., Dingledine, R., & Mathewson, N. (2003, May). Mixminion: Design of a type III anonymous remailer protocol. In *2003 Symposium on Security and Privacy, 2003.* (pp. 2-15). IEEE. 

[7] Goldschlag, D. M., Reed, M. G., & Syverson, P. F. (1996, May). Hiding routing information. In *International workshop on information hiding* (pp. 137-150). Springer, Berlin, Heidelberg.

[8] Syverson, P. F., Goldschlag, D. M., & Reed, M. G. (1997, May). Anonymous connections and onion routing. In *Proceedings. 1997 IEEE Symposium on Security and Privacy (Cat. No. 97CB36097)* (pp. 44-54). IEEE.

[9] Rennhard, M., & Plattner, B. (2002, November). Introducing morphmix: Peer-to-peer based anonymous internet usage with collusion detection. In *Proceedings of the 2002 ACM Workshop on Privacy in the Electronic Society* (pp. 91-102).

[10] Freedman, M. J., & Morris, R. (2002, November). Tarzan: A peer-to-peer anonymizing network layer. In *Proceedings of the 9th ACM Conference on Computer and Communications Security* (pp. 193-206).

## **Source**
Hafiz, M. (2006, October). A collection of privacy design patterns. In *Proceedings of the 2006 conference on Pattern languages of programs* (pp. 1-13).
