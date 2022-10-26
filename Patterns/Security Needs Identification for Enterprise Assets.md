# **Security Needs Identification for Enterprise Assets**

## **Intent**
This is the root pattern for all enterprise security concerns. It helps resolve the issue of whether security is really needed and, if it is, what properties of security should be applied for a particular enterprise. Security properties considered include confidentiality, integrity, availability, and accountability.

## **Example**
A new wing of a museum of gemstones is to be opened. The museum has significant previous experience handling gems, and theft is a large enough risk for the museum to want protection from the unauthorized removal of any gems. The museum also has information about the collections, and employee information, that should be protected from damage or deletion, and in some cases should be kept confidential. How can the museum determine the assets that need security protection, and which types of protection?

## **Context**
An enterprise considers security as a significant non-functional requirement. Key business factors and assets of the enterprise are understood.

## **Problem**
An enterprise that considers security to be important must plan for appropriate security in accordance with the overall enterprise business plans. An enterprise may need to address legacy security plans and policies for the enterprise or develop completely new ones. The same will apply to any information technology (IT) systems that are major assets of the enterprise. For the IT systems, the enterprise may need to adopt an existing security architecture or specify a new target architecture. To determine the most appropriate security to select and implement, the enterprise must establish its validated security needs. 

How can realistic enterprise security needs be explicitly identified? 

The resolution of this problem is strongly intertwined with the environment of the enterprise, which consists of the following forces:

- The enterprise needs to comply with laws and regulations, such as privacy laws 
- It needs to handle sensitive information in a way that protects confidentiality 
- It must comply with its own existing policy, especially any security policies 
- It needs to provide sufficient protection for mission-critical business assets 
- It must ensure that the security employed has minimum potential impact on business efficiency and efficacy—that is, it does not protect more than is necessary 
- It must know when undesired events occur 
- It must be able to recover from undesired events 
- Overall costs need to be minimized

## **Solution**
Systematically and explicitly identify the types of business assets that need protection and determine the types of protection they need. This activity is typically performed by an enterprise architect or strategic planner, and includes five steps: 

1. Identify the business assets of the enterprise: 
   1. Information or data assets such as personnel and financial data 
   1. Physical assets such as personnel and buildings 
1. Identify business factors that influence the security protection needs of assets, both external and internal to the enterprise: 
   1. Laws and regulations, such as privacy laws: see [1,2,3,4] 
   1. Enterprise partner relationships 
   1. Enterprise mission, goals, and objectives 
   1. Desire for strong enterprise financial health 
   1. Business processes, such as accounting and ordering processes 
   1. Sensitive business events, such as the monthly payroll processing 
   1. Locations at which business processes and events occur
1. Determine which assets relate to which business factors. Examples include: 
   1. A privacy law may apply to employee data 
   1. Certain physical asset types may exist only at certain business locations 
   1. Selected financial data may need to be shared with an enterprise partner 
1. Identify what types of security may be needed: see [5,6,7,8]. Our recommended set is: 
   1. Protection against inadvertent or unauthorized disclosure: confidentiality 
   1. Protection against inadvertent or unauthorized modification: integrity 
   1. Making business assets available for authorized use: availability 
   1. Attribution of responsibility for actions: accountability 

Confidentiality, integrity, and availability are the core properties of security literature. Accountability is also important, but it has a different context. Confidentiality, integrity, and availability are attributes of an asset, while accountability is not. When someone is specifying security properties of enterprise assets, it is important to identify who is responsible for security related activities, and that is where accountability comes in. 

1. Based on the business factors, determine for each asset type which types of security are needed. The desire for security must be balanced against the resources required to achieve security in making this determination. More details about the association of common types of assets, types of security needed, and business factors are provided in the Implementation section below. 

These steps can be applied in a linear fashion, as listed, but other alternatives are also possible. The Dynamics section discusses allowable sequences.

## **Structure**
Using the UML class diagram notation, the general relationships among assets, business factors, and security properties are illustrated in the figure below. A security need is an association between an asset type and a security property: each asset type needs a security property. A given asset type may need any number of security properties (0, 1, or multiple), while a given security property may be needed by any number of asset types. Business factors influence security needs. The security properties are listed in the figure, as well as common asset types and business factors:

