# **Layered Encryption**

## **Intent**
Use a sender-initiated packet routing scheme and encrypt the data packets in multiple layers so that the intermediaries only have access to a particular layer and use that information to route the packet to the next hop.

## **Example**
Ghost in the Machine. Alice, Bob, and Carol are using a mix-based system to communicate over the Internet. Alice sends her data through a node in the network where it gets mixed with the data coming from other sources (e.g., Bob, Carol, etc.). Figure 57 shows that Mallory is an active semi-honest adversary who controls the mix node, i.e., Mallory obeys the mix protocol to appear as an honest mix, but she tries to learn information by looking at the packets that are routed through the mix.

![](./Images/layered_encryption_example.png)

*Figure 57: An active attacker controlling a mix node*

The body of Alice’s packet is encrypted with a symmetric key between Alice and the recipient, so that the intermediaries cannot access the content. According to the mix protocol, when Alice’s packet is in transit between Alice and the mix node, it is encrypted with a shared symmetric key between her and the mix. Passive observers monitoring the link between Alice and the mix node cannot access the header of the packet. The mix network decrypts the packet, reads the header, finds the next hop, and routes the packet to the next hop after encrypting it with a shared key between the mix node and the next hop. Again, passive adversaries monitoring the egress packets of the mix node cannot access the packet header. Moreover, an adversary monitoring the ingress and egress links of the mix network cannot correlate the incoming and outgoing packets because of the mix protocol.

The problem arises because the mix node is controlled by an active adversary Mallory. Mallory shares the encryption key with Alice and the next hop, and accesses Alice’s packet header to determine the routing option. This compromises the sender anonymity of Alice. Also, from the header of Alice’s packet, Mallory can determine the ultimate recipient, and therefore can compromise sender and recipient unlinkability.

## **Context**
You are designing a mix-based system to protect the privacy of the users. You want to have sender anonymity and sender and receiver unlinkability for the communicating parties. The system can be a mix-based filter on the Internet that is used for email messaging or web browsing.

## **Problem**
In the mix protocol, the mix nodes share symmetric keys between themselves. The mix decrypts and then re-encrypts the packets flowing through the node. This protects against a passive adversary observing the network traffic but is insufficient against an active adversary controlling a mix node.

The mix node accesses the packet headers in order to identify the next hop. The header contains the ultimate destination, and the choice of next hop is determined by that. A malicious attacker controlling the mix node can follow the mix protocol, and yet prof le the behavior of a message sender, because of the header in plaintext available to him.

How can the mix network be made secure against an active adversary?

The forces that need to be considered when choosing to use this pattern are as follows. 

- Type of Adversary. Privacy can be compromised by different types of adversaries. A passive adversary only observes the network traffic but does not manipulate the data packets. An active adversary manipulates the data packets or compromises and controls a mix node. After controlling the mix network, an adversary can act semi-honestly, i.e., he continues to act like an honest node by following the mix protocol but at the same time tries to get information from the packets f owing through the mix node. Adversaries can also collude to undermine the anonymity of the message sender. 
- Routing Mechanism. The packet header contains information that is essential for the routing decision. A distributed routing mechanism would delegate this decision to the intermediary nodes. Contrarily, in a centralized routing mechanism, the sender determines the route that the packet will take and adds that information in the packet header. 
- Cost of Encryption. The cost of decryption and encryption can become an overhead. The mix network should be usable for a low latency messaging requirement like web browsing. 
- Key Establishment. The network follows protocols for dynamic negotiation and establishment of keys. A symmetric key share can be established by using a PKI scheme, but it assumes the presence of a global PKI framework. Key sharing schemes with low infrastructure requirements can be used, e.g., Diff e-Hellman key exchange [1]. 
- Application Independence. The mechanism for achieving sender anonymity should be application independent. It should be applicable for low latency messaging domain like web browsing, and latency independent messaging domain like email messaging.

## **Solution**
The sending client is responsible for establishing the path between the sender and the recipient. The neighboring nodes in the circuit share symmetric keys between themselves. The packet is then encrypted in multiple layers (like the onion skin). The innermost layer is encrypted with the symmetric key used in the last hop before the server, the next layer is encrypted with the symmetric key used in the preceding hop and so on.

Thus, the sending client has to construct a chain of nodes, and when the message is in transit through these nodes, each node strips off a layer using its key share, finds the identity of the next hop within the decrypted bundle, and forwards it to that node. For example, for a remailer Ei, with Ri as its public key, Ai as its address, and B as the destination address, a three-link route between Alice and Bob looks like

Alice –[E1(A2, E2(A3, E3(B, M)))]– > 

R1 –[E2(A3, E3(B, M)))]– > 

