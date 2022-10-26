# **Behavior-Based IDS**

## **Intent**
The BEHAVIOR-BASED IDS pattern describes how to check every request for access against patterns of network traffic in order to detect possible deviations from normal behavior (anomaly) that may indicate an attack and trigger appropriate responses.

## **Example**
A company uses a public network for its applications. The network is exposed to security threats, especially a variety of unknown attacks. Their business could be in jeopardy if their customers realize that their system is not secure enough.

## **Context**
Any network application where the temporal behavior of network traffic is repetitive and predictable.

## **Problem**
Whenever data is accessed from the Internet or other external networks, there is always a possibility that this access can be harmful to the network. We need to detect possible attacks while they are occurring. 

The solution to this problem must resolve the following forces:

- New attacks. In today’s world networks are constantly bombarded with new attacks that do not have a specific attack signature. We need to detect these kinds of attacks. 
- Real-time. We need to detect attacks in real time while they are happening, and not after the attack has happened and it is too late to recover from it. 
- Increased vulnerability. Some networks, such as mobile networks, are more vulnerable to unknown attacks because of their mobile nature.

## **Solution**
Observe the traffic over a network and try to find deviations from normal or expected behavior. Any deviation from normal behavior is treated as a sign of intrusion.

## **Structure**
Figure 112 shows the class diagram for this pattern. A Client requests some service from the system. The IDS intercepts this request and sends it to an EventProcessor. The EventProcessor processes the event data asXXX needed by the AttackDetector, which involves establishing profiles of normal behavior that can be compared with the current behavior in BehaviorProfileInformation and passes the processed data to the AttackDetector. When an attack is detected, a Response is created.

![](./Images/behavior-based_ids_structure.png)

*Figure 112: Class diagram for the BEHAVIOR-BASED IDS pattern*

## **Dynamics**
We present the dynamic aspects of the BEHAVIOR-BASED IDS pattern using sequence diagrams for the following use case.

|Use Case:|Detect an Intrusion – Figure 113|
| :- | :- |
|Summary|The Client requests a service from the Host. The behavior-based IDS intercepts the message and determines whether the behavior of the request matches a normal behavior profile. If it does not, an attack is suspected, and a response is raised.|
|Actors|Client, Server.|
|Precondition|A set of normal behavior profiles is available.|
|Description|<p>1. Client makes a service request to the Host. </p><p>2. The IDS send the request event to an EventProcessor. </p><p>3. The EventProcessor processes the event as required by the AttackDetector and passes the processed event data to the AttackDetector. </p><p>4. The AttackDetector tries to detect whether this request is an attack or not by comparing the behavior profile of the request with the available behavior profiles in the BehaviorProfileInformation. </p><p>5. If a match is detected, a Response is created.</p>|
|Alternate Flows|<p>- The BehaviorProfileInformation may not be able to detect an attack (a false negative). </p><p>- The BehaviorProfileInformation can match and may indicate an attack when no attack is present (a false positive).</p>|
|Postcondition|If an attack is detected while it is happening, suitable preventive measures can be adopted.|

![](./Images/behavior-based_ids_dynamics.png)

*Figure 113: Sequence diagram for the use case ‘Detect an intrusion’*

## **Implementation**
Examples of techniques used for anomaly detection in practice are: 

- Genetic algorithms. In this approach, applications are modeled in terms of system calls for different conditions, such as normal behavior, error conditions and attack conditions. A typical genetic algorithm involves two steps. The first step involves coding the input population of the algorithm. The second step involves finding a fitness function to test each individual of the population against some evaluation criteria. In the learning process each event sequence of node behavior forms a gene. Fitness is calculated for a collection of genes. If genes with required fitness cannot be found in the current generation, new sets of genes are evolved through crossover and mutation. The process of evolution continues until genes with the required fitness are found. The detection process involves defining vectors for event data and methods of testing whether the vector indicates an intrusion or not [1]. 
- Protocol verification. The basis for this approach is the fact that most intruders use irregular or unusual protocol fields, which are not handled properly by application systems [2]. 
- Statistical models. These can be either multivariate models or models based on available statistics such as threshold measures or mean and standard deviations of the profile. Clustering analysis where clusters represent similar activities or user patterns is also sometimes used [2].