![](./Images/security_needs_identification_for_enterprise_assets_structure.png)

*Figure 100: Security needs solution structure*

## **Dynamics**
Allowable sequences for performing the solution steps are shown in figure 101. Identifying assets and identifying business factors are essentially independent activities and can therefore be performed in parallel. However, both activities must be performed prior to determining relationships between assets and business factors. There is often some iteration among these three steps. Defining the set of security properties is also independent and can be performed in parallel with the first three steps. 

Defining properties can also be trivial, providing that the enterprise planner agrees with our suggested set of properties: confidentiality, integrity, availability, and accountability. Some enterprises may want to focus on a subset of these, or add related properties such as privacy, safety, or reliability. Several references discuss this issue further: see [5,6,7,8]. In any case, both defining properties and defining relationships must be performed prior to the last step, determining asset security needs.

![](./Images/security_needs_identification_for_enterprise_assets_dynamics.png)

*Figure 101: Security needs solution sequence constraints*

## **Implementation**
Business factors tend to present conflicting forces regarding security. Some, such as laws and regulations, the sensitivity of certain assets, and the desire to be viewed as a secure enterprise, encourage a high level of security. Others, such as cost constraints, the need for financial health, and the desire to be viewed as open and accessible, encourage a minimum degree of security. The result of this trade-off is that assets need to be differentiated according to their importance to the enterprise.

An investment in security is needed for critical assets, while a greater degree of risk may be accepted for non-critical assets. Critical assets typically are those whose loss or damage would cause significant harm to the enterprise, such as assets whose protection is required by law, strategic plans and other assets related to competitive advantage, irreplaceable items, the reputation of the enterprise, or assets whose loss would entail significant cost impact. Non-critical assets are those whose loss or damage would cause little or no harm to the enterprise, such as easily replaceable items, or information that could be divulged with little or no effect.

In addition to criticality of asset, the types of security needed can also vary by type of asset. Confidentiality and integrity typically apply to data. Integrity and availability apply to physical assets as well. Availability applies to services and may also apply to data. Accountability applies to actions taken on assets. To some degree confidentiality conflicts with availability—the more available an asset is, the less confidential it tends to be.

In some cases, one asset or type of asset may require all types of security protection. A software program is an example: 

- It may be proprietary, in which case it requires confidentiality 
- It needs to be protected against unauthorized change, and it thus requires integrity 
- It must be accessible for authorized users, and it thus requires availability 
- Any changes made to it must be known and attributed, and it thus requires accountability.

The following tables identify typical asset categories that need protection, the type of security needed to protect the assets, the business factors that influence the need, and some explanatory discussion. The tables provide common examples from an enterprise perspective, but they should not be construed as addressing all possible asset types. Table 11 lists and discusses protection of information assets, while Table 12 lists and discusses protection of physical assets.

In using the above tables, it is important to understand, first, that the information is generated from an overall enterprise perspective, and second, that specific combinations may vary from those in the tables for a given enterprise.

An example will illustrate both of these points. Table 11 indicates that personnel data needs availability, while financial data does not. The reasoning is that, in a typical enterprise, availability of finance information is not needed outside the finance department and the senior officers, while availability of personnel data is needed by multiple parts of the enterprise, such as human resources, finance, training, and security. Clearly the finance department needs availability of financial data, but this pattern is an enterprise-level pattern, and across the typical enterprise availability of financial data is not a significant issue. In addition, this table is only representative of common associations. There may be variations for specific enterprises—each will have its own business processes that may differ.

