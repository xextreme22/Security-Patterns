# **Abstract Virtual Private Network**

## **Intent**
The ABSTRACT VIRTUAL PRIVATE NETWORK pattern describes how to set up a secure channel between two endpoints using cryptographic tunneling, with authentication at each endpoint. An endpoint is an interface exposed by a communicating unit (user site or network).

## **Example**
Our company has employees all over the world. Because of cost, we decided to use the Internet to communicate. However, we are having problems because their orders are hacked, and the attackers get access to customers’ credit card numbers and other details. Our staff want to be sure they are talking to other employees and must be able to send secure messages to discuss prices, discounts and so on.

## **Context**
Users scattered in many fixed locations, who need to communicate securely with each other using the Internet or some other insecure network. In such a network attackers may intercept messages and try to read, modify, or replay them.

## **Problem**
In today’s world, companies have offices all over the world and a lot of people work remotely. They need a secure connection to other specific nodes so that confidential work can be performed securely. Their communication can be intercepted by attackers, who may get access to private information and may even modify the messages. How can we establish a secure channel for the end users of a network so that they can exchange messages through fixed points using an insecure network?

The solution to this problem must resolve the following forces: 

- We need to use the Internet or other insecure networks to reduce cost, but in turn subjecting our network to numerous threats. 
- Only registered users should access the institution’s endpoints. 
- We need to make sure that the users with which we are communicating are the right ones, otherwise confidentiality may be compromised. 
- The number of users remotely connected may be growing: the system should be scalable. 
- Because different users or institutions require different levels of security, the system should be flexible enough to accommodate different ways of providing security and different degrees of security.
- In some cases, we also need to support authorization to access specific resources in the endpoints. 
- The system should be easy to set up and use, otherwise users and administrators will not want to use it. 
- The system should not impose a heavy performance penalty, otherwise it will not be used all the time. 
- The pattern should be adaptable to the needs and constraints of different protocol layers.

## **Solution**
Protect communications by establishing a cryptographic tunnel between endpoints on one of the layers of the communication protocol. Add authentication functions at each endpoint. Figure 104 shows the case in which site A is talking to site B over the Internet using routers R1 and R2, respectively. The secure connection is established through one of the Internet layers.

![](./Images/abstract_virtual_private_network_solution.png)

*Figure 104: Two sites communicating through the Internet [For04b]*

## **Structure**
Figure 105 shows the class diagram for the ABSTRACT VIRTUAL PRIVATE NETWORK pattern, in which a SecureChannel can be established between a Client and a NetworkEndPoint. EndPoints communicate with other EndPoints. A user is authenticated by an AUTHENTICATOR pattern. AUTHENTICATOR and Secure Channel are patterns, composed typically of several classes, and are shown using the UML symbol for package.

![](./Images/abstract_virtual_private_network_structure.png)

*Figure 105: Class diagram for the ABSTRACT VIRTUAL PRIVATE NETWORK pattern*

## **Dynamics**
The sequence diagram of Figure 106 shows a use case in which an end user tries to access an endpoint in a network, endPoint2, from another endpoint, endPoint1. The Authenticator at endPoint1 authenticates the user. The Authenticator creates a Token as proof of authentication, which can be used to establish a SecureChannel. This channel allows secure access to endPoint2.

![](./Images/abstract_virtual_private_network_dynamics.png)

*Figure 106: Sequence diagram for accessing an endpoint*

## **Implementation**
First define the endpoints which the VPN will reach. Consider the architectural layer where the communications should be secured, according to the needs of the applications. 

After this decision, use the concrete VPN pattern at the corresponding level, IP, or TCP. See the corresponding patterns (IPSEC VPN, and TLS VPN) for help in making this decision [1]. Both endpoints must share a public key system for authentication and must have appropriate software packages running on them.

## **Example Resolved**
Now the users can be authenticated at the endpoints, which ensures that they are communicating with their own employees. User messages are now protected from external attacks when sent over the secure channel.

## **Consequences**
The ABSTRACT VIRTUAL PRIVATE NETWORK pattern offers the following benefits:

- We can use the Internet or other insecure networks to reduce cost. 
- Cryptography can protect our messages from being read or modified by attackers. 
- Authentication at endpoints ensures that only registered users can access the secure channel. 
- MUTUAL AUTHENTICATION between end users is possible. 
- The system can accommodate new links for new users by just replicating the access software. 
- We can use any cryptographic algorithm to establish the secure channel, which allows us to make trade-offs between security and cost. 
- We can add authorization to access specific resources at each endpoint. 
- We can add a logging system for the users logging in at the endpoints for use in audits. 
- The VPN is transparent to the users, who are authenticated by their local endpoints. 
- The VPN is a client-server architecture that is easy to configure. 
- We can have different versions of the pattern that can use the specific features of each protocol layer. 

The pattern also has the following potential liabilities: 

- If the VPN connection is compromised, the attacker could get full access to the internal network. Authorization can restrict this access, however. 
- Because of encryption, VPN traffic is invisible to IDS monitoring. If the IDS probe is outside the VPN server, as is often the case, then the IDS cannot see the traffic within the VPN tunnel. Therefore, if a hacker gains access to the end node of the VPN, they can attack the internal systems without being detected by the IDS. 
- In the case of VPN with a private end user, the remote computer used by the private user is vulnerable to outside attacks, which in turn can attack the network to which it is connected. 
- There is some overhead in the encryption process.

## **Variants**
Virtual private networks can be established at the application layer (XML or application VPN); TLS (SSL) VPNs are established at the transport layer. IPSec VPNs are established at the IP layer [1]. 

## **Known Uses**
Citrix provides a site-to-site SSL VPN connection for remote users to log into the secure network, as well as access applications on the company (secure) network [2]. 

Cisco VPN uses an IPSec VPN and provides authorization [3]. 

Nokia provides a VPN connection for Nokia mobile users.

## **See Also**
Firewalls can be added to each endpoint to filter inputs. They can protect against some types of attacks coming from untrusted sources. 

IDS patterns can be added in each of the network layers to detect attacks in real time. 

The VPN uses the Secure Channel pattern that in turn uses cryptography to protect its messages. 

The AUTHENTICATOR pattern can authenticate users and nodes. 

Access Control/AUTHORIZATION can be added at each site to control access to specific resources.

## **References**

[1] Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.

[2] Citrix. (n.d.). Explore Citrix Products for Enterprise and Medium Business. Retrieved June 21, 2010, from <http://www.citrix.com/English/ps2/products/product.asp?contentID=15005>.

[3] Cisco. (n.d.). Cisco Systems: Products and Technologies > Cisco Intrusion Detection. Retrieved from <http://www.cisco.com/warp/public/cc/pd/sqsw/sqidsz/>.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
