# **Hidden Metadata**

## **Intent**
Hide the meta information associated with data content that reveal information about sensitive data content.

## **Example**
Exposure. Alice is suffering from a medical condition, and she wants to find some information about it. She visits the website www.dishonest-medical-website.orgthat is controlled by Mallory (figure 56). While Alice is visiting the website, Mallory secretly gathers Alice’s email address, geographical location, computer type, operating system, web browser, previous web site visited etc. Mallory is gathering these information about Alice even if Alice does not accept cookies, which is primarily used for prof ling browser behavior. Mallory analyzes the HTTP\_USER\_AGENT, REMOTE\_HOST, and HTTP\_REFERER variables, which almost all web browsers provide to each site visited as part of the HTTP protocol.

HTTP\_USER\_AGENT reveals the user’s browser software, which the remote web site could use to generate web pages specifically tailored to the browser’s capabilities. However, both Netscape and IE also include the user’s computer type and operating system as part of this variable.

![](./Images/hidden_metadata_example.png)

*Figure 56:  Compromised Anonymity by Header Matching*

The REMOTE\_HOST variable reveals the Internet address of the computer making the request for a web page. Let us assume that Alice is running a single-user workstation. The computer’s identity may be the key to an enormous source of personal information. Using the Unix ‘finger’ command, Mallory can identify the Alice’s full name, email address, and even phone numbers. Even if Alice is accessing the web via a large commercial provider such as AOL, or from behind a corporate firewall, the REMOTE\_HOST f eld reveals that the user is an AOL member or an employee of that particular company. People who access the web via local Internet service providers (ISPs) reveal the identity of that ISP, which in turn reveals their geographic location. Mallory then performs a ‘whois’ lookup from the InterNIC database to find and report the physical address associated with the user’s Internet host.

The HTTP\_REFERER variable reveals the previous page visited by Alice. All of these information’s are provided without Alice’s consent or knowledge and thus is a threat to Alice’s privacy. Mallory also combines the data with another publicly accessible database such as a phone directory, marketing data, voter registration list, etc. She gains a significant amount of personal data on every visitor to her pages. All of this information-gathering is accomplished without Alice’s authorization or awareness.

Other than the application layer protocol attacks, the data packets carrying the request from Alice to the end host are also vulnerable to inspection by a global passive eavesdropper Eve. These packets contain the IP addresses for routing. Figure 56 shows that Alice is sending the packets through a Mix network along with other users Bob and Carol. The input data packets to the mix are encrypted. The mix decrypts the data, re-encrypts it and sends them out after adopting a mix strategy. However, the IP addresses associated with the data packets are needed for routing. Eve does not have access to the data content and because of the mix network’s mixing and morphing mechanism she cannot correlate the input and output packets based on data content. But with the use of the headers, she can easily identify which message is coming from Alice.

**In and out.** Alice and Bob are in a location tracking system where they want to anonymize the information of their whereabouts. The system has regions called the mix zone that work on the principle of mix network. Once multiple agents enter the region and then come out it should be impossible to identify the agents. The system has a handle for all the agents. The handle acts as a pseudonym for the agent. Suppose Alice has the pseudonym agent12345 and Bob has the pseudonym agent12346. Now agent12345 and agent12346 enter the mix zone and come out. However, they still retain their pseudonym and therefore are trivially identified.

## **Context**
You are designing a system to protect the privacy of the users. This system will maintain sender anonymity in a messaging scenario. Although this is an important application area, the context is not limited to messaging only. The context also entails other scenarios like anonymity in a location tracking system or anonymity preserving data sharing in a data publishing system.

## **Problem**
Any metadata associated with the data traffic in the network reveals information about the originator of that data packet. The packet headers are used for information routing in forward and reverse direction. The information in the packet headers like IP addresses reveal private information about sender’s identity and their location.

In the spatial mix zone-based location anonymity system, the agents are identified by their pseudonyms. If the pseudonym of an agent entering the mix zone and the agent exiting the mix zone remains the same, a location monitoring system can successfully correlate the outgoing agent with the incoming agent.

When private databases are shared for public use, obfuscation mechanisms like k-Anonymity [1] are applied on the database to create ANONYMITY SET. This ANONYMITY SET is compromised if information can be revealed by using meta-information associated with the data. Fow example, the sort order of a table in the database can be used to compromise private information. If there is a patient in a medical database whose last name starts with Z, and the patient database is sorted by last name then that person’s information should appear in the last portion of the database. If the sort order is retained, the data obfuscation may be unsuccessful.