## **Example Resolved**
We added an intrusion detection system to our network. Now all traffic is checked against a normal behavior profile to see whether the access request is an anomaly and hence a possible attack. We are now able to detect many new attacks that do not have a known signature and prevent them.

## **Consequences**
The BEHAVIOR-BASED IDS pattern offers the following benefits: 

- New attacks. Detection can be effective against new attacks that could cause abnormal behavior in the network traffic. For example, we can identify an attack with a specific behavior, such as when a usually passive web server tries to connect to a large number of addresses, it could be the result of a worm attack. 
- Real time. This kind of IDS works well with network traffic that exhibits a normal behavior and where it will be easier to detect an abnormal behavior pattern for the network. 
- Increased vulnerability. This kind of IDS is usually good in wireless networks, which are more vulnerable due to their mobile nature. 

The pattern also has the following potential liabilities: 

- It generates a lot of false positives. Many anomalies detected are not attacks but could be just unusual behaviors of users. 
- It cannot be implemented in networks that do not have a predictable traffic pattern. 
- The technology adopted for one network is not easily portable to another system and can be different from system to system in a network, as normal behavior for one system is usually not the normal behavior for another system. 
- If the attacker carries out an attack by mimicking regular traffic or normal behavior, the attack may go undetected.

## **Known Uses**
- Cisco IPS 4200 Series utilizes detection techniques including stateful pattern recognition, protocol parsing, heuristic detection, and anomaly detection [3]. 
- AirTight’s wireless IPS automatically detects, classifies, blocks, and locates wireless threats using behavior analysis. They use a genetic algorithm to establish normal behaviors [4]. 

Some other uses of anomaly based IDSs are given in Table 14 [5].

|**Name**|**Manufacturer**|**Hybrid**|**Response**|**Anomaly-Related Techniques**|
| :- | :- | :- | :- | :- |
|AirDefense Guard|AirDefense, Inc.|Y|Y|Detection, correlation, and multi-dimensional detection|
|Barbedwire’s IDS Softblade|Barbedwire Technologies|Y|Y|Protocol analysis, pattern matching|
|BreachGate WebDefend|Breach Security|Y||Behavior-based analysis, statistical analysis, Using correlation functions|
|Bro|Lawrence Berkeley National Laboratory|Y|Y|Application-level semantics, event analysis, pattern matching, protocol analysis|

*Table 14: Network-based IDS platforms with anomaly detection functionalities, according to the manufacturer’s information [Gar09]*

The Hybrid column indicates hybrid detection, and the Response column indicates that some kind of response mechanism is also available.

## **See Also**
- This pattern is used in conjunction with the SIGNATURE-BASED IDS pattern. 
- Firewall patterns are usually used together with the IDS in a network. 
- The response class could be implemented as a Strategy pattern [6].

## **References**
[1] Raja, P. K. (2006). Wireless node behavior based intrusion detection using genetic algorithm.

[2] Verwoerd, T., & Hunt, R. (2002). Intrusion detection techniques and approaches. *Computer communications*, *25*(15), 1356-1365.

[3] Cisco. (n.d.). Cisco Systems: Products and Technologies > Cisco Intrusion Detection. from <http://www.cisco.com/warp/public/cc/pd/sqsw/sqidsz/> 

[4] Airlight. (n.d.). Airtight Networks: WLAN Intrusion Prevention. from <http://www.airtightnetworks.com/home/solutions/wireless-intrusion-prevention.html> 

[5] Garcia-Teodoro, P., Diaz-Verdejo, J., Maciá-Fernández, G., & Vázquez, E. (2009). Anomaly-based network intrusion detection: Techniques, systems and challenges. *computers & security*, *28*(1-2), 18-28.

[6] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
