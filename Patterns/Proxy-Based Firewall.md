# **Proxy-Based Firewall**

## **Intent**
A PROXY-BASED FIREWALL inspects, and filters incoming and outgoing network traffic based on the type of application service to be accessed or performing the access. This pattern interposes a proxy between the request and the access and applies controls through this proxy. This is usually done in addition to the normal filtering based on addresses.

## **Example**
After we started using a PACKET FILTER FIREWALL most of our problems were reduced. However, some of the messages sent from sites we don’t consider suspicious contain malicious payloads, because hackers were spoofing trusted addresses. These payloads sometimes contained incorrect commands or the wrong type and length of parameters. Our PACKET FILTER FIREWALL cannot stop these attacks because it doesn’t look at the message payload, and as a result we are experiencing new problems. It is also hard to block every malicious site.

## **Context**
Computer systems on a local network connected to the Internet and to other networks, where a higher level of security than the one provided by packet filters is needed. Specifically, we want to control attacks at the application layer of the network protocol. Incorrect commands or parameters can produce buffer overflows and other conditions that can be exploited for attacks. In some cases, we might also want to authenticate the client to avoid spoofing. Outgoing flows (to malicious sites) can also be damaging in this environment.

## **Problem**
PACKET FILTER FIREWALL only inspects the network addresses when deciding whether to allow access for a request. We can only block supposedly malicious sites. It is hard to know about all of those sites, and we need further defenses. Also, how do we protect our network from potential attacks that might be embedded within the data segment of the packets?

The solution to this problem must resolve the following forces: 

- We need to let external networks access our services and local users access external sites. Isolation is not acceptable. 
- There are a variety of application services in a system, for example mail, file transfer, and others. Hackers can plan specific attacks against them, and we need to be prepared for a variety of attacks. 
- Network administrators deploy and configure a variety of protection mechanisms, so it is important to have a clear model of what is being protected and what types of attacks are possible. 
- The protection mechanism should be able to precisely reflect the security policies of the organization. 
- The types of attacks are constantly changing, so it should be easy to make changes to the configuration of the protection mechanisms. 
- It may be necessary to log requests for auditing and defense purposes.

## **Solution**
Make the client interact only with a proxy of the service requested, which in turn communicates with the protected service (see figure 212). The client can only receive service from the server if an application proxy exists for the requested service. Each application proxy has its own access rules pre-defined by the administrator that may be used to authenticate, inspect, change, and filter the incoming (or outgoing) messages.

![](./Images/proxy-based_firewall_solution.png)

*Figure 212: The concept of the PROXY-BASED FIREWALL*

## **Structure**
The figure below shows the class diagram for this pattern. We show here only the proxy aspects: the classes shown in figure 213 can be part of this firewall or can be provided separately. This firewall contains Proxies, which in turn contain Rules, collected in a RuleBase. All the hosts of a local network share the firewall. Each local host provides a set of services. The rules may now specify specific constraints for the use the available services.

![](./Images/proxy-based_firewall_structure.png)

*Figure 213: Class diagram for PACKET FILTER FIREWALL*

## **Dynamics**
We illustrate a use case for filtering requests for services. See the sequence diagram on figure 214.

|Use Case:|Providing Service to a Client|
| :- | :- |
|Summary|An external client wants access to a service from a local host. The access request is made through the firewall, which according to its application proxies and their rules determines whether to deny or accept the request.|
|Actors|External client.|
|Precondition|None.|
|Description|An external network requests a service to the PROXY-BASED FIREWALL. The firewall filters the request according to its application proxies and their access rules. If none of the rules in the rule set are satisfied, then a default rule is used to filter the request. If the request is accepted, the client is allowed to access the service through the proxy.|
|Alternate Flows|If the service request is not supported by the PROXY-BASED FIREWALL, or the firewall considers the client untrustworthy, the firewall will block the access.|
|Postcondition|The firewall has accepted the service request from a trustworthy client to the local host.|

![](./Images/proxy-based_firewall_dynamics.png)

*Figure 214: Sequence diagram for filtering service requests.*

## **Implementation**
1. According to organization policies, define which services will be made available to clients of the network. 
1. Write, reuse, or buy a proxy for each service and assign a location or address to it. 
1. Define who can have what type of access to which service and other restrictions on their use.
1. Implement these constraints in the rule base. 
1. Consider configurations such as PROTECTION REVERSE PROXY, INTEGRATION REVERSE PROXY or a combination with a PACKET FILTER FIREWALL in a distributed configuration [1].

## **Example Resolved**
We bought a PROXY-BASED FIREWALL and now every request for a service is authenticated and checked. We can verify that the requests are authentic and filter out some payload attacks, for example, a wrong command for a service, wrong type parameters in the service call, and so on.

## **Consequences**
The following benefits may be expected from applying this pattern: 

- The firewall inspects and filters all access requests based on predefined application proxies that are transparent to the users of the services. In some cases, it may even modify a request—for example, doing network address translation. 
- It is possible to express the organization’s filtering policies through its application proxies and their rules. 
- The implementation details of the local host can be hidden from the external clients. This also improves security. 
- A firewall permits systematic logging and tracking of all service requests going through it. This facilitates the detection of possible attacks and helps hold local users responsible of their actions. 
- It provides a higher level of security than packet filters because it inspects the complete packet including the headers and data segments. This global view may control attacks in the payload and attacks based on the structure and size of the packets. 

The following potential liabilities may arise from applying this pattern: 

- Possible implementation costs due to the need for specialized proxies. The proxies also need to be configured correctly. On the other hand, proxies already exist for common services. 
- Performance overhead due to the need for inspection of the data segment of packets and maybe additional checking.
- Increased complexity of the firewall. A PROXY-BASED FIREWALL may require a change in applications and/or the user’s interaction with the system. This is not necessary, however, in a well-designed system.

## **Known Uses**
Some specific firewall products that use application proxies are Pipex Security Firewalls [2] and InterGate Firewall. The SOCKS Protocol from IETF, although not intended as a firewall, uses a similar principle [3]. Postfix filters act as proxy and packet filter firewalls [4].

## **See Also**
This pattern uses the PROXY pattern from [5]. It can be combined with PACKET FILTER FIREWALL and STATEFUL FIREWALL.

## **References**

[1] CyberGuard Corporation. (2003, January). Defense in Depth: Applying a Fortified Firewall Strategy. white paper

[2] Pipex. (n.d.). Pipex Firewall Solutions. <http://www.security.pipex.net/about.shtml> 

[3] IETF. (n.d.). SOCKS Protocol. <http://www.socks.permeo.com/> 

[4] Hafiz, M., Johnson, R. E. (2005). The Security Architecture of qmail and Postfix. <https://netfiles.uiuc.edu/mhafiz/www/ResearchandPublications/SecurityArchitectureofqmailAndPostfix.htm> 

[5] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

## **Source**
Schumacher, M., Fernandez-Buglioni, E., Hybertson, D., Buschmann, F., & Sommerlad, P. (2006). Security Patterns: Integrating Security and Systems Engineering (1st ed.). Wiley.