How can the meta-data associated with the data content be hidden?

The forces that need to be considered when choosing to use this pattern are as follows. 

- Anonymity Service. Users might be sensitive about some of their Internet browsing behavior and do not want these behaviors to be associated with their prof le. For some other browsing activities, users might be apathetic to the fact that they are being prof led. In some cases, users might be willing to be prof led over a long time so that they can get customized service experience. 
- Routing. Packet headers are used for routing. Stripping off packet headers would make it diff cult to route the request. Also, the response from the recipient has to be sent back to the request originator. If the header is irretrievably tampered, then the response cannot be routed back to the originator. Encrypting packet headers do not work either because, the intermediate mixes have to access the packet headers for routing. If a mix is controlled by an adversary, the plaintext header information at that node would reveal sender identity. 
- Performance. The system should not have complex operations that add to the latency in a messaging system. The system should be applicable in a low-latency messaging domain like web browsing. 
- Type of Adversary. A global, passive adversary monitors the data traffic in the network and do not manipulate the data content. Active adversaries may control the mix nodes and manipulate the through traffic. Mix nodes can also be controlled by passive (or semi-honest) adversaries that adhere to the mix protocol but only monitor the data content passing through the node. The adversary has access to other third-party data sources that he can use to infer information. 
- User Consent. Most of the web browsing information are gathered without users’ consent. The technology underlying web browsing makes it possible for web sites to collect varying amounts of personal information about each user. Although it is important that the consumers have the right to be informed about the privacy and security consequences of an online transaction before entering into one, current technology does not provide any mechanism to enforce this user right. 
- Payment for Privileged Service. Users are sometimes willing to pay for privileged anonymity service for their sensitive data. The privileged service would involve lower latency in data transmission and better anonymity. 
- Law Enforcement. Anonymity can be abused by malicious users and to thwart that law enforcement agencies might require that the anonymity solutions sometimes lift their anonymity cover to investigate on crime suspects.

## **Solution**
Obfuscate the metadata associated with the data. For the web browsing domain, create a middleman between the request sender and the recipient that strips off identity-revealing meta-data from the packet headers. The sender submits the request to the middleman that acts as a proxy. It submits the request to the recipient on behalf of the sender but removes the values from the tags like HTTP\_USER\_AGENT and HTTP\_REFERRER from the header. For email messaging, use a remailer that strips identifying header information from outbound email messages. Hide the metadata that do not hamper message routing, or the service provided by the end server upon receiving the message.

For the location anonymity domain, strip off the pseudonym associated with an agent when he enters the mix zone. Assign a different pseudonym to the agent when he comes out of the mix zone.

For privacy preserving data sharing, scramble the sorting order of the data in the table. Remove any other meta-information in the data that can compromise the privacy.

## **Structure**

## **Dynamics**

## **Implementation**
**Storage of State Information.** The anonymizer should keep track of the changes it has made to the sender’s request header. This is needed to forward the response back to the sender. This can be stored in a table-based storage with hashed key, or a database if the anonymizer controls large amount of traffic. The anonymizer proxy should also keep the states to prevent replay attacks.

**Traffic Analysis of Anonymizing Proxy.** The ingress and egress traffic of anonymizing proxy can be monitored, and privacy can be compromised if the ANONYMITY SET is small. The anonymizer can adopt mix technologies to prevent against these attacks. Also, the sender might submit traffic to the anonymizer through a mix network or onion routing portal to achieve stronger anonymity guarantee.

**Trust Relationship with the Anonymizer.** The users should establish a trust relationship with the anonymizing proxy. A malicious proxy could in principle track its users’ browsing patterns and make unscrupulous use of that information. The trust establishment can be achieved with a legal contract or can be done dynamically using explicit trust negotiation protocols.

**Bandwidth Requirement.** The anonymizing proxy has to handle hundreds of thousands of page requests. For each request, the anonymizing proxy has to fetch, process, and forward a web page from elsewhere on the net. A subscription mechanism can be introduced to create users of various privilege levels and the requests can be prioritized based on that.

**Direct Sender-recipient Link.** Many ActiveX controls require direct linkage between the sender and the recipient. For example, RealAudio goes around the proxy by establishing their own direct net connections. Recent Ajaxian applications also require direct connection between the browser and the server. The link-rewriting mechanism of anonymizer proxy cannot provide anonymity for browsing these pages with active controls.

