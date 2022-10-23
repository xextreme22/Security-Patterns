# **History-Based Authentication**

## **Intent**
Authentication based upon the capability of an entity to satisfactorily account for its history. To be successfully authenticated, an entity should be able to answer a set of questions regarding its history. The structure of the inquiry could range from simple question-answer interchange to an exhaustive, extraordinarily complex interrogation.

## **Example**
This example sketches the THOFU Security System, hereinafter referred to as TSS. The system has been designed within the framework of the project THOFU, a multidisciplinary collaboration intending to perform a prospective study on technologic concepts and solutions enabling advanced services in the context of the hotels of the future [1]. One of the main research areas of THOFU is related to security. Predictably, monitoring of comfort, safety and security of hotels and many other social and industrial environments of the future will be assigned to pervasive systems. Such systems, operating with little or no human intervention, should in turn control and protect themselves to assure their correct performance and their compliance with requirements such as security. An essential security requirement in such a system is to verify the identity of entities intending to perform tasks or to access resources, i.e., to authenticate them. 

The system is composed of entities that can be categorized into several types, according to their roles:

- robots, responsible for the surveillance of the facilities and resources of the organization. Robots may be grouped into patrols. 
- controllers, responsible for monitoring the performance of other entities (e.g., robots and other controllers). In case an asset (resource or capability) is unavailable or underperforms, they trigger a corrective action. Controllers may be in turn controlled by other entities.

Next, we present some examples of events of the lifecycle of the system. Let R1, R2, R3 and R4 be robots and C1, C2 controllers:

1. At 01/11/2013 07:30:00, the control of R1 is assigned to controller C1.
1. A new patrol, identified as P1, controlled by C1 and composed of R1, R2, and R3, is created at 01/11/2013 09:00:10. The patrol components are informed about this. 
1. At 04/12/2013 09:45:00 C1 notifies R1, R2, and R3 that a new entity, R4, joins the patrol since that time. 
1. R1 communicates C1 that sensor S0 11 has reported a temperature above 50o C at 06/12/2013 10:45:00 in room B and proceeds to probe into the incident. 
1. At 01/10/2014 07:30:00, the control of R1 is transferred to controller C2. 
1. At 01/10/2014 07:30:01, C2 issues R1 the order: “You must send me a greeting signal on security time intervals δt” (the intention of C2 is to maintain continuous contact with R1). 
1. At 10/01/2014 23:05:30 the stipulated security time interval δt expires, and C2 receives no greeting signal from R1 (Up to this moment, greeting signals had been periodically received in time). 
1. At 10/01/2014 23:35:30 C2 receives a greeting signal from an entity R which claims to be R1.

Thirty minutes is time enough for an attacker to capture R1 and replace it by an impostor. How can C2 be sure of the identity of R?

## **Context**
A dynamical system comprising uniquely identifiable artificial entities, endowed with processing, communication, and memory capabilities, such as robots or smart objects. The purpose of the system is to provide a service to an organization (e.g., security or logistic services). Moreover, the system is responsible for assuring its own security and monitoring its own performance. 

An essential feature of the system is its dynamism. The system evolves over time, changing from one state to another, which gives rise to the system history. Two highly desirable properties of such a system are self-management (i.e., minimizing human intervention) and traceability of entities and processes. Self-management implies monitoring over time the performance of the system entities in order to allow early reaction in case of misbehavior or underperformance. Traceability can be used for suggesting process improvements, strengthens confidence in the entities and allows accounting for the provenance of valuable assets.

## **Problem**
For the sake of security, it must be assured that only legitimate entities are allowed to accomplish tasks related to the service and to access any kind of resource from the system. For this reason, the entities must be properly authenticated, that is, they must prove that they are what they claim to be. The system must be protected from attackers trying to impersonate legitimate entities. Different types of attack are possible, ranging from stealing identification and authentication information to cloning the entity. The realization of such threats could result in harms such as privacy loss, misuse of the system for malicious purposes, unreliability of the system, or system disablement.

The following forces apply: 

