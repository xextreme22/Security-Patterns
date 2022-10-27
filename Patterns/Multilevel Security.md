# **Multilevel Security**

## **Intent**
In some environments data and documents may have critical value and their disclosure could bring serious problems. The MULTILEVEL SECURITY pattern describes how to categorize sensitive information and prevent its disclosure. It describes how to assign classifications (clearances) to users and classifications (sensitivity levels) to data, and how to separate different organizational units into categories. Access of users to data is based on policies, while changes to the classifications are performed by trusted processes that are allowed to violate the policies.

## **Example**
The general command of an army has decided on a plan of attack in a war. It is extremely important that this information is not known outside a small group of people, or the attack may be a failure.

## **Context**
In some environments data and documents may have critical value and their disclosure could bring serious problems.

## **Problem**
How can you control access in an environment with sensitive documents so as to prevent leakage of information? 

The solution to this problem must resolve the following forces: 

- We need to protect the confidentiality and integrity of data based on its sensitivity. 
- Users have to be allowed to read documents based on their position in the organization. 
- There should be a way to increase or decrease the ability of users to read documents and the sensitivity of the documents. Otherwise, people promoted to higher positions, for example, will not be able to read sensitive documents, and we will end up with a proliferation of sensitive and obsolete documents.

## **Solution**
Assign classifications (as clearances) to users and classifications (as sensitivity levels) to data. Separate different organizational units into categories. For example, classifications may include levels such as ‘top secret’, ‘secret’ and so on, and categories may include units such as engDept, marketingDept and so on. For confidentiality purposes, access of users to data is based on policies defined by the Bell-LaPadula model [1], while for integrity the policies are defined by Biba’s model [2]. Changes to the classifications are performed by trusted processes that are allowed to violate the policies of these models.

## **Structure**
Figure 163 shows the class diagram of the MULTILEVEL SECURITY pattern. The UserClassification and DataClassification classes define the active entities and the objects of access respectively. BothXXX classifications may include categories and levels. Trusted processes are allowed to assign users and data to classifications, as defined by the Assignment class.

![](./Images/multilevel_security_structure.png)

*Figure 163: Class diagram for the MULTILEVEL SECURITY pattern*

## **Dynamics**

## **Implementation**
Data classification is a tedious task, because every piece of information or document must be examined and assigned a classification tag. New documents may get automatic tags based on their links to other documents. User classifications are based on users’ rank and units of work and are only changed when they change jobs. It is hard to classify users in commercial environments in this way: for example, in a medical system it makes no sense to assign a doctor a higher classification than a patient, because a patient has the right to see their record.

## **Example Resolved**
The group involved in planning attacks, as well as all the related documents it produces, are given a classification of ‘top secret’. This will prevent leakage towards lower-level army staff.

## **Consequences**
The MULTILEVEL SECURITY pattern offers the following benefits: 

- The classification of users and data is relatively simple and can follow organization policies. 
- This model of the pattern can be proved to be secure under certain assumptions [2]. 
- The pattern is useful to isolate processes and execution domains. 

The following potential liabilities may arise from applying this pattern: 

- Implementations should use labels in data to indicate their classification. This assures security: if not done, the general degree of security is reduced. 
- We need trusted programs to assign users and data to classifications. 
- Data must be able to be structured into hierarchical sensitivity levels and users should be able to be structured into clearances. This is usually hard, or even impossible, in commercial environments. 
- This model can handle only secrecy and prevention of leakage of information. A dual model is needed to also handle integrity. 
- Covert channels may break the assumed security

## **Variants**
It is possible to define a similar pattern to control integrity in multilevel models according to the Biba rules [1].

## **Known Uses**
This pattern has been used by several military-sponsored projects and in a few commercial products, including DBMSs (Informix, Oracle) and operating systems (Pitbull [3] and HP’s Virtual Vault [4]).

## **See Also**
The concept of roles can also be applied when implementing this pattern, role classifications replacing user classifications.

## **References**

[1] Gollmann, D. (2010). Computer security. *Wiley Interdisciplinary Reviews: Computational Statistics*, *2*(5), 544-554.

[2] Summers, R. C. (1997). *Secure computing: threats and safeguards*. McGraw-Hill, Inc..

[3] Argus Systems Group. (n.d.). Trusted OS security: Principles and Practice. Retrieved October 27, 2012, from <http://www.argus-systems.com/Products/products.html> 

[4] Hewlett Packard Corporation. (n.d.). Virtual Vault. <http://www.hp.com/security/products/virtualvault> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
