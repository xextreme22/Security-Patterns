# **Asset Valuation**

## **Intent**
Asset valuation helps you to determine the overall importance an enterprise places on the assets it owns and controls. Loss or compromise of such assets may result in anything from hard costs, such as fines and fees, to soft costs due to loss of market share and consumer confidence.

## **Example**
The museum has begun a risk assessment and identified the following assets to be in scope: 

- Information Assets 
  - Museum employee data 
  - Museum financial/insurance data, partner financial data 
  - Museum contractual data and business planning 
  - Museum research and associated data 
  - Museum advertisements and other public data 
  - Museum database of collections information 
- Physical Assets 
  - Museum building 
  - Museum staff 
  - Museum collections and exhibits 
  - Museum transport vehicles 

They must now determine the overall importance of these assets.

## **Context**
An enterprise has determined which assets are to be included in the overall risk assessment process and must now ascertain the value it places on those assets.

## **Problem**
The ability to define an asset’s value is a key component of any risk assessment. Threats and vulnerabilities that target and expose an asset are only significant within the context of the asset’s value. Without this determination, an enterprise is unable to properly assess the risks posed to its assets. How can an enterprise determine the overall value of its assets? 

An enterprise must resolve the following forces: 

- It must develop a standardized way of assessing and describing an asset’s worth 
- It must provide consistent results despite the subjectivity inherent in this process 
- It must be able to assess, as much as possible, the soft costs due to loss or compromise of an asset 
- It may not be able fully to evaluate the safety impact due to the loss of an asset without having previously experienced a harmful event 
- When evaluating hard costs, an enterprise may waste time on incidental costs, or those of much lesser value relative to the asset

## **Solution**
Systematically determine the overall value of the assets identified in the scope of the risk assessment. This process means to perform the following four steps: 

1. Determine the security value. Determine the security value of the asset based on the importance the enterprise places on guaranteeing the asset’s information security properties: confidentiality, integrity, availability, and accountability. 
1. Determine the financial value. Determine the financial value of the enterprise asset based on the cost of repair or replacement as well as the cost to maintain and operate the asset. Remember that costs such as electricity and storage are probably distributed across many assets. 
1. Determine the impact to business. Determine the value of the asset in relation to the impact a compromise of the asset would have on the enterprise’s business processes. 
1. Determine the overall value and build an asset valuation table. Combine the results of the security, financial and business valuations and determine the overall value the enterprise places on the asset. Enter these results into the asset valuation table.

## **Structure**

## **Dynamics**
The allowable sequence for performing the asset valuation process is shown in figure 184. The three factors, security value, financial value, and business impact, can be assessed in any order. The results are collected and entered into the asset valuation table.

![](./Images/asset_valuation_dynamics.png)

*Figure 184: Sequence constraints of the asset valuation process*

## **Implementation**
For each section, create a value rating scale by defining a range, then providing an accompanying description. Examples for security value, financial value and business impact have been provided in Tables 17, 18, and 19 respectively. To maintain consistency with THREAT ASSESSMENT and VULNERABILITY ASSESSMENT, six ratings are defined. Note that the ratings may be modified according to the preference of the enterprise, although they should remain consistent throughout the risk assessment.

1. Determine the security value. Determine the security value of an asset by considering the following: 
   1. Demand for multiple security properties of a single asset. An asset that requires all four properties would have a significantly higher value than an asset that has a single requirement.
   1. The extent of the consequence due to a compromise of an asset’s security property. For example, the consequence of the unauthorized modification to a payment transaction could be far more severe than modification to a research and development document, though they both require INTEGRITY PROTECTION. 
   1. Health and safety implications resulting from the loss or damage of physical assets, for example ladders, bridges, security guards, or informational assets, such as evacuation procedures, fire containment and first aid instructions.

The information security requirements are normally provided by the asset owner. Otherwise, the security needs of the asset can be identified by applying SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS. Use Table 17 to qualify the security value placed on the asset.

|**Rating**|**Qualitative**|**Description**|
| :- | :- | :- |
|6|Extreme|The asset requires an extreme degree of confidentiality, integrity, and availability. Compromise of these security properties would expose massive amounts of confidential information and endanger public safety.|
|5|Very high|The asset requires a very high degree of confidentiality, integrity, availability, or accountability. Compromise of one or more of these properties would expose sensitive information and possibly endanger public safety.|
|4|High|The asset requires a high degree of confidentiality, integrity, availability, or accountability. Compromise of these properties would expose sensitive information, violating local or federal legislation.|
|3|Medium|The asset has a moderate requirement for information security controls. Compromise of the security properties would violate corporate policy and possibly local or federal legislation.|
|2|Low|The asset has a low requirement for security. Compromise of the asset would expose only non-critical information or data.|
|1|Negligible|The information is publicly available, or the asset has no information security value for the enterprise.|

*Table 17: Security requirements rating*

1. Determine the financial value. Determine the financial value of an asset by considering the following: 
   1. Cost to replace or repair asset due to a damaging event 
   1. Regulatory or legal penalties, fines or fees incurred due to a security violation
   1. Transaction value for which the asset is responsible 
   1. Time incurred by an employee or contractor to receive, configure, or maintain an asset 
   1. Loss of productive employee time, or work backlog incurred

Replacement and repair costs can be obtained from an enterprise’s procurement department, which in turn obtains them either from a VAR (value-added reseller) or directly from the vendor. Regulatory fines and penalties are usually publicly and readily available from the entity that enforces the fines, such as a local or federal government. Hard costs associated with assets that serve a health and safety purpose include hospital and insurance fees, as well as the cost of a cleanup from a fire or flood. Use Table 18 to qualify the financial value placed on the asset.

|**Rating**|**Qualitative**|**Description**|
| :- | :- | :- |
|6|Extreme|The asset has an extreme monetary value for the enterprise. Loss or damage of the asset would probably bankrupt the enterprise.|
|5|Very high|The asset has a major monetary value. Loss or damage of the asset would impose a substantial financial burden on the enterprise.|
|4|High|The asset has a significant monetary value. Repair or replacement would require significant funds.|
|3|Medium|The asset has moderate financial value. Loss or damage of the asset would require financial repurposing.|
|2|Low|The asset has low financial value to the company.|
|1|Negligible|The asset has no monetary value.|

*Table 18: Financial value rating*

1. Determine the impact to business. Determine the impact to business processes by considering the following: 
   1. Loss of customer or investor confidence as a result in the compromise of the asset 
   1. Loss of competitive advantage due to compromise of security properties 
   1. Impact to enterprise partner relationships. or other contractual repercussions 
   1. Lack (or presence) of alternate service: that is, if an alternate service exists that can fulfill customer needs, then the loss of one service (or asset) may have reduced implications
   1. Extent of disruption to other enterprise services due to asset dependencies 
   1. Percentage of customer base affected by outage or degradation of service

Disaster recovery and business continuity plans found in many enterprises may already sort assets by value to the organization. This can provide a starting point for defining the relative business value an enterprise places on the asset. 

Business impact is inherently more subjective and difficult to assess than hard costs. Quite often one may not be able to completely predict the loss of customer confidence, or the extent of loss of competitive advantage. One may, however, draw on events that have occurred to other enterprises of similar size in similar markets.  Use Table 19 to qualify the business value placed on the asset.

|**Rating**|**Qualitative**|**Description**|
| :- | :- | :- |
|6|Extreme|The enterprise cannot function without this asset. Its compromise or loss would result in immediate termination of critical business services.|
|5|Very high|This asset represents a major service of the enterprise. Its loss would result in termination of a critical service or severe degradation of many services.|
|4|High|This asset supports many enterprise services. Its loss would result in termination of a major service or degradation of services.|
|3|Medium|This asset supports a fair number of customers or supports a major service of the enterprise. Its loss would result in degradation of more important services.|
|2|Low|This asset supports an ancillary enterprise service. Its compromise would have a slight impact on business services.|
|1|Negligible|The loss of asset would have no impact to the business.|

*Table 19: Business impact rating*

1. Determine the overall value. Determine the overall value the enterprise places on the asset from the results of the security, financial and business impact valuations. Use Table 20 to qualify the overall value of the asset to the enterprise and collect them in Table 21. 

There will not be direct translation from these three ratings to the single overall value. It is more likely that the overall value will be the highest of the three ratings. That is, if an asset has a very high security value, but a low financial value, its overall value should still be appropriately high.