|**Asset Type**|**Protection Needed**|**Business Factors**|**Discussion**|
| :- | :- | :- | :- |
|Personnel data (including payroll)|Confidentiality, integrity, availability, and accountability|<p>- Privacy laws </p><p>- Competition issues</p>|Privacy law will require that personnel private information be treated confidentially. Enterprise staff will need assurance that only human resource staff can modify their information. The data will need to be available to human resource staff as needed, and to financial staff to support payroll. Changes to personnel data must be accountable within the enterprise.|
|Financial data (enterprise financial data)|Confidentiality, integrity, accountability|<p>- Reporting requirements of tax collection agency </p><p>- Competition issues </p><p>- Nature of the enterprise (public, private, or stockheld)</p>|Financial laws and the regulations of government agencies must be upheld in the enterprise or legal repercussions will ensue. Such laws and regulations will require that the financial data be protected from unauthorized modifications and that when modifications occur, there is a clear record of accountability in the enterprise. No enterprise willingly provides its financial data to its competition; the confidentiality of this information must be protected.|
|Legal data (for example, contracts and information on legal proceedings)|Confidentiality, integrity, accountability|<p>- Law </p><p>- Competition issues</p>|An enterprise will need to provide confidentiality under contract law that may also require confidentiality of information related to participants in the contract. The modification of such contracts should be restricted to authorized and knowledgeable personnel and there should be a clear record of accountability in the enterprise.|
|Intellectual property (data and processes)|Confidentiality, integrity, availability|<p>- Partially dependent on the nature of the enterprise (public, private, stock-held) </p><p>- Some competition issues</p>|While some intellectual information (for example, advertisements) will be for the public, others, such as sensitive business processes, will not. Sensitive intellectual property may need restricted access. At the same time, if the business process contains design specifications, it may also need to be highly available within the enterprise.|
|Customer and business partner data (including personal and financial data and intellectual property)|Confidentiality, integrity, accountability|<p>- Competitive issues </p><p>- Service issues if a public company</p>|Enterprise privacy information may be contained in this data. If competitors are aware of the relationships with customers and business partners, they can cause an enterprise to lose its competitive edge. Access to all customer and partner data should be accounted for to ensure that it is not altered in unauthorized ways, and that access to the data is restricted.|
|Public data (product/service information, advertisements, public enterprise information)|Integrity, availability|- Service issues|Unauthorized modification of the data could result in loss of enterprise reputation and/or business share. When such public information is made unavailable, a denial-of-service situation arises.|

*Table 11: Common information asset categories and protections*

|**Asset Type**|**Protection Needed**|**Business Factors**|**Discussion**|
| :- | :- | :- | :- |
|Buildings|Integrity, availability|Critical business processes|An enterprise needs to protect the buildings that provide a work environment for the enterprise from unauthorized modifications or destruction. By doing so, they also promote the availability of the buildings for the enterprise.|
|Employees|Availability, accountability|Critical business employees and processes|An enterprise needs to provide environments that are safe for personnel to ensure the availability of critical personnel. In part they accomplish protecting personnel by establishing accountability for employees.|
|Raw materials/ durable goods/ manufactured products|Integrity, availability|Need to minimize the cost of doing business|Raw materials and durable goods need to be available for use in business processes as required. The enterprise needs to be able to assure its client base that manufactured products will be available as required. Damage, theft, or destruction of raw materials/durable goods will make them unavailable to support business processes. Likewise, damage, theft, or destruction of products will make them unsalable to clients.|

*Table 12: Common physical asset categories and protections*

## **Example Resolved**
This example solves the problem identified as the problem example described earlier. The museum enterprise identifies the following asset types and business factors: 

- Information asset types 
  - Museum employee data 
  - Museum financial/insurance data, partner financial data 
  - Museum contractual data and business planning 
  - Museum research and associated data 
  - Museum advertisements and other public data 
  - Museum database of collection information 
- Physical assets 
  - Museum building 
  - Museum staff 
  - Museum collections and exhibits 
  - Museum transport vehicles 
- External business factors 
  - Insurance policy constraints 
  - International laws and agreements relative to on-loan materials 
  - Privacy laws 
  - Museum charter 
  - Goals and strategies relative to exhibits 
  - Loan of materials and accessions (acquisitions) 
  - Requirements or constraints of organizations that loan materials 
- Internal business factors 
  - Tracking of exhibit items/cataloguing 
  - Item data, including location and value (both a factor and an asset) 
  - Exhibit planning, including loan agreements, transport and installation plans and schedules, legal contracting with exhibitors 
  - Accession (acquisition) planning, via purchase or loan or gift 
  - Legal data (acquisition) 
  - Cost constraints, including funds available for acquisition, personnel, and patron data (including donation amounts), financial data (how much depends on charter: public, private, semi-public, and so on), and cost of security 
  - Intellectual property, such as studies and research data, statistics, and papers 
  - Public information, including hours and current exhibit schedules (near term) as well as brochure and exhibit publications 
  - Building plans 
  - Importance of enterprise reputation for security 
  - Importance of enterprise reputation for accessibility 
  - Sensitive business events, including accession of new items, asset transport to alternate locations, cleaning/caretaking of assets, and special temporary accession for on-loan exhibits

