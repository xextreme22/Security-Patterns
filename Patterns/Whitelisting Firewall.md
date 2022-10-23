# **Whitelisting Firewall**

## **Intent**
We want to prevent the client to get access to an external site or service (any kind of IP address or port) that is considered untrustworthy, or to stop traffic from an untrusted site. WHITELISTING FIREWALL (WLF) defines a list of sites with which we want to communicate.

## **Example**

## **Context**
Nodes connected to the Internet and to other networks who need to enforce their policies to all of the sites as one layer in a defense in depth strategy in their site.

## **Problem**
Some sites are insecure and may contain malware, which could attack our host or download malware if we visit them. How to prevent or stop the traffic of a system so we do not get access to external sites that are considered untrustworthy?

The solution will be affected by the following forces: 

- Security: We handle sensitive assets. We only want to communicate with sites that are known by the user and are trusted. This will increase security. 
- Transparency: The users of the system should not need to perform special actions, or they may not use the filtering. 
- Overhead: Filtering should not significantly reduce performance. 
- Usability: It should be easy to apply and manage filtering policies. 
- Number of interlocutors. The number of sites with which we want to communicate is reasonable small.

## **Solution**
Use a WHITELISTING FIREWALL which is a filtering mechanism that can enforce communication with only approved sites by keeping a list of acceptable interlocutors (hosts or sites).

## **Structure**
The class diagram for the WHITELISTING FIREWALL is shown in Figure 27. The Whitelisting (WL) Firewall intercepts the traffic between a LocalHost and a set of ExternalHosts. The WLFirewall includes a Whitelist, which is composed of a set of ordered rules. The ordering of rules accelerates checking by avoiding checking of sites included in site groups. Some of the rules may be ExplicitRules or may be ImplicitRules (default) rules, which apply to all sites.

![](./Images/whitelisting_firewall_structure.png)

*Figure 27: Class diagram for the WHITELISTING FIREWALL pattern.*

## **Dynamics**
We describe the dynamic aspects of the WHITELISTING FIREWALL using a sequence diagram for one of its basic use cases. Basically, the WHITELISTING FIREWALL applies the policy of a closed world, where every address is denied access unless explicitly permitted. Figure 28 shows a sequence diagram for the Use Case "Filter an incoming request". A node requests to run some service or application. The WHITELISTING FIREWALL receives this request and using a list of known and trusted sites checks if the request comes from or goes to a trusted site and allows the connection or denies the request.

|Use Case:|Filtering an Incoming Request – Figure 28|
| :- | :- |
|Summary|A host in a remote network wants access to a site to either transfer or retrieve information. The access request is made through the firewall, which according to its set of rules determines whether to accept or deny the request.|
|Actors|A host in an external network trying to access a site (local host).|
|Precondition|A set of rules to filter the request exist in the whitelist of the firewall.|
|Description|<p>1. An external host requests access to the local host. </p><p>2. A WHITELISTING FIREWALL filters the request according to its rules to accept or deny the access. </p><p>3. If the request is accepted, the firewall allows access to the site.</p>|
|Alternate Flows|- If no rule allows the access, the request is denied.|
|Postcondition|The firewall has accepted the access of a trustworthy site to the Localhost.|

![](./Images/whitelisting_firewall_dynamics.png)

*Figure 28: Sequence diagram for the UC: Filter an incoming request.*

## **Implementation**
The WHITELISTING FIREWALL should reside on a host computer and maintain a local set of rules, which are maintained by a security administrator. The institution must define these rules according to their policies. The whitelist is usually rather short and there is no need for hardware assistance to search it and the filtering should happen at the IP layer. It is possible to filter also at the application layer and STATEFUL FIREWALLs could accelerate filtering.

There are specific approaches to use with the WHITELISTING FIREWALL such as Gold Image and Digital Certificates. With the Gold Image for static systems, the list is created by first hashing a standard workstation image. After using an image to build the first whitelist, keeping it up to date will be the biggest challenge facing most groups.

On the other hand, digital certificates are one of the most effective techniques to trust certain publishers of software. Based on their signature the certificates are automatically in the whitelist and are considered trusted [1].

## **Example Resolved**

## **Consequences**
This pattern has the following advantages: 

- Security. Whitelisting is more secure than blacklisting because we only deal with trusted sites. 
- Transparency: Filtering is transparent to the users. 
- Overhead: Checking is faster because whitelists are shorter than blacklists. 
- Usability. The list is short and only requires updates when a new site is added. 

This pattern has the following disadvantages: 

- Annoyance. It may cause some users to be annoyed because they cannot download applications from anywhere or visit sites not in the list. 
- Effect of address changes. If a trusted site in the whitelist changes its address, we need to update the list. 
- Wolf in sheep clothes. A trusted site may still be harmful. 
- Breadth. It is inappropriate if we need to offer products or services and we want to reach a wide audience. 
- Number of interlocutors. If the number of sites with which we want to communicate is large, this may not be a convenient approach.

## **Known Uses**
- Bit9 Parity: It uses a software registry, a locally installed management server and a client to enforce software policies throughout the enterprise. Bit9 developed an adaptive whitelist strategy. The proprietary Global Software Registry is an online index. This list contains unique applications and the registry acts as a reference library for IT administrators building their whitelists [2]. Bit9 has also built a Parity Suite based on the qualities of transparency, flexibility, and scalability [3]. 
- Coretrace Bouncer: It contains a standard file‐based whitelisting protection mechanisms [4]. 
- McAfee: This centrally managed whitelisting solution uses a dynamic trust model and security features that try to control advanced persistent threats [5].

## **See Also**
- The PACKET FILTER FIREWALL pattern: Filter traffic from untrusted sites based on blacklists. 
- The CREDENTIAL pattern: Provide secure means of recording authentication and authorization information for use in distributed systems.

## **References**

[1] SANS Institute. (n.d.). Application Whitelisting: Panacea or Propaganda? SANS. Retrieved November 17, 2012, from http://www.sans.org/reading\_room/whitepapers/application/application‐whitelisting‐panacea‐propaganda\_33599

[2] Bit9. (n.d.). Bit9 Security Platform. Retrieved November 17, 2012, from http://www.bit9.com

[3] EMA Corporation. (2012, October). Realistic Security, Realistically Deployed: Today’s Application Control and Whitelisting. Bit9. Retrieved November 17, 2012, from http://www.bit9.com

[4] Coretrace Corp. (n.d.). Coretrace products. Coretrace. Retrieved November 17, 2012, from http://www.coretrace.com/products‐2/bouncer‐overview#application

[5] McAfee. (n.d.). McAfee Application Control. McAfee Products. Retrieved March 21, 2012, from http://www.mcafee.com/us/products/application‐control.aspx

## **Sources**
Villarreal, I. N. B., Fernandez, E. B., Larrondo-Petrie, M., & Hashizume, K. (2013). A Pattern for Whitelisting Firewalls (WLF). PLoP.