|**Rating**|**Qualitative**|**Description**|
| :- | :- | :- |
|6|Extreme|The enterprise places the highest possible value on this asset. Its compromise results in human deaths, immediate and total loss of business services or financial bankruptcy.|
|5|Very high|The asset represents or supports a critical business function for the enterprise. Loss or damage of it results in severe financial, security or health repercussions.|
|4|High|The asset is highly valued because of its security requirements or customer focus. Its loss would result in considerable harm to customer services and reputation.|
|3|Medium|The asset is of moderate value. It has some security needs and financial value. Compromise of it would impede the enterprise’s mission.|
|2|Low|This asset is of minor financial value. Compromise of it results in little business impact.|
|1|Negligible|The asset has insignificant importance for the enterprise. It is easily replaced or repaired. It has little to no security requirements and represents no health impact.|

*Table 20: Overall asset value scale*

|**Asset**|**Security Value**|**Financial Value**|**Business Impact**|**Overall Value**|
| :- | :- | :- | :- | :- |
||||||

*Table 21: Asset valuation table template*

## **Example Resolved**
After applying ASSET VALUATION, the museum has determined the value for its information and physical assets. These are shown in Tables 22 and 23 respectively.

|**Asset**|**Security Value**|**Financial Value**|**Business Impact**|**Overall Value**|
| :- | :- | :- | :- | :- |
|Museum employee data|5|3|5|5|
|Museum financial/insurance data, partner financial data|4|3|4|4|
|Museum contractual data and business planning|4|3|4|4|
|Museum research and associated data|3|2|3|3|
|Museum advertisements and other public data|1|2|2|2|
|Museum database of collections information|3|3|4|4|

*Table 22: Information asset valuation table*

|**Asset**|**Security Value**|**Financial Value**|**Business Impact**|**Overall Value**|
| :- | :- | :- | :- | :- |
|Museum building|5|5|6|6|
|Museum staff|6|5|6|6|
|Museum collections and exhibits|5|6|5|6|
|Museum transport vehicles|3|2|2|3|

*Table 23: Physical asset valuation table*

## **Consequences**
This pattern has the following benefits: 

- An enterprise obtains a realistic and complete view of which assets are most critical to its business. 
- The results of the asset valuation can be used to develop or update an enterprise’s disaster recovery and business continuity plan documents. 
- A qualitative value can be easier to obtain than hard costs required by a quantitative asset valuation, thus expediting the overall risk assessment process. 

As well as the following liabilities: 

- An enterprise may be forced to change its practices if it determines that an asset is worth a great deal more than it thought—a change which may be difficult and expensive. This will undoubtedly become a benefit in the long-term. 
- The individuals implementing the pattern may find difficulty in translating results from other parties—for example, hard costs or subjective results—into values consistent with the rating system used here.

## **Variants**
An enterprise may choose a different scale or demand a more complete qualitative description. This is certainly acceptable: the important consideration is that the scale be consistent throughout the use of the pattern and for all assets. For example, [1] uses the following qualitative values for information sensitivity:

- High: extreme sensitivity—restricted to specific individual need to know. Its loss or compromise may cause severe financial, legal, or reputation damage to the enterprise. 
- Medium: used only by specific authorized groups with legitimate business need. May have significant adverse impact, possible negative financial impact. 
- Low: information for internal business use within the company. May have adverse impact, negligible financial impact. 

[1] also uses the following quantitative table to describe financial loss:

|**Financial Loss**|**Valuation Score**|
| :- | :- |
|Less than $2,000|1|
|Between $2k and $15k|2|
|Between $15k and $40k|3|
|Between $40k and $100k|4|
|Between $100k and $300k|5|
|Between $300k and $1M|6|
|Between $1M and $3M|7|
|Between $3M and $10M|8|
|Between $10M and $30M|9|
|Over $30M|10|

*Table 24: Financial loss scale*

## **Known Uses**
An asset valuation is a key component of all widely accepted risk assessments, including those from [1,2,3,4], and others. While they differ slightly in their approach, their purposes and overall goals are consistent.

## **See Also**

## **References**
[1] Peltier, T. R. (2001). *Information security risk analysis.* Auerbach Publications

[2] Stoneburner, G., Goguen, A., & Feringa, A. (2002). Risk management guide for information technology systems. *Nist special publication*, *800*(30), 800-30.

[3] “Technical Report ISO13335-3 Part 3: Techniques for the Management of IT Security” International Organization for Standardization, American National Standards Institute, 2002

[4] “ISO/IEC 17799:2005 Information Technology – Code of Practice for Information Security Management” International Organization for Standardization, 2000.

## **Source**
Schumacher, M., Fernandez-Buglioni, E., Hybertson, D., Buschmann, F., & Sommerlad, P. (2006). Security Patterns: Integrating Security and Systems Engineering (1st ed.). Wiley.
