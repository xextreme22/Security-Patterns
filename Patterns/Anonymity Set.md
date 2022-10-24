# **Anonymity Set**

## **Intent**
Hide the data by mixing it with data from other sources.

## **Example**
**The Athenian Mistake.** In a message communication scenario, the content of the message is not always important. Merely the fact that a sender is sending a message can reveal important information. Suppose there are two regions Athens and Sparta, that are going through troubled times. The threat that one can launch a preemptive attack on the other is imminent. The Athenian army hired a veteran cryptographer who devises an unbreakable cipher. The intelligence branch of Sparta has not been able to decrypt this cipher scheme, but they have under-cover probes that let them know who is sending message to whom, a correlation that the Athenian army is not choosing to conceal.

Deep into one night, Athens decides to launch the attack the next morning. Suddenly there is a furry of messages passing among the chain of command of the Athenian army. Spartan intelligence picks up the information that suddenly the Athenian Generals have become active late into the night. They mobilize their army that night. Athens was not prepared for the counterstrike. They lose the battle.

**Protected Health Information.** Health Insurance Portability and Accountability Act (HIPAA) [1] of 1996 defines the appropriate way to handle Protected Health Information (PHI). For research purpose, Cure Clinic is releasing its PHI about cancer victims to Acme Laboratories. Let us suppose that Cure Clinic was keeping the patients’ name, birth date, sex, zip code and diagnostics record. To protect privacy, Cure Clinic does not release the name of the patients. The birth date, sex, zip code and diagnostics records are released. Acme Laboratories is doing the research on the probability of cancer attack on a particular age group and sexual orientation over the people of a particular locality. So, all the data fields that are released are important for the research. Mallory is a malicious worker at Acme Laboratories who wants to unravel private information from this data. Mallory goes to the city council of a particular area and gets a voter list from them. The two lists are matched for age, sex, and locality. Mallory finds the name and address information from the voter registration data and the health information from the patient health data.

## **Context**
You are designing a system to protect the privacy of the users. This system will maintain sender and/or recipient anonymity in a messaging scenario. Although this is an important application area, the context is not limited to messaging only. The context also entails other scenarios like anonymity in a location tracking system, anonymous voting in an electronic voting system, or anonymity preserving data sharing in a data publishing system.

## **Problem**
It is difficult to ensure sender and recipient anonymity during message communication. The only concern of traditional security approaches is to protect data content. This does not hide the message path from the sender to the receiver and thus the anonymity is compromised.

In an electronic voting system or an online voting system, the system should protect the privacy of the voters by not revealing their vote.

Similarly, a user may want to hide his information from a location tracking system. This may be because the location tracking device is offering some context-aware service based on user location, but the user is not interested at the moment, or may be because the user is not trusting the location tracking system or may be because the user does not want to reveal his private location information at all.

When private datasets are released, the private data about the subjects may be exposed. The released dataset has to be sufficiently rich in order to be useful, but it also should protect the privacy of the entities.

In all the cases, the general problem is to ensure the anonymity. How can the anonymity of an entity or a personal information be retained?

The forces that need to be considered when choosing to use this pattern are as follows. 

- User Count. The number of users using an anonymous messaging system may vary with time. This fluctuation may depend on operational hours, user interest, etc. If the user f ow is low, the solution does not work because there are not enough candidates to create the anonymity set. 
- User Friendliness. Users should be able to use the privacy enabling mechanisms with minimal alteration of their primary task. If the users have to adapt a lot to achieve anonymity, they may start judging where they should have anonymity. This way, a user’s misjudgment can sometimes reveal private information. 
- Data Usability. An anonymous data set has to be usable. One extreme of achieving anonymity is not to release any data, but obviously this is not a usable scheme. 
- Performance. The privacy retaining operations should not become a performance bottleneck (i.e., latency, bandwidth etc.). For anonymous messaging, the system should be usable in low latency usage scenarios, like web browsing. 
- Law Enforcement. Law enforcement agencies might require that the anonymity solutions sometimes lift their anonymity cover to investigate on crime suspects. This would prevent a malicious user from abusing anonymity

## **Solution**
Mix the private information with other information so that the private information is not distinguishable from other information. Create a set of equally probable information and hide the user information by making it a part of the set. This set is called the anonymity set. If the size of the anonymity set is large, it will ensure stronger privacy.

For message communication, create an abstract mechanism between the message sender and the recipient that hides the correlation between the sender and the recipient. This can be done by network intermediaries called mix networks that mix the message coming from one source with messages coming from different other sources. Once the data packet from the sender passes through one of these filters, it is indistinguishable from other packets (i.e., sender anonymity). The anonymity set is the set of messages from different sources. Figure 53 illustrates this solution. Alice’s data is collected at the mix node and mixed with Bob and Carol’s data.

![](./Images/anonymity_set_solution.png)

*Figure 53: Sender anonymity in a mix network*

For recipient anonymity in message communication, broadcast the message to all users or send the message to a message pool instead of one single recipient. The recipient will view the message like everyone else, but an adversary will not be able to tell who that message is for.

Use the same idea for sender anonymity in a location monitoring system. Install the abstract obfuscation mechanism in a region. Agents are identified by pseudonyms in the location tracking system. Once an agent enters the region where the obfuscation mechanism is installed, he is given a different pseudonym. If there are multiple agents in the obfuscation region and all of them adopt a different pseudonym upon entry, then the agents in the region create the anonymity set at a particular point of time. Once the agents come out of the region, their new pseudonyms cannot be correlated with the old pseudonyms.

In an anonymous voting system, a voter’s vote is passed through an obfuscation mechanism where it is mixed with other votes and the generated outcome leaves no trace that can be used to link the voter with the vote.

When releasing private data sets for public use, change the specific values of attributes that might reveal private information to more generalized values. If the dataset has a specific gender information, change the values of gender (male/female) to more general values (person). Also partition the attribute domain space and provide partitioned information rather than exact information. For example, if the dataset has specific age information, create age groups, and convert the dataset such that the specific ages are replaced with age groups. What this mechanism is doing is that it is creating an anonymity set so that one row in the dataset becomes indistinguishable from another.

## **Structure**

## **Dynamics**

## **Implementation**
**Size of the Anonymity Set.** The size of the anonymity set will determine how good the obfuscation will be. If the set is small, then correlation between input and output of the obfuscation mechanism can be determined with higher probability. If the anonymity set has only one element, then the privacy is provably compromised.

**Latency.** In a messaging scenario, the mixing mechanism might stall the data traffic to wait for enough data packets to arrive so that the mixing can be done effectively. This means that the latency of the data f ow increases, which might make it unusable in a low latency messaging scenario like web browsing. Different strategies can be taken to counter the latency issue. The required degree of privacy is scenario specific and based on that the designer can identify the trade-off between privacy and performance. 

**Usability of Information.** In the case of dataset release, absolute data obfuscation is possible by replacing all the specific attribute values with more general values. But this way the datasets may not be useful. For example, if a research is interested in the impact of sexual orientation on cancer attacks and the dataset has been anonymized in a way that all the gender values are replaced with a more general ’person’ value, then this dataset becomes useless for the research purpose. So, an anonymity protection mechanism that retains the usability of data is required.

## **Example Resolved**

## **Consequences**
The pattern has the following benefits. 

- Privacy. The obfuscation mechanism ensures that private information is not easily compromised. Not all mechanisms provide absolute privacy, but they ensure that the attacker will have to do more work to break into the system’s sensitive information. 
- Freedom from User Profiling. Business entities are interested in user prof ling to make smart advertisements. Users may not want to be bothered by these marketing suggestions. An anonymous user is free from such user annoying sales mechanisms. 
- Minimal user involvement. The users do not have to modify their normal activities to get anonymity service. The service is provided by proxies resident at the user end and the intermediaries in the network. Usability is improved because of this transparency.

The pattern has the following liabilities. 

- Performance. In the messaging domain, when the system is waiting for enough probable suspects to arrive to mix with incoming traffic, the users experience increased latency. Cover traffic can be used to create dummy probable suspects. But maintenance of a cover traffic f ow is expensive in the bandwidth. Also, the mix nodes might employ a batched transaction strategy that causes flush traffic out of the anonymity nodes. In that case, bandwidth becomes a big factor.
For data anonymization, it has been proven that general data obfuscation mechanism is NP-Hard [2]. In a location anonymity system, adding effective obfuscation mechanism (by introducing cover traffic) is very computation extensive.

- Usability of Information. Too much data obfuscation can undermine the usefulness of data. In the case of private dataset publishing, if all the attributes of the dataset are anonymized such that they retain privacy, the resultant dataset may not be useful at all. Queries granularity (that may be important for the research for which the dataset was made public initially) cannot be served.
- Abuse of Privacy. Anonymity systems are open to abuse by malicious users. An anonymous sender might be encouraged to send a hate mail in a public forum showing his ethnic bias. Sensitive information about a person can be posted anonymously to commit a smear attack. Terrorists might want to use the anonymity mechanism to communicate between themselves. Strong privacy guarantee for the end user makes the task of crime-fighting very diff cult.

## **Known Uses**
The Mix based networks [3] are based on the idea of mixing the incoming data traffic from one user with the data traffic coming from other users. Each mix has a public key which is used by the message senders to encrypt the message between the user and the mix. The mix accumulates these messages, decrypts them, optionally re-encrypts the messages, and delivers them to the subsequent node. If there is a sufficient amount of input data packet from different sources, the mix ensures that the sources cannot be linked with the data packets once the packets come out of the mix. Users should not trust only one mix. Instead, they should send their data through a cascade of mixes. In this way, weak anonymity is preserved even if some of the mixes are honest (i.e., not run by an adversary). The first widespread public implementation of mixes was produced by contributors of the Cypherpunks mailing list [4]. Then Mixmaster [5] and Babel [6] were based on the mix network idea to send anonymous emails. These systems are called remailers. Mixmaster was a Type II remailer (Cypherpunks remailers were Type I remailers). Mixminion [7] is a Type III remailer that addresses the problems of previous generations of remailers like the sophisticated flood and trickle attacks. The types associated with the remailers generate different generations of these remailers.

Onion Routing [8,9] systems are based on mixes, but they leverage the idea of mixes and add LAYERED ENCRYPTION. Onion Routing systems have better latency values than mix networks and therefore are more applicable in a web browsing scenario. Crowds [10] is another system that provides low-latency data anonymization. Its approach is different from onion routing or mix schemes. Here, every node in the network is similar and every node can either forward the data packet to another node in the network or send the request to the end server based on a probabilistic coin toss. Thus, every node is a probable suspect of being the originator. In this way, Crowds provides plausible deniability for the message sender.

Hordes [11] uses multicast routing where every responder gets every message. This provides recipient anonymity.

The Votegrity system based on by Chaum’s secret-ballot receipts [12] and the VoteHere system based on Neff’s secret shuffle algorithm [13] use the concept of mix networks for mixing the votes.

Mix zone [14, 15] is the same concept of mix networks taken into the domain of location anonymity. The mix zone concept was added with the Active Bat [16] system to provide location anonymity.

The principle of k-Anonymity [17] was introduced by Latanya Sweeney for publishing of secret data. The sensitive information in a dataset is obfuscated by replacing them with a more general information.

## **See Also**
MORPHED REPRESENTATION pattern is used with ANONYMITY SET pattern to hide the correlation between incoming and outgoing traffics.

## **References**

[1] US Department of Homeland Health Services Office for Civil Rights. (May 2003). Summary of the HIPAA privacy rule.

[2] Ryan, A. M., & Williams, R. (2003). General k-anonymization is hard. In *In Proc. of PODS’04*.

[3] Chaum, D. L. (1981). Untraceable electronic mail, return addresses, and digital pseudonyms. *Communications of the ACM*, *24*(2), 84-90.

[4] APAS Anonymous Remailer Use [FAQ 3/8]: Remailer Basics. (2001, December). Computer Cryptology. http://www.faqs.org/faqs/privacy/anon-server/faq/use/part3/

[5] Cotrell, L. (1995). Mixmaster & remailer attacks. <http://riot.eu.org/anon/doc/remailer-essay.html>. 

[6] Gulcu, C., & Tsudik, G. (1996, February). Mixing E-mail with Babel. In *Proceedings of Internet Society Symposium on Network and Distributed Systems Security* (pp. 2-16). IEEE.

[7] Danezis, G., Dingledine, R., & Mathewson, N. (2003, May). Mixminion: Design of a type III anonymous remailer protocol. In *2003 Symposium on Security and Privacy, 2003.* (pp. 2-15). IEEE.

[8] Goldschlag, D. M., Reed, M. G., & Syverson, P. F. (1996, May). Hiding routing information. In *International workshop on information hiding* (pp. 137-150). Springer, Berlin, Heidelberg.

[9] Syverson, P. F., Goldschlag, D. M., & Reed, M. G. (1997, May). Anonymous connections and onion routing. In *Proceedings. 1997 IEEE Symposium on Security and Privacy (Cat. No. 97CB36097)* (pp. 44-54). IEEE.

[10] Reiter, M. K., & Rubin, A. D. (1998). Crowds: Anonymity for web transactions. *ACM transactions on information and system security (TISSEC)*, *1*(1), 66-92.

[11] Shields, C., & Levine, B. N. (2000, November). A protocol for anonymous communication over the internet. In *Proceedings of the 7th ACM Conference on Computer and Communications Security* (pp. 33-42).

[12] Chaum, D. (2004). Secret-ballot receipts: True voter-verifiable elections. *IEEE security & privacy*, *2*(1), 38-47.

[13] Neff, C. A. (2001, November). A verifiable secret shuffle and its application to e-voting. In *Proceedings of the 8th ACM conference on Computer and Communications Security* (pp. 116-125).

[14] Beresford, A. R., & Stajano, F. (2004, March). Mix zones: User privacy in location-aware services. In *IEEE Annual conference on pervasive computing and communications workshops, 2004. Proceedings of the Second* (pp. 127-131). IEEE.

[15] Beresford, A. R., & Stajano, F. (2003). Location privacy in pervasive computing. *IEEE Pervasive computing*, *2*(1), 46-55.

[16] Ward, A., Jones, A., & Hopper, A. (1997). A new location technique for the active office. *IEEE Personal communications*, *4*(5), 42-47.

[17] Sweeney, L. (2002). k-anonymity: A model for protecting privacy. *International Journal of Uncertainty, Fuzziness and Knowledge-Based Systems*, *10*(05), 557-570.

## **Sources**
Hafiz, M. (2006, October). A collection of privacy design patterns. In *Proceedings of the 2006 conference on Pattern languages of programs* (pp. 1-13).