R2 –[E3(B, M)]– > 

R3 –[M]– > Bob

Each remailer is able to decrypt the bundle it receives, but it cannot itself look more than one link ahead, let alone determine the final destination. Moreover, after the first link, the sender’s identity has been removed. The first remailer R1 is connected with the sender but when it receives the message, it has no way to determine whether its previous node is the sender or just another mix node in the remailer chain.

## **Structure**

## **Dynamics**

## **Implementation**
The implementation issues described in the MORPHED RESPRESENTATION pattern also applies here. Additional issues are as follows.

**Service Composition.** LAYERED ENCRYPTION can be used in conjunction with other services. LAYERED ENCRYPTION can be used for the path between a request sender and the anonymizing proxy (e.g., Anonymizer, LPWA etc.) and the proxy then submits the request on the sender’s behalf.

**Layered Encryption Overhead.** The main overhead of LAYERED ENCRYPTION is the path setup cost. Typically, it is much less than one second, and it appears to be no more noticeable than other delays associated with normal web connection setup on the Internet. Computationally expensive public key operation is only used during the connection setup phase. By using dedicated hardware accelerators on the routers, the burden of public key operations can be relaxed.

## **Example Resolved**

## **Consequences**
The pattern has the following benefits. 

- Sender-determined Routing and Privacy. In a distributed routing protocol, the intermediaries determine the path of the packet on its route, but for this the intermediaries need to have access of sender and recipient information. Sender and recipient anonymity is achieved by using this pattern because here the routing decision is taken by the sender only. The sender initiates a path setup protocol to create the route. This can be done in offline (i.e., when the system is idle) to reduce performance overhead.
- Application Independence. LAYERED ENCRYPTION can be used with proxy-aware applications, as well as several non-proxy-aware applications. LAYERED ENCRYPTION supports various protocols, e.g., HTTP, FTP, SMTP, rlogin, telnet, finger, whois and raw sockets. Proxies can be used with NNTP, Socks 5, DNS, NFS, IRC, HTTPS, SSH and Virtual Private Networks (VPN).

The pattern has the following liabilities. 

- Data Integrity. The LAYERED ENCRYPTION technology does not perform integrity checking on the data. Any node in the path of LAYERED ENCRYPTION can change the content of data cells. However, if the adversary controlling a mix node alters the data content of the packet, the subsequent node will figure out the discrepancy. This mix node sends the information about the mal-formed data packet in the backward path, and eventually the sender finds out about the integrity violation. The sender can then initiate a new path and send the message along that path. 
- Path Setup Overhead. The sender has to create the complete route from the sender to the recipient and this setup cost is significant. The LAYERED ENCRYPTION systems balance this by creating the paths offline.

## **Known Uses**
Onion Routing [2, 3] systems are based on mixes, but they leverage the idea of mixes and add LAYERED ENCRYPTION. Onion Routing systems have better latency values than mix networks and therefore are more applicable in a web browsing scenario. Private web browsing systems for peer-to-peer communication like Morphmix [4], Tarzan [5] etc. follow LAYERED ENCRYPTION mechanism.

## **See Also**
LAYERED ENCRYPTION follows the MORPHED REPRESENTATION pattern by performing cryptographic operations at each nodes in the path.

See the ASYMMETRIC ENCRYPTION and SYMMETRIC ENCRYPTION patterns for types of encryption.

## **References**

[1] Diffie, W., & Hellman, M. (1976). New directions in cryptography. *IEEE transactions on Information Theory*, *22*(6), 644-654.

[2] Goldschlag, D. M., Reed, M. G., & Syverson, P. F. (1996, May). Hiding routing information. In *International workshop on information hiding* (pp. 137-150). Springer, Berlin, Heidelberg.

[3] Syverson, P. F., Goldschlag, D. M., & Reed, M. G. (1997, May). Anonymous connections and onion routing. In *Proceedings. 1997 IEEE Symposium on Security and Privacy (Cat. No. 97CB36097)* (pp. 44-54). IEEE.

[4] Rennhard, M., & Plattner, B. (2002, November). Introducing morphmix: Peer-to-peer based anonymous internet usage with collusion detection. In *Proceedings of the 2002 ACM Workshop on Privacy in the Electronic Society* (pp. 91-102).

[5] Freedman, M. J., & Morris, R. (2002, November). Tarzan: A peer-to-peer anonymizing network layer. In *Proceedings of the 9th ACM Conference on Computer and Communications Security* (pp. 193-206).

## **Source**
Hafiz, M. (2006, October). A collection of privacy design patterns. In *Proceedings of the 2006 conference on Pattern languages of programs* (pp. 1-13).