The planner generates a scope statement listing all the above information. The scope statement will be presented to and refined with the museum director. Together they will work to generate an asset protection list such as that shown in Table 13.

|**Asset Type**|**Required Security Properties**|**Business Factor**|
| :- | :- | :- |
|Museum employee data|Confidentiality (HR, management, individual), Integrity (HR, individual only), Availability (HR and management), Accountability (changes in HR)|<p>- Privacy law </p><p>- Enterprise/employee relations</p>|
|Museum financial/ insurance data, partner financial data|Confidentiality, Integrity, Basic accounting|<p>- Contractual obligations </p><p>- Financial reporting laws</p>|
|Museum contractual data and business planning|Confidentiality, Integrity, Basic accounting|<p>- Museum/partner relationships </p><p>- Protect acquisition and transport plans and strategies </p><p>- Protect scheduling data </p><p>- Insurance policy constraints</p>|
|Museum research and associated data|Confidentiality (restricted to narrow group)|<p>- Museum charter requirements </p><p>- Intellectual property </p><p>- Enterprise/employee relations </p><p>- Enterprise/public reputation</p>|
|Museum advertisements and other public data|Integrity|<p>- Enterprise/public reputation </p><p>- Museum charter requirements </p><p>- Partner reputations for loan exhibits</p>|
|Museum building|Integrity, Accountability (for any change)|<p>- Insurance policy constraints </p><p>- Enterprise/employee relations </p><p>- Enterprise/public relations</p>|
|Museum staff|Availability (safety)|<p>- Enterprise/employee relations </p><p>- Laws </p><p>- Enterprise/public reputation</p>|
|Museum collections and exhibits|Integrity, Availability, Accounting|<p>- Insurance policy constraints </p><p>- Enterprise/partner relations </p><p>- Costs</p>|

*Table 13: Establishing security properties for the museum*

## **Consequences**
SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS has the following benefits: 

- It facilitates making balanced and informed decisions about enterprise security needs, by making the competing forces and business factors explicit. The tradeoffs in these factors cause a clear distinction to be made between critical and non-critical assets. The result is increased likelihood that security properties will be applied where needed. That is, protection needs will be explicitly designated for the most critical assets. 
- An additional beneficial result of applying this pattern is that traceability of business asset protection needs back to the relevant business factors is produced and is available for additional use. This information offers a useful rationale to support the evolution of security needs over time. It can also be used, as indicated above, as a basis for more detailed protection requirements in ENTERPRISE SECURITY SERVICES.

SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS also suffers from the following liabilities: 

- Applying this pattern does not come free of charge. It requires an investment of resources, including the time of people who have intimate knowledge of enterprise assets and business factors. While the benefits of applying the pattern are expected to exceed these costs, it is possible for an enterprise to assign people to this task who have less than adequate knowledge of enterprise assets and business factors, and thus to obtain results that are inaccurate or not useful. 
- It is also possible for an enterprise to produce good results from this pattern, but then fail to make use of the results in succeeding patterns.

In both cases, the cost of applying this pattern can exceed the benefits.

## **Known Uses**
Identification of enterprise assets and their security needs is best practice but is often done informally or as part of security risk analysis. A few examples that illustrate concepts in this pattern—and in some cases were sources for the guidance in the pattern—are briefly discussed here.

The Systems Security Engineering Capability Maturity Model (SSE CMM) [6] defines capability levels of a security engineering process, associated with risk assessment. It has elements in common with this pattern:

- It addresses security across the scope of the enterprise 
- It addresses coordination of security needs driven from external entities, including laws, policies, standards 
- The assess impact process includes identifying and characterizing enterprise assets and the need for confidentiality, integrity, availability, accountability, authenticity, or reliability.

PriceWaterhouseCoopers has a process for designing an enterprise security framework that incorporates many of the elements of this pattern [9]. It tailors a security process based on the business requirements and factors of an enterprise. It includes asset inventory collection and information classification.

