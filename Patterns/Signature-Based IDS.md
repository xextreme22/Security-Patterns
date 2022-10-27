# **Signature-Based IDS**

## **Intent**
The SIGNATURE-BASED IDS pattern describes how to check every request for access to the network against a set of existing attack signatures, to detect possible attacks and trigger an appropriate response.

## **Example**
Our company has a firewall to control traffic from the Internet. However, we are still plagued by viruses and other attacks that penetrate the firewall. We need to improve our defense against such attacks.

## **Context**
Distributed systems executing applications that may provide services to remote nodes. Access to the network can be from the Internet or from other external networks.

## **Problem**
Whenever data is accessed from the distrusted networks, there is always a possibility that this access can be harmful to the local node. We need to detect possible attacks while they are occurring. Security techniques such as authentication and firewalls are usually implemented to provide security, but we need additional defenses to detect whether an access request is a possible attack or not. 

The solution to this problem must resolve the following forces:

- Known attacks. It is easier to protect the system against known attacks. Many attacks are new instances of known attacks and have a well-defined attack signature. 
- Completeness. If we have a complete collection of known attacks and their signatures, it is easier to detect an attack exhibiting one of these signatures. 
- Flexibility. Hard coding the type of attack can be done easily, but it will be hard and time-consuming to adapt to attack patterns that keep changing constantly.

## **Solution**
Detect the occurrence of attacks by matching the current attack signature against the signature of previously known attacks.

## **Structure**
Figure 168 shows the class diagram of this pattern. The IDS intercepts an access request for a service. An EventProcessor processes the information and feeds this processed information to a AttackDetector, which tries to match the sequence of requests to the signatures in the AttackSignatureInformation and decides whether or not the request is an intrusion. If an attack is detected by getting a match of signatures, some appropriate Response is raised.

![](./Images/signature-based_ids_structure.png)

*Figure 168: Class diagram for the SIGNATURE-BASED IDS pattern*

## **Dynamics**
We describe the dynamic aspects of the SIGNATURE-BASED IDS pattern using a sequence diagram for the following use case.

|Use Case:|Detect an Intrusion – Figure 169|
| :- | :- |
|Summary|The Client requests a service from the Host. The Signature-Based IDS intercepts the message and determines whether the signature of the event matches an existing attack signature. If the request is an attack, appropriate response is raised.|
|Actors|Client, Server.|
|Precondition|Information about attack signatures is available.|
|Description|<p>1. A Client makes a service request for a service to the Host. </p><p>2. The IDS send the request event to an EventProcessor. </p><p>3. The EventProcessor processes the event as required by the AttackDetector and passes the processed event data to the AttackDetector. </p><p>4. The AttackDetector tries to detect whether this request is an attack or not by comparing the signature of the event with the available signatures in the AttackSignatureInformation. </p><p>5. If a match is detected, a Response is created.</p>|
|Alternate Flow|<p>- The AttackSignatureInformation may not be able to detect an attack (a false negative). </p><p>- The AttackSignatureInformation can match and may indicate an attack when no attack is present (a false positive).</p>|
|Postcondition|If an attack is detected while it is happening, suitable preventive measures can be adopted.|

![](./Images/signature-based_ids_dynamics.png)

*Figure 169: Sequence diagram for the use case ‘Detect an intrusion’*

## **Implementation**
We first need to create a database with a set of all the known or expected attack patterns. We then select a detection algorithm. Some possible detection algorithms are: 

- Expression matching. The simplest form of misuse detection involves searching the event stream for known attack pattern expressions [1]. 
- State transition analysis. The whole process is a network of states and transitions. Every observed event is applied to finite state machine instances (each representing an attack scenario), possibly causing transitions [1]. 
- Dedicated languages. Some IDS implementations describe intrusion signatures using specialized languages varying from compiled expressions to programming languages such as Java. A signature takes the form of a specialized program, with raw events as input. Any input triggering a filtering program, or input that matches internal alert conditions, is recognized as an attack [1]. 
- Genetic algorithms. A genetic algorithm is used to search for the combination of known attacks (expressed as a binary vector, each element indicating the presence of a particular attack) that best matches the observed event stream [1].

## **Example Resolved**
We added an intrusion detection system beside the existing firewall to the system. Now any request authorized by the firewall is checked against known attack signatures to detect whether the access request is a possible attack. If we detect an attack, an alert can be raised, and the firewall can block the request.

## **Consequences**
The SIGNATURE-BASED IDS pattern offers the following benefits: 

- Known attacks. Detection can be effective against known attacks. 
- Completeness. If all known attack signatures are available in the database, attacks can be detected in real time. 
- Flexibility. It is relatively easy to add new attacks to the detection set. 

The pattern also has the following potential liabilities: 

- It only works for known attacks: a new attack will not be detected. We have to constantly update the database with new attack signatures. 
- Some attacks don’t have well-defined signatures, or the attacker may disguise the signatures. This may lead to false positives and false negatives. 
- Some attacks may be so fast that it may be hard to recognize them in real time. 
- Attack patterns are closely tied to a given environment (operating system, hardware architecture, and so on) and cannot be applied easily to other systems.

## **Known Uses**
- An IDS can be combined with a firewall, as is done in Nokia’s network systems [2]. 
- Cisco IDS utilizes detection techniques including stateful pattern recognition, protocol parsing, heuristic detection, and anomaly detection [3]. 
- LIDS is a signature-based intrusion detection/defense system for the Linux kernel [4]. 
- RealSecure [5] by Internet Security Systems is an IDS adapted by IBM for intrusion detection packages. It can monitor TCP, UDP and ICMP traffic and, if a match is found, countermeasures can be implemented along with read/write server locking, IP blocking and other measures. This product is bundled with CheckPoint Software’s Firewall [6].

## **See Also**
- This pattern is a special (concrete) case of the REFERENCE MONITOR pattern. 
- Firewall patterns complement this pattern. 
- The response class could be implemented as a Strategy pattern [7].
 
## **References**

[1] Verwoerd, T., & Hunt, R. (2002). Intrusion detection techniques and approaches. *Computer communications*, *25*(15), 1356-1365.

[2] Nokia. (2001, April). Combining Network Intrusion Detection with Firewalls for Maximum Perimeter Protection. White Paper. <http://www.itu.dk/courses/DSK/F2003/Combining_IDS_with_Firewall.pdf>

[3] Cisco Systems. (n.d.). Cisco Intrusion Detection. <http://www.cisco.com/warp/public/cc/pd/sqsw/sqidsz/> 

[4] Linux. (n.d.). Intrusion Detection System. <http://www.lids.org/>

[5] IBM. (n.d.). Real Secure Intrusion Detection Systems. <http://publib.boulder.ibm.com/infocenter/sprotect/v2r8m0/index.jsp>

[6] Checkpoint Software Technologies. (n.d.). Retrieved July 20, 2010, from <http://www.checkpoint.com/products/softwareblades/ipsec-virtual-private-network.html>

[7] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
