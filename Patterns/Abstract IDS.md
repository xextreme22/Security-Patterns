# **Abstract IDS**

## **Intent**
The ABSTRACT IDS pattern allows monitoring of all traffic as it passes through a network, and its analysis it to detect possible attacks and trigger an appropriate response.

## **Example**
Our company has a firewall to control traffic from the Internet. However, we are still plagued by viruses and other attacks that penetrate the firewall. The attacks can be existing attacks or new attacks. We need to improve our defense against such attacks.

## **Context**
Nodes for local systems that need to communicate with each other using the Internet or another insecure network.

## **Problem**
An attacker may try to infiltrate our system through the Internet and misuse our information by reading or modifying it. We need to know when an attack is happening and take appropriate response. 

The solution to this problem must resolve the following forces:

- Communication. The system is usually more secure if we have a closed network. However, in today’s world it is better and more realistic to use the Internet or other insecure network to reduce costs, which may subject our network to security threats. 
- Real time behavior. Attacks should be detected before the attack completes its purpose, so that we can preserve our assets and save time and money. It is difficult to detect an attack when it is happening, but such detection is imperative if we are to react timely and appropriately. 
- Incomplete security. Security measures such as encryption, authentication and so on may not protect all our systems, because they do not cover all possible attacks. 
- Non-suspicious users. Protecting our system through a firewall is quick and easy. However, request coming from a non-suspicious address (that is, one permitted by a firewall) could still be harmful and should be monitored further. 
- Flexibility. Hard coding the type of attack can be done easily. But it will be hard and time-consuming to adapt to attack patterns that change constantly

## **Solution**
Each request to access the network is analyzed to check whether it conforms to the definition of an attack. If we detect an attack, an alert is raised, and some countermeasures may be taken.

The ABSTRACT IDS pattern defines the basic features of any intrusion detection system (IDS). An abstract pattern defines only fundamental, implementation-independent functions and threats [1]. Concrete patterns add functionalities and threats and consider the characteristics of their specific concrete environment. In this case, the abstract functions are realized by concrete IDSs that operate based on known attack signatures or based on abnormal behavior or anomaly in the network: SIGNATURE-BASED IDS or BEHAVIOR-BASED IDS. We present here patterns for all these three types of IDS.

Figure 96 shows the typical placement of an IDS in a network, complementing a firewall. The firewall filters requests for services, and the IDS further checks for suspicious patterns in request sequences. If a suspicious pattern is detected, the network operator is alerted, and the firewall may block some or all traffic.

![](./Images/abstract_ids_solution.png)

*Figure 96: Possible placement of network IDS to complement a firewall*

## **Structure**
Figure 97 shows the class diagram for the ABSTRACT IDS pattern. A Client requests some service from the Server. The IDS intercepts this request and sends it to an EventProcessor. The EventProcessor processes the event so that the AttackDetector can analyze the event and implement some method of detection using information from AttackInformation. When an attack is detected, a Response is created.

![](./Images/abstract_ids_structure.png)

*Figure 97: Class diagram for the ABSTRACT IDS pattern*

## **Dynamics**
We describe the dynamic aspects of the ABSTRACT IDS pattern using a sequence diagram for the following use case.

|Use Case:|Detect an Intrusion – Figure 98|
| :- | :- |
|Summary|The Client requests a service from the Host. The IDS intercepts the message and checks whether the request is an attack or not and raises a response.|
|Actors|Client, Server.|
|Precondition|We have attack information available.|
|Description|<p>1. A Client makes a service request to the Host. </p><p>2. The IDS send the request event to an EventProcessor. </p><p>3. The EventProcessor processes the event data so that the AttackDetector can interpret the event. </p><p>4. The AttackDetector tries to detect whether this request is an attack or not by comparing with the available information in the AttackInformation. </p><p>5. If an attack is detected, a Response is created.</p>|
|Alternate Flows|<p>- The AttackInformation may not be able to detect an attack (a false negative). </p><p>- The AttackInformation may indicate an attack when no attack is present (a false positive).</p>|
|Postcondition|If an attack is detected, suitable preventive measures may be applied.|

![](./Images/abstract_ids_dynamics.png)

*Figure 98: Sequence diagram for the use case ‘Detect an intrusion’*