Mint Business Solutions has defined an approach to security in the context of best practice in enterprise information management [10]. Within this broad framework, they base their security approach on the ISO standard 17799. This standard identifies a set of controls that include:

- Organization of assets and resources, with relation to managing information security 
- Asset classification and control, so that they may be identified and protected 
- Information security policy 
- Compliance with any criminal and civil law, statutory, regulatory, or contractual obligations, and any other security requirement

These controls correspond to the identification of assets and business factors for security addressed in this pattern.

Other standards and practices, including ISO 13335 Part 3 [11], SANS Institute [12], and Operationally Critical Threat, Asset, and Vulnerability Evaluation (OCTAVE) [13], discuss asset identification as part of the overall risk analysis process. OCTAVE is an asset-driven evaluation approach that requires an analysis team to identify information-related assets such as information and systems that are important to the organization.

## **See Also**
After applying this pattern, the next step typically is to apply a set of risk-assessment patterns to further calibrate the security needs of each asset type to determine more specific security requirements. Risk patterns help assist in deciding how much protection is needed for each business asset type. While SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS is somewhat internally focused, the risk assessment patterns include both internal and external considerations.

Following risk assessment, the next step is to assess ENTERPRISE SECURITY APPROACHES that meet the combined security needs and requirements from this pattern and from the risk assessment.

## **References**

[1] Department of Justice. (2004, May). Overview of the Privacy Act of 1974, US DoJ, Washington, DC. <http://www.usdoj.gov/04foia/04_7_1.html> 

[2] European Union. Directive 95/46/EC of the European Parliament and of the Council of 24 October 1995 on the Protection of Individuals with Regard to the Processing of Personal Data and on the Free Movement of Such Data, Official Journal of the European Communities, No. L 281, 23rd November 1995, pp. 31–50

[3] European Union. Directive 2002/58/EC of the European Parliament and of the Council of 12 July 2002 Concerning the Processing of Personal Data and the Protection of Privacy in the Electronic Communications Sector (Directive on privacy and Electronic Communications), Official Journal of the European Communities, No. L 201, 31 July 2002, pp. 37–47

[4] U.S. Congress. FOIA – The Freedom of Information Act, 5 U.S.C. § 552, As Amended by Public Law No. 104-231, 110 Stat. 3048, U.S. Congress, Washington, DC, 1996

[5] International Organization for Standardization. (1999). Common Criteria for Information Technology Security Evaluation (Version 2.1). CCIMB-99- 031. ISO/IEC JTC 1 adopted CC 2.0 with minor modifications in June 1999 as ISO/IEC 15408, Version 2.1

[6] Carnegie Melon University (CMU). (2003, June). Systems Security Engineering Capability Maturity Model, Model Description Document, Version 3.0. <http://www.sse-cmm.org> 

[7] DiDuro, J., Crosslin, R., Dennie, D., Jung, P., & Louden, C. (2002). *A Practical Approach to Integrating Information Security into Federal Enterprise Architecture*. LOGISTICS MANAGEMENT INST MCLEAN VA.

[8] National Security Agency (NSA). (2002, September). Information Assurance Technical Framework (IATF), Release 3.1. Fort Meade. MD. <http://www.iatf.net> 

[9] PriceWaterhouseCoopers. (2001). Designing a Secure Framework for the Competitive Enterprise.

[10] R. Leming. (2003). Best Practice in Information System Management. Mint Business Solutions. <http://www.mintsolutions.co.uk/pages.asp?p=43> 

[11] “Technical Report ISO13335-3 Part 3: Techniques for the Management of IT Security” International Organization for Standardization, American National Standards Institute, 2002

[12] Bayne, J., SANS Institute. (2002). An Overview of Threat and Risk Assessment, Version 1.2f. <http://www.sans.org/rr/paper.php?id=76> 

[13] Alberts, C. J., Dorofee, A. J. (2001, December). OCTAVE Criteria, Version 2.0. <http://www.sei.cmu.edu/pub/documents/01.reports/pdf/01tr016.pdf> 

## **Source**
Schumacher, M., Fernandez-Buglioni, E., Hybertson, D., Buschmann, F., & Sommerlad, P. (2006). Security Patterns: Integrating Security and Systems Engineering (1st ed.). Wiley.