## **Example Resolved**

## **Consequences**
The pattern has the following benefits. 

- Privacy. The anonymizing proxy provides an alternative for privacy. It does not depend on costly cryptographic operations like a mix network. Also mix networks require wide-scale deployment, an issue that is not relevant to the architecture of anonymizing proxy. The anonymity service is offered transparently by the anonymization proxy, and the users do not have to modify their normal behavior to use the service.
- Business Incentive for running an Anonymizer. Business venture can be established to provide anonymity services like anonymizer, because it does not rely on wide-spread deployment of services. In a mix network, the mix node has to communicate with other nodes beyond the organizational boundary. The anonymizer can be deployed independently and the success of the it depends on the reputation of the authority running the anonymity service. Moreover, the anonymizer can provide privileged service based on subscription. A client using free service would experience higher latency than the paid service.

The pattern has the following liabilities. 

- Performance Overhead. Heavy traffic can throttle beyond the bandwidth limit of the anonymizer creating a DoS scenario. The storage and maintenance of meta-information of anonymized packets and packet processing cost has severe performance overhead. 
- Single Point of Failure. The anonymizing proxy is a single point of failure. For a mix network, a passive adversary has to monitor different parts of the network. On the other hand, for an anonymizing proxy monitoring the ingress and egress paths is sufficient for the attacker. 
- Forced Compromise of Privacy. An anonymizing proxy may keep track of the obfuscations it is making on incoming data to generate outgoing data traffic. This is especially necessary because the response traffic has to be routed in the reverse direction. Maintainer of an anonymizing proxy can be forced by law enforcement authorities to divulge this information, thereby undermining the anonymity of the proxy users. This can also lead to extortion from influential organizations. One of the first remailers built on this concept (the Penet remailer, developed in 1993) came under attack several times from different organizations. In 1995, the Church of Scientology f led a lawsuit against Johan Helsingius (the creator and maintainer of the Penet remailer) to disclose the identity of an anonymous user, who posted a stolen f le anonymously in the alt.religion.scientology newsgroup. The f le was stolen from the Church’s internal server. The Church’s initial claim was to reveal the identity of all the users of the remailer (about 300,000 in that time), but in the end they settled with the disclosure of the person responsible for the post. The identity of the anonymous user, who was posting under the pseudonym “-AB-" and the anonymous ID an144108@anon.penet.fi, was revealed to be Tom Rummelhart, a system administrator of the Church of Scientology’s INCOMM computer system.

Johan Helsingius was also contacted by the government of Singapore as part of an effort to discover who was posting messages criticizing the government in the newsgroup soc.culture.singapore. This time Johan did not have to compromise the identity of the user because the Finnish law did not rule the posting as a crime.

Then in September 1996, Church of Scientology sued Grady Ward [2] under the suspicion that he posted secret f les under the posting title “Scamizdat" in the Penet remailer and forced Johan Helsingius to disclose the identity of two users, an498608@anon.penet. fi and an545430@anon.penet.fi, posting under the handle “DarkDemonStalker". Johan decided to close the remailer in September 1996. The stories of these attacks on the Penet remailer have been written in many newspaper and online articles [3, 4, 5, 6]. Ironically, the Church extorted the information of the anonymous post, but it turned out to be anonymized by another anonymous remailer, the alpha.c2.org nymserver. alpha. c2.org was a more advanced and more secure remailer that obfuscated the mapping of the input and the output, and hence the Church could not get the conviction they were after.

## **Known Uses**
The Penet remailer (anon.penet.f) [7] was a pseudonymous remailer operated by Johan Helsingius of Finland from 1993 to 1996. The concept of this remailer is to provide a portal that stores pseudonyms for users. The users send messages hiding behind the pseudonym. By stripping the user’s name and assigning a pseudonym, the system provides sender anonymity through pseudonymity. Moreover, recipient anonymity can be achieved if the recipient of the mail is also a user behind a pseudonym. Because the users always use one pseudonym, it has the advantage that the users can create a reputation by using the pseudonym for a long time. But repeated use of the pseudonym means that privacy can be weakened by long term salvage of context information from a user’s correspondence.