## **Implementation**
We need to create a database with attack information so that we can check against this database and decide whether an attack is happening. The incoming event is compared against the database and a decision is made whether the incoming event is an attack or not. The concrete versions of this pattern use different types of information to detect attacks.

The Common Intrusion Detection Framework (CIDF) is a working group created by DARPA in 1998 that is mainly oriented towards creating a common framework in the IDS field. CIDF defined a general IDS architecture based on the consideration of four types of functional modules as shown in Figure 99 [2].

![](./Images/abstract_ids_implementation.png)

*Figure 99: General CIDF architecture for IDS systems (from [Gar09])*

- E blocks (‘event-boxes’). This block is composed of sensor elements that monitor the target system, thus acquiring information events to be analyzed by other blocks.
- D blocks (‘database-boxes’). These are elements intended to store information from E blocks for subsequent processing by A and R boxes. 
- A blocks (‘analysis-boxes’). Processing modules for analyzing events and detecting potential hostile behavior. 
- R blocks (‘response-boxes’). The main function of this type of block is the execution, if any intrusion occurs, of a response to thwart the detected menace.

## **Example Resolved**

## **Consequences**
The ABSTRACT IDS pattern offers the following benefits: 

- Communication. If we can detect most attacks, we can safely use the Internet or other insecure networks to access other systems. 
- Real time behavior. Attacks can be detected when the attack happens and the system alerted, which saves both time and money in recovery measures, and may prevent misuse of assets. Attacks can be detected in real time if they have sufficient and appropriate information. 
- Incomplete security. The IDS provides be an added layer of security in addition to encryption, authentication and so on. 
- Non-suspicious users. A request coming from a non-suspicious address (permitted by a firewall) is further inspected and analyzed. 
- Flexibility. The detection information can be modified to include new attacks or new behavior. 

The pattern also has the following potential liabilities: 

- Some attacks may be so fast that it may be hard to recognize them in real time. 
- Attack patterns are closely tied to a given environment (operating system, hardware architecture and so on) and cannot be applied easily to other systems. This means we need to define detection information tailored to an environment. 
- There is some overhead in the addition of IDSs to a system. 
- The concrete versions of these systems have additional liabilities.

## **Variants**
- IDS can be either behavior (rule) based or can be based on anomalies (abnormal behavior). There are significant differences in the use and effectiveness of these two approaches. See SIGNATURE-BASED IDS pattern and BEHAVIOR-BASED IDS pattern. 
- A hybrid model of both the signature based and BEHAVIOR-BASED IDS together is now available: a BEHAVIOR-BASED IDS detects the anomalies in traffic and then compares the anomalies with an attack signature in a signature-based IDS. 
- According to the resources they monitor, IDS systems are divided into two categories: host-based IDS systems and network-based IDS systems. Host-based IDS systems are installed locally on host machines and evaluate the activities and access to key servers within the host. Network-based IDS systems inspect the packets passing through the network [3]. 

## **Known Uses**
NID is a freely available hybrid intrusion detection package. It monitors network traffic and scans for the presence of known attack signatures, as well as deviations from normal network behavior [4].

## **See Also**
Firewall patterns can be added to complement the IDS. Firewalls usually deny requests made by unknown addresses. They can protect against attacks coming from distrusted sources and can block the addresses from where an attack originates. The response class could be implemented as a Strategy pattern [5].

## **References**

[1] Fernandez, E. B., Washizaki, H., & Yoshioka, N. (2008, October). Abstract security patterns. In *Proceedings of the 15th Conference on Pattern Languages of Programs* (pp. 1-2).

[2] Garcia-Teodoro, P., Diaz-Verdejo, J., Maciá-Fernández, G., & Vázquez, E. (2009). Anomaly-based network intrusion detection: Techniques, systems and challenges. *computers & security*, *28*(1-2), 18-28.

[3] Depren, O., Topallar, M., Anarim, E., & Ciliz, M. K. (2005). An intelligent intrusion detection system (IDS) for anomaly and misuse detection in computer networks. *Expert systems with Applications*, *29*(4), 713-722.

[4] C. Grace. (n.d.). Understanding Intrusion Detection Systems. IT Journalist PC Network Advisor – Tutorial. <http://www.techsupportalert.com/pdf/t1523.pdf>

[5] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