- What the claimant has may be lost, damaged, stolen or cloned. To have deeper insight into the weaknesses of particular solutions, such as smart cards, we refer the reader to [2]. Moreover, due to costs or technical reasons, it may not be feasible to provide non-human entities with such devices. 
- Though promising, the existing techniques aiming to authenticate artificial entities through biometric-like features (see, e.g., [3] or [4]), are not yet mature or widely applicable. 
- The information required for authentication must be difficult to steal or forge. 
- As a result of previous force, one static password is not sufficiently secure: it may be guessed by an attacker or discovered by eavesdropping. A set of passwords, possibly changing over time, would provide an extra level of security. But an entity should focus on its performance: generating new authentication information and communicating it to authentication authorities should not be a distraction from its work.
- The more comprehensive authentication is, the more difficult it becomes to circumvent it. If situation requires so (e.g., when trying to access critical resources) authentication should be as comprehensive as possible. 
- Proportionality requirement: the resources assigned to a task should be proportional to its frequency and criticality level. Different contexts call for different levels of rigor in the authentication process. 
- The entities responsible for authentication should not be able to impersonate an entity that has been authenticated by them (i.e., inside attacks should be prevented). 
- Error and tampering threats: data corruption, malicious injection of wrong data or illegal manipulation of data stored by an entity should be detected early, if possible before providing the entity with access to the required resources. While this force is not strictly related to authentication, an authentication mechanism checking the integrity of data would provide an added value.

## **Solution**
Base authentication upon the entity’s history.

Forces F1 and F2 suggest ruling out two authentication factors (what an entity has, what an entity is) and focus on the remaining one: what an entity knows. The system has a history, which grows and becomes more complex as the system evolves. The enhanced capabilities of the entities allow them to be aware of their history and process queries related to it. Taking advantage of this fact, authentication can be based upon the entity’s history. To be successfully authenticated, an entity should be able to answer a set of questions regarding its history. The structure of the inquiry could range from simple question-answer interchange to an exhaustive, extraordinarily complex interrogation (e.g., before providing the entity with access to a critical resource).

Some entities are specialized as authenticators. To exercise their role, they must be provided with adequate authority and capabilities, as well as knowledge about the history of the entities to be authenticated. In particular, the authenticator should be able to increase such knowledge by aggregating information regarding the entity provided by other relied-upon entities. In this way authentication benefits from the collective memory.

## **Structure**
As previously stated, we consider a system of uniquely identifiable entities endowed with enhanced capabilities. This fact has been represented in the UML class diagram of Figure 24 with the class Entity, which has an attribute (Identity) and several methods. We restrict ourselves to the methods that model the capabilities supporting our mechanism such as communicate, which represents the communication capability of entities.

We understand the history of an entity as a set of elementary components, which are in turn modelled by the notion of occurrence. An occurrence is defined as something (an event, an incident or something else) that happens to one or several entities during a period of time. In our example, each of the events described in subsection 12.2 is an occurrence. We are aware that there is an even simpler concept defined as something that happens, without referring to any entity. This simpler concept is similar to the notion of event defined in the glossary [5] as “anything that happens or is contemplated as happening”. However, because our goal is to manage the entities’ history, our atomic concept will be occurrence. Our current concept of occurrence generalizes the one we introduced in [6].

An entity builds up its history by registering the occurrences in which it is involved. Specialized types of occurrences can be derived from the basic notion. A type especially relevant to our proposal is Communication, which models the registration of a communication act. The registered occurrence is “an entity has taken part in an act of communication whose content is the communicatedObject”. Two associations between Communication and Entity are used to indicate the role that each entity plays (sender or receiver).

In particular, an entity can communicate with another one of its occurrences so that, in this case, the communicated object is an occurrence. As a consequence, an entity can acquire (at least a partial) knowledge of the history of other entities; this feature is highly relevant for our authentication mechanism.

Two different types of entities can be differentiated in the authentication process, the entity that has to be authenticated (authenticable entity) and the entity that authenticates it (AUTHENTICATOR).

An authenticable entity is an entity capable of recording, recognizing and reporting on its own history. In our proposal, the history of an entity enables it to get authenticated. Note that, because a history grows over time, it becomes more and more complex (in size and complexity) and thus more and more difficult to steal or forge. The strength of our mechanism is thus an increasing function of time. An authenticable entity should be able to generate an output in response to such an input. This output, hereinafter referred to as the proof of identity (token authenticator, according to [7]), proves that the entity knows the correct answers and serves to authenticate it to the system. The simplest case of a proof of identity is just the concatenation of the answers to the posed questions. Proofs of identity may be strengthened by further processing the answers through computation methods such as hashing functions, encryption with secret keys or zero knowledge proof techniques. We remark that an authenticable entity is inextricably linked to its history: if dissociated from it, the entity is no longer able to generate a proof of identity and thus ceases being an authenticable entity. An example of a history-aware entity would be a smart thing such as a spime [8], defined as a new category of space-time objects that are aware of their surroundings and can memorize real-world events; if provided with the capability to generate proofs of identity, a spime would be an authenticable entity.

On the other hand, an authenticator (verifier, according to [7]) is an entity endowed with both the necessary capabilities and the authority to authenticate other entities. To exercise its role, it must have access to the history of the entity to be authenticated. Such knowledge may be direct (derived from its own history) or communicated by other trusted entities.

We note that specialization of Entity into AuthenticableEntity and Authenticator is incomplete and overlapping. Incompleteness permits the existence of other sorts of entities in the system, such as sensors. In addition, overlapping allows an authenticator to be, in turn, authenticated by another entity.

Our concrete proposal can be seen as a specialization of the Authenticator Abstract Security Pattern proposed in [9] since our proposal includes the Abstract Authenticator classes plus several other classes specific to our approach. Specifically, the Authenticator and Proof of identity classes of the abstract pattern appear explicitly in our proposal, the Subject class corresponds to our AuthenticableEntity class, and the Authentication Information class corresponds to the history associated to the authenticator.

![](./Images/history-based_authentication_structure.png)

*Figure 24:  History–based Authentication Class Diagram.*

## **Dynamics**
There are two actors involved in an authentication process: an entity claiming to be AE and an authenticator Aut. The steps of the authentication process are described below and shown graphically in Figure 25.

1. The authenticable entity AE identifies itself to the authenticator Aut by means of the method communicate (Aut, identity). 
1. To authenticate AE, Aut first must possess sufficient knowledge about the history of AE. Aut can rely on its current knowledge or gather information from trusted entities (i.e., demand information on AE to a trusted entity T E through requestInformation(T E, AE) and receive a set of occurrences regarding AE through communicate(V, Set(O)) ). Depending on the centralized/distributed architecture of the system, such trusted entities could be a central repository, a set of credential service providers (whose credentials would authoritatively bind the identity of an entity and a subset of its history), or just trusted peers. 
1. Based upon the collected knowledge, Aut generates a set of questions Set(Q) by invoking generateQuestions (AE). 
1. Such questions are posed to AE: communicate (AE, Set(Q). 
1. AE, in turn, generates a proof of identity (generateResponse (Set(Q))) and 
1. returns it to Aut (communicate (Aut, Response)). 
1. After validating the received proof of identity (validate (AE,Set(Q), Response)), Aut may reach a definitive conclusion on the authenticity of AE’s identity or pose new questions, possibly after gathering further information from its trusted parties. To meet the proportionality requirement, the thoroughness of the questions posed during the interrogation depends on the criticality of the information/services/location the entity intends to access and the cause that triggered the authentication process. Some of the new questions can be made depending upon the answers formerly provided by AE (e.g., questions can be raised to clarify an ambiguous answer or to obtain more detailed answers). 
1. Finally, Aut reports on the success or failure of the authentication process to AE (communicate (AE, result)) . If the claiming entity’s answers are right, the authentication is positive. Otherwise, if the claiming entity fails to prove its knowledge about the history of AE, the authentication is negative.

![](./Images/history-based_authentication_dynamics.png)

*Figure 25: History–based Authentication Sequence diagram.*

## **Implementation**
In order to implement the pattern, authenticable entities must first be registered within the system. A registrar, on behalf of the system, issues an identity certificate to each new entity. As a consequence, an identity management model is needed, and unique names should be assigned to entities. Furthermore, if a new entity has a history prior to entering the system, the registrar should validate the identity proofs provided by the entity

During the authentication process an entity AE must not leak information about its occurrences to non-authorized parties such as eavesdroppers (history secrecy). Moreover, an eavesdropper should not be able to trace the history of the interactions between AE and the system. In particular, the link between AE and its communications (i.e., individuating the communications where AE has taken part) should not be established with a reasonable effort (unlinkability). A possible solution is to encrypt communications between authenticable entities and authenticators. Moreover, for the sake of performance, the answers provided by an authenticable entity may be hashed.

To perform its task, an authenticator does not need to know the whole history of an authenticable entity. Care must be taken to assure that an authenticator doesn’t have such a thorough knowledge (which would allow him to impersonate the entities he is supposed to authenticate).

According to the proportionality requirement, the complexity of each realization of authentication must be proportional to the criticality of the circumstances. To meet this requirement both the different possible circumstances and authentication complexity levels should be categorized. Circumstances should be classified depending on the criticality of the information/services/location the entity intends to access. Furthermore, authentication complexity levels should be defined by establishing the thoroughness of the questions posed during the interrogation. Moreover, a policy is required to assign minimum and standard authentication complexity levels to each circumstance.

Persistence structures for storing the occurrences must be set up, as well as a language for communicating occurrences (including its syntax and semantics).

## **Example Resolved**
Each of the events described in subsection 12.2 is an occurrence and has been recorded by the entities involved in it. Robots are required to identify and authenticate themselves to the system, thus they are authenticable entities, as described in Section 12.7. Because controllers should be able to authenticate the entities under their control, they are a type of the authenticators described in Section 12.7.

To authenticate R, C2 asks its trusted entities if any of them has ever worked with R. Among others, C1 answers affirmatively. C2 asks C1 further details of the history of R1. C1 signs a credential binding the identity of R1 to the set of occurrences (1) to (4) (see subsection 12.2) and sends it to C2. For the sake of security, the credential can be encrypted using C2’s public key. Provided with such information, C2 asks R1 “What temperature did you report at 06/12/2013 10:45:00?”. AE provides the right answer. C2 goes on asking: “When was such temperature measured? Who was your controller in that occasion? Who were your patrol partners?”. R1 fails to answer satisfactorily the last set of questions. C2 reacts to this situation by putting AE into quarantine and transferring the problem to a specialized subsystem that, after performing in–depth hardware and software testing on AE, decides the subsequent step (e.g., repair or retire R2).

## **Consequences**
Authentication is reinforced. Confidentiality, integrity, authenticity, accountability, and non-repudiation are strengthened over time as the history increases. The system has been endowed with an authentication mechanism that exploits the knowledge about its history. To get authenticated, an entity C claiming to be AE needs to prove an up–to–date knowledge of the history of AE. Failing to provide a satisfactory proof of identity, based on the answers to the questions posed by the authenticator, is a sign that there is something wrong with C: inability to account for its own history may be due to a communication error (either in the reception of the request made by Aut or in the transmission of the message), memory corruption (C is really AE, but has forgotten part of its history) or the entity being an impostor impersonating AE.

Benefits: 

- Is fully applicable to artificial entities, provided they have sufficient storage and computational capacities. 
- Enforces the prevention of impersonation. Even though an attacker has forged the identity certificate of an entity, it still must capture a significant part of the history of the entity. As time goes by, this becomes an increasingly overwhelming task. This consequence matches force F3. In particular this can be applied to authenticators: since a single authenticator do not know the complete history of an entity AE and, moreover, does not know in advance what questions may be posed to AE by another authenticator, insider attacks are prevented. This matches force F7. 
- Resorts to information that comes out naturally with the entity performance. It does not require an extra effort to generate such information. This consequence matches force F4. 
- Allows dynamism and flexibility: the authenticator can choose in any moment which and how many questions an entity must answer to be authenticated. The number and nature of questions can be adapted to the criticality of the resource an entity intends to access or to its observed behavior. If an intrusion detection system raises suspicion about an already authenticated entity, a more exhaustive authentication process may be triggered. This consequence matches forces F5 and F6. 
- Provides a basis for error tampering detection: inability to provide satisfactory answers to questions related to history may be an indication of integrity loss. This consequence matches force F8. 
- Allows collaboration: an authenticator can complement its knowledge on the entity’s history by consulting other trusted ‘colleagues’. Such a feature is highly relevant in decentralized environments such as the IoT.

Liabilities:

- Need for storage and some computational capability in each entity. The storage resources needed increase as time goes by. 
- Manageability: The overhead required to maintain the history of the entities implies a larger impact on manageability than other, simpler authentication technologies, such as passwords. For this reason, a trade-off between manageability and the required security level should be considered. Being aware of this issue, we establish the requirement of proportionality. 
- Performance: Performance is affected by the need to store and manage the history of the entities. In general, choosing more secure procedures will generally have an adverse effect on performance. 
- Cost: Costs will be incurred from the additional processing power required and the additional management overhead. However, this investment could cost less in the long term because the system will be less vulnerable to attacks, which are inherently expensive to detect and recover from.

## **Known Uses**
The idea of benefiting from the knowledge about an entity’s history to verify its identity is not new. It has been extensively applied to persons over time. Moreover, such an idea underlies several solutions proposed in the literature. For example, in [10], mobile sensors in a wireless network authenticate each other based on the history of their previous contacts. In [11] a history-based mechanism has been proposed for robot authentication: a robot builds its own personal profile, by tracing selected data regarding the robot’s sensors and actuators. The robot communicates such a profile to a Trusted Authority TA, that can make use of its knowledge of the personal profile in future authentications. In [12], a person is authenticated based on the history of his interactions with his smartphone. 

We have followed this approach in two development projects. AWHS Project [6] aimed to support the collaborative management of biobanks collecting, storing, processing, and lending biological samples. In order to satisfy the requirement for traceability, the objects (biological samples and their aliquots) were augmented with their history, to which they remained inextricably linked. If an object is dissociated from its history and it is not possible to assure its provenance, it ceases to be acceptable for the purposes of the biobank. The history of an object was defined as the set of all occurrences related to it: how it was created, what changes it has experienced, what other entities it has been related to, what processes it has been subject to (e.g., sample aliquoting, box storage) and what protocols have followed such processes. The ability to build up the whole history of an object allowed to set up a “chain of custody” (the object provenance). Among other outcomes, it would allow to undoubtedly assure the object’s identity, i.e., to authenticate it.

Project THOFU is a multidisciplinary collaboration intending to perform a prospective study on technologic concepts and solutions enabling advanced services in the context of the hotels of the future [1]. One of the main research areas of THOFU is related to security. A pervasive security system was designed to operate in a self-managed way. The system benefits from the knowledge about its own history to assure its correct performance and its compliance with requirements such as privacy and security.

## **See Also**
Our pattern is related to the AUTHENTICATOR Pattern, which aims to authenticate distributed objects. Furthermore, our concrete proposal is a specialization of the Authenticator Abstract Security Pattern [9].

## **References**

[1] Thofu: Technologies of the hotel of the future. (2013). Thofu. Retrieved January 2014, from https://thofu.es/

[2] Madhusudhan, R., & Mittal, R. C. (2012). Dynamic ID-based remote user password authentication schemes using smart cards: A review. *Journal of Network and Computer Applications*, *35*(4), 1235-1248.

[3] Suh, G. E., & Devadas, S. (2007, June). Physical unclonable functions for device authentication and secret key generation. In *2007 44th ACM/IEEE Design Automation Conference* (pp. 9-14). IEEE.

[4] Cobb, W. E., Laspe, E. D., Baldwin, R. O., Temple, M. A., & Kim, Y. C. (2011). Intrinsic physical-layer authentication of integrated circuits. *IEEE Transactions on Information Forensics and Security*, *7*(1), 14-24.

[5] Luckham, D., & Schulte, R. (2011). Event processing glossary-version 2.0. Real Time Intelligence & Complex Event Processing. Retrieved May 2014, from https://complexevents.com/2011/08/23/eventprocessing-glossary-version-2-0/

[6] Domínguez, E., Pérez, B., Rubio, Á. L., Zapata, M. A., Lavilla, J., & Allué, A. (2013). Occurrence-oriented design strategy for developing business process monitoring systems. *IEEE Transactions on Knowledge and Data Engineering*, *26*(7), 1749-1762.

[7] Burr, W. E., Dodson, D. F., & Polk, W. T. (2011). *Electronic authentication guideline* (pp. 800-63). US Department of Commerce, Technology Administration, National Institute of Standards and Technology.

[8] Sterling, B., & Things, S. (2005). Shaping Things. The MIT Press.

[9] Fernandez, E. B., Yoshioka, N., Washizaki, H., & Yoder, J. W. (2014, April). Abstract security patterns for requirements specification and analysis of secure systems. In *WER*.

[10] Zhu, W. T., Zhou, J., Deng, R. H., & Bao, F. (2012). Detecting node replication attacks in mobile sensor networks: theory and approaches. *Security and Communication Networks*, *5*(5), 496-507.

[11] Adi, W. (2009, August). Mechatronic security and robot authentication. In *2009 Symposium on Bio-inspired Learning and Intelligent Systems for Security* (pp. 77-82). IEEE.

[12] Das, S., Hayashi, E., & Hong, J. I. (2013, September). Exploring capturable everyday memory for autobiographical authentication. In *Proceedings of the 2013 ACM international joint conference on Pervasive and ubiquitous computing* (pp. 211-220).

## **Sources**
Ciria, J. C., Domínguez, E., Escario, I., Francés, Á., Lapeña, M. J., & Zapata, M. A. (2014, July). The history-based authentication pattern. In *Proceedings of the 19th European Conference on Pattern Languages of Programs* (pp. 1-9).