The Anonymizer provides a technological means for preserving a user’s privacy when surf ng the web. A third-party web site (http://www.anonymizer.com) is set up to act as a middleman between the sender and the recipient. When the client wants to visit a web site, say the Google web site (www.google.com), he does not send the request directly to the Google server. Instead, it directs the request through the anonymizer proxy by using the URL http://www.anonymizer.com:8080/www. google.com. The Anonymizer then connects to google.com without revealing any information about the user who requested the information, and forwards the information received from Google to the user.

The first version of the Anonymizer was based on the public-domain CERN proxy server, but with several modifications to preserve anonymity:

- It does not forward the source IP address of the end-user. 
- It eliminates revealing information about the user’s machine configuration from the “User-Agent" MIME header, user’s name from the “From" MIME header, and previously visited site name from the “Referer" MIME header. 
- It does not forward the user’s email address to serve as a password for FTP transactions. 
- It filters out Java applets and JavaScript scripts which may compromise anonymity. 
- It filters out all “magic cookies" which may compromise anonymity. 
- It gives positive feedback to the user by displaying an Anonymizer header on the page and adding the word “[Anonymized]" to the page’s title.

The Anonymizer provides an easy-to-use interface which allows users to bypass the configuration procedure normally associated with using a proxy. Users access the service simply with extended URLs, such as http://www.anonymizer.com:8080/www. google.com/. The interface is flexible, and the users can freely switch to their regular browsing behavior when anonymity is not required.

There are various other anonymity providing services that are built on the principle of anonymizer, e.g., iProxy and the Lucent Personalized Web Assistant (LPWA). LPWA does not offer the Anonymizer’s page-rewriting mechanism which enables users to easily change between anonymized and non-anonymized browsing. However, it does provide an additional feature, support for anonymous authentication and registration at web sites which provide personalized services.

Mix zone [11, 12] is the same concept of mix networks taken into the domain of location anonymity. Agents are identified by pseudonyms in the mix zone. When the pseudonymous agent enters into the mix zone, his pseudonym is changed, so that once he comes out of the zone, his identity cannot be correlated with that of the entering agent. The mix zone concept was added with the Active Bat [13] system to provide location anonymity.

The principle of k-Anonymity [1] was introduced by Latanya Sweeney for publishing of secret data. The sensitive information in a dataset is obfuscated by replacing them with a more general information. The sort order is also altered to obfuscate meta-information.

## **See Also**

## **References**

[1] Sweeney, L. (2002). k-anonymity: A model for protecting privacy. *International Journal of Uncertainty, Fuzziness and Knowledge-Based Systems*, *10*(05), 557-570.

[2] Newman, R. (1996, July 24). The Church of Scientology vs. Grady Ward. XS4ALL. http://www.xs4all.nl/~kspaink/cos/rnewman/grady/home.html

[3] Post, D. G. (1996). The first Internet war-The state of nature and the first Internet war: Scientology, its critics, anarchy, and law in Cyberspace. *Reason Magazine*.

[4] Newman, R. (1997, Mar 23). The Church of Scientology vs. anon.penet.fi - Julf Helsingius voluntarily closes his remailer after Finnish court orders him to turn over a user name to Scientology. XS4ALL. http://www.xs4all.nl/~kspaink/cos/rnewman/anon/penet.html

[5] EFF. (1996, Sept 23). Johan Helsingius gets injunction in Scientology case: Privacy protection of anonymous messages still unclear. EFF. http://www.eff.org/Privacy/Anonymity/960923\_penet\_ injunction.announce

[6] Helsingius, J. (1996, Aug 30). Johan Helsingius closes his Internet remailer. Cyberpass. http://www.cyberpass.net/security/penet. press-release.html

[7] Helsingius, J. J. (1995). The anon.penet.fi anonymous server.

[11] Beresford, A. R., & Stajano, F. (2004, March). Mix zones: User privacy in location-aware services. In *IEEE Annual conference on pervasive computing and communications workshops, 2004. Proceedings of the Second* (pp. 127-131). IEEE.

[12] Beresford, A. R., & Stajano, F. (2003). Location privacy in pervasive computing. *IEEE Pervasive computing*, *2*(1), 46-55.

[13] Ward, A., Jones, A., & Hopper, A. (1997). A new location technique for the active office. *IEEE Personal communications*, *4*(5), 42-47.

## **Source**
Hafiz, M. (2006, October). A collection of privacy design patterns. In *Proceedings of the 2006 conference on Pattern languages of programs* (pp. 1-13).