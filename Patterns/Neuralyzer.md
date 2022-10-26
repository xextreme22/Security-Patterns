# **Neuralyzer**

## **Intent**
Describe how to manage the right to be forgotten in Big Data environments. Usually, this right is asked by the data subject, which is the person who wants that the system “forgets” his data. However, there are some scenarios where this action should be done automatically by the system.

## **Example**

## **Context**
Generally, Big Data ecosystems have a huge amount of personal and sensitive data stored in different databases and storage systems, supported by a variety of utility software.

## **Problem**
The right to be forgotten, also known as the right to erasure, is a feature that suggests that it is necessary to find a way to effectively delete data from storage systems. Erasing an individual’s data implies to find and delete all his data; possibly scattered in several places. This functionality gets even more complicated in Big Data contexts where not only personal data is stored, but also information inferred from it due to the use of analysis techniques like business intelligence or machine learning.

The solution is constrained by the following forces:

- The gap between regulators and technology. There is a huge gap between the good intentions of the regulators and the complexity of real Big Data environments. It is necessary that our pattern meets both necessities.
- Flexibility. Big Data ecosystems can be used in different scenarios from a hospital to a factory. Different kinds of contexts require different solutions.
- Value. Probably, one of the main characteristics of a Big Data system is the value that can be obtained from this data. Due to the application of the RTBF, this characteristic can be compromised; erasing records may affect the obtained value from the analytics.
- Access Control. Due to the importance of the operation, any erasure or anonymization must be performed by order of the CDO: who will manage all the requests made by the data subjects, and who is the only role authorized to perform the data changes.

## **Solution**
Our solution focuses on an architecture pattern. Hence, it defines an architecture where the data subject demands the deletion of his data from all the databases of the ecosystem. It is important to highlight that the deletion of the data must be authorized by the data officer. Then, an entity called Neuralyzer will delete all the data related to the data subject. In order to do that, it will use different techniques, for example, data masking of the data. These decisions must be taken by considering the context of the system and how it can affect the performance of the analytics. Different scenarios and organizations need different solutions, so the decision about which data can be deleted from the system is a complex process in which the CDO together with the top management of the company must consider all possibilities and thereby decide. Further details of the solution will be explained in the following subsections.

## **Structure**
Figure 91 depicts the class model for this pattern. The Subject and the Chief Data Officer represent two interfaces where active entities are authenticated by the Server using an Authenticator Service. Once they are authenticated, they have different objectives. On one hand, the subject makes a request to delete all his personal data from the system. On the other hand, the data officer receives from the server the notification and starts the process to comply with the RTBF. The Neuralyzer is the main class in this pattern, its main goal is to decide which technique should be used depending on the context and the data stored from the user. There are three main categories of techniques represented by three different sub-classes: Anonymization, Pseudo-anonymization, and Deletion. Once the Storage is properly modified to comply with the regulation the system should notify the subject that the erasure has been completed. All the operations performed on the data must be recorded in a log file using a SECURITY LOGGER AND AUDITOR. Table 10 summarizes the main components of the pattern and gives a brief explanation of its purpose.

![](./Images/neuralyzer_structure.png)

*Figure 91: Class model of the NEURALYZER pattern*

|**Name**|**Type**|**Description**|
| :-: | :-: | :-: |
|Role|Meta-subject|Generalization of the subjects that can perform actions in the pattern.|
|- Requester|Subject|Subject requesting that their data be erased from the system. They initiate the use of the pattern.|
|- Chief Data Officer (CDO)|Subject|CDO is responsible for a company's data at the highest level, both from a technological and business point of view, including security. This role adds the forgetting rules into the system based on the context of the company.|
|Server Interface|Software|Interface that shows all the actions provided by the server.|
|Server|Software|Server provides the actions related to the RTBF depending on the role that accesses. It acts like a bridge between the roles and the NEURALYZER pattern.|
|AUTHENTICATOR|Security pattern|Auxiliary security pattern used to provide authentication to the users.|
|Authorizer|Security pattern|Auxiliary security pattern used to give specific permission to each role.|
|Forgetter|Software|The main class of the NEURALYZER pattern. Based on the forget rules implemented by the CDO and the data that should be forgotten, it applies the forget techniques on the storage system. Some cases require to use different techniques at the same time.|
|- Anonymization|Software|A mechanism to perform the NEURALYZER. Data anonymization is a technique that refers to hiding identity and sensitive data for owners of data records (Zhang, Yang, Liu, & Chen, 2014).|
|- Pseudoanonymization|Software|A mechanism to perform the NEURALYZER. Data pseudo-anonymization substitutes the identity of the user in such way that additional data is needed to re-identify the data user. In some cases, anonymization is enough to comply with the regulations.|
|- Deletion|Software|A mechanism to perform the NEURALYZER. Deletion of the subject’s data from all the different data sources along the Big Data ecosystem.|
|Storage|Software|A collection of data that allows it to be accessed, managed, and updated. In Big Data ecosystems, there are typically three ways of storage data: structured, semi-structured, and non-structured data.|
|- Structured|Software|The structured storage can be considered as the traditional relational databases, for example, MySQL or PostgreSQL; usually, in this kind of stores, they use an SQL similar language to make queries to the data.|
|- Non-structured|Software|The non-structured stores, usually known as NoSQL databases, are widely used in Big Data ecosystems. In this kind of stores, there are four different subtypes that can be identified: graph based (usually used to represent data from social networks; for example, neo4j), columnar (in these stores each key is associated with one or more attributes, unlike the relational databases; for example, HBase or Cassandra), documental (data is stored with a document form, its main advantage is the scalability; for example, MongoDB or CouchDB), and key-value (they use a similar hash table style where each key is associated with a set of values; for example, Apache Accumulo or Riak).|
|- Semi-structured|Software|A way of storing data that is neither raw data, nor strictly typed as relational database systems; for example, JSON format can be considered as a semi-structured data format.|
|SECURITY LOGGER AND AUDITOR|Security pattern|A mechanism to perform the NEURALYZER. Most of the time, the application of the RTBF implies the management of sensitive personal information, for that reason, it is important to keep track of all operations performed in this process.|

*Table 10: Components of the NEURALYZER pattern*

## **Dynamics**
Figure 92 shows the use case “Forget a user”, which corresponds to the sequence: 

1. Requester requests to be forgotten from the system. 
1. Requester is asked for authentication. 
1. Requester authenticates in the system. 
1. ` `Server receives proof of authentication. 
1. Server initiates the Forgetter. 
1. Forgetter queries the Storage systems to receive all the data from the requester. 
1. Forgetter receives the data related to the requester. 
1. Forgetter decides which technique will use depending on the data retrieved and the rules created by the Chief Data Officer. 
1. In this case, an anonymization technique is used to forget the data from the user. 
1. The anonymization technique is used in the Storage systems. 
1. The server notifies the involved users.

![](./Images/neuralyzer_dynamics_1.png)

*Figure 92: Sequence diagram for the use case “Forget a user”*

This scenario can be considered as the “ideal case” in which the data is easily found, and the forgetting rules are perfectly designed. Although this pattern is supposed to work automatically without human interaction, in some cases the CDO will be asked to introduce new rules that meets these characteristics. Those new rules will be added to the system, so they can be reused in the future. This scenario is depicted in Figure 93.

![](./Images/neuralyzer_dynamics_2.png)

*Figure 93: Alternative sequence diagram*

## **Implementation**
A possible implementation is shown in Figure 94. In this object diagram, we have a scenario with the two roles involved: the requester (who initiates the request to delete their data), and the Chief Data Officer (who is the person in charge to manage the deletion of the data). We have also added the role of data scientist, which is normally present in this type of system, to show that in this particular case it does not have any kind of right over the data. Furthermore, in this scenario, we have a Big Data system that has a storage system based on HDFS (Hadoop Distributed File System).

![](./Images/neuralyzer_dynamics_3.png)

*Figure 94: Example of use of the NEURALYZER pattern*

Also, this is a scenario where there is no need to fully eliminates all the data; instead of doing that it is enough to apply anonymization (for example, k-anonymization) or pseudo-anonymization (for example, data masking) techniques on the data. This example tries to highlight that the pattern depends on the context and regulations that can affect the Big Data environment. It is also affected by the implementation features of the system, for example, the use of HDFS.

## **Example Resolved**

## **Consequences**
Advantages derived from the use of this pattern include the following:

- The gap between regulators and technology. The appropriate use of different techniques can reduce this gap. However, it is still important to improve training on these topics for regulators and IT people.
- Flexibility. The pattern includes three ways to face the right to be forgotten problem. Hence, it can be adapted to different contexts.
- Access Control. We can use RBAC to control actions on the databases and restrict access to only the CDO role.

However, the implementation of this pattern can have some liabilities:

- Value. Each time any record of the data being analyzed is deleted, it is highly likely that the results will be affected and vary from the previous time.

## **Known Uses**
- In [1] the authors explain the main approaches to deal with the right to be forgotten in Artificial Intelligence environments. These solutions can be classified in three categories: anonymization, pseudo-anonymization, and deletion of the data. They also conclude that all these solutions have a relevant impact in the quality of the analysis. Furthermore, in [2] the author also highlights that the best way to comply with the right to be forgotten is the anonymization of the data. For that purpose, he shows some examples related to the GDPR regulation. We created an entity named “NEURALYZER” that generalizes the main kinds of solutions used to comply with the right to be forgotten.
- BankingHub (3). Explains a specific scenario where the right to be forgotten is applied in a bank context. In this case, they tag the different types of data stored and depending on that, they assign a retention period for the data; for example, the migration probability and the hobbies of the subject must be deleted immediately, while the account transactions must remain in the system 10 years after the account gets inactive. This is an example of how the data cannot be deleted by demand only, but also, because of the requirements of the context where the Big Data system performs its operations.

## **See Also**
- AUTHENTICATOR. When an active entity like a user or system (subject) identifies itself to the system, how do we verify that the subject intending to access the system is who it says it is? Present some information that is recognized by the system as identifying this subject. After being recognized, the requestor is given some proof that it has been authenticated. This pattern is used to authenticate the Requester and the Chief Data Officer in the system.
- ROLE-BASED ACCESS CONTROL pattern. Describe how to assign rights based on the functions or tasks of users in an environment in which control of access to computing resources is required. The RBAC pattern is needed to establish the rights that the different roles have over the data.
- SECURITY LOGGER AND AUDITOR. How can we keep track of user’s actions in order to determine who did what and when? Log all security-sensitive actions performed by users and provide controlled access to records for Audit purposes. Deleting individual data is a very sensitive operation that must be registered in a log file.

## **References**

[1] Villaronga, E. F., Kieseberg, P., & Li, T. (2018). Humans forget, machines remember: Artificial intelligence and the right to be forgotten. *Computer Law & Security Review*, *34*(2), 304-313.

[2] Mohammed, J., Khan, C. (2018). Big Data Deidentification, Reidentification and Anonymization. Retrieved from <https://www.isaca.org/Journal/archives/2018/Volume-1/Pages/big-data-deidentification-reidentification-andanonymization.aspx>

[3] GDPR deep dive—how to implement the ‘right to be forgotten.’ (2017, November 15). Retrieved June 5, 2018, from <https://www.bankinghub.eu/banking/finance-risk/gdpr-deep-dive-implement-right-forgotten>

## **Source**
Moreno, J., Fernandez, E. B., Fernandez-Medina, E., & Serrano, M. A. (2018, October). Neuralyzer: a security pattern for the right to be forgotten in big data. In *Proceedings of the 25th Conference on Pattern Languages of Programs* (pp. 1-9).
