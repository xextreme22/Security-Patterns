# **Enterprise Security Approaches**

## **Intent**
This pattern guides an enterprise in selecting security approaches, that is, prevention, detection, and response. Security approaches are driven by the security properties its assets require, such as confidentiality, integrity, and availability, and by assessed security risks. Security approaches also provide a basis for deciding what security services should be established by the enterprise.

## **Example**
A new wing of an existing museum of gemstones is to be opened. Business planning activities have provided an enterprise scope in terms of needs, concerns, and assets. Application of SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS has identified security properties applicable to each asset type. The dominant asset type for the museum is gemstones. Gems are valuable and should not be stolen or manipulated, so their required properties are availability and integrity. Another important asset type is documentation and records of gem properties, which require confidentiality, integrity, and availability. The museum needs to determine the security approaches most appropriate for achieving these required security properties, and how those approaches should be coordinated for the museum.

## **Context**
Business assets that require protection and their required security properties (confidentiality, integrity, and availability) are understood, for example from applying SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS. Enterprise or business unit security risks (not system risks) are sufficiently understood, for example, from applying RISK DETERMINATION and its closely related patterns.

## **Problem**
To integrate security into a business model, an enterprise or organization needs to determine preferred security approaches for achieving the security properties of its assets. Planning and operational diligence are security approaches that are always necessary to ensure effective security. In contrast, prevention, detection, and response are security approaches that may be applied in different proportions to each asset type and security property combination. Some business asset/property combinations will have one preferred approach. For example, critical assets that require the integrity property and that cannot be repaired or replaced will have a focus on prevention, since detection and response do not offer a solution to business impairment. On the other hand, assets that require the integrity property but are not critical, or that can be easily and cheaply repaired or replaced, will have a focus on detection of integrity problems and response (usually replacement).

How can security approaches be selected and integrated across an enterprise? 

The forces applicable at the business model level of organization concerns are still abstract and are strongly intertwined with the business processes of the organization. The enterprise needs to resolve the following forces:

- The security properties identified for enterprise assets must be achieved. 
- Security risks cannot be eliminated but can be significantly reduced by a combination of prevention, detection, and response approaches. 
- For critical assets, prevention is preferable to recovery, that is, it is better to prevent a violation of security than to have the violation occur and then try to recover from it. 
- Prevention is sometimes impossible to guarantee or is prohibitively expensive. A prevention mechanism can fail in the face of an unforeseen attack, but it can still be effective for the regular case. 
- Some detection mechanisms can also facilitate prevention, especially when made obvious, such as a prominently displayed security camera, or a motion sensor that sets off a loud alarm. 
- The costs of providing security must be kept to a minimum. 
- Security should have minimal negative impact on business process performance and on users (for example, vendors, clients, staff). 
- Continuity of operations must be maintained even in the face of security incidents, and you want to recover in a timely and satisfactory way from security incidents that cannot be prevented (for example disaster recovery). 
- It should be possible to analyze security incidents to improve your approach.

## **Solution**
Specify an integrated set of approaches that achieve the required security protection for each asset type. The process emphasizes two perspectives, namely, the individual perspective of each asset type, and a holistic perspective of the overall organization. For each asset type, systematically and explicitly examine a set of risk criteria to determine appropriate security approaches and their suggested business priorities. Risk criteria involve the security properties for an asset type, business risk analysis results regarding criticality of the asset, and other high-level business operations information. From a holistic perspective, ensure that the various approaches for asset types complement and reinforce each other, rather than work against each other. 

The process of defining approaches is typically performed by an enterprise architect or strategic planner. The first step is to collect all the necessary information, including asset types and their security needs. Next, information on risk criteria that influence approaches is either collected or generated. Finally, approaches are selected and integrated.

## **Structure**
Table 40 shows elements of the structure of this solution. Participating elements include humans involved in defining the solution for a specific situation. Participants also include primary elements of the process of defining a solution: security needs, security approaches, and selection criteria. More details of these three primary elements are also given in the table. The Implementation section gives additional common examples of selection criteria. Multiple criteria apply to each security approach. More than one approach can be selected for each need.

|**Participating Element**|**Security Need**|**Security Approach**|**Selection Criterion**|
| :- | :- | :- | :- |
|<p>- Business planner/ controller </p><p>- Enterprise architect </p><p>- Enterprise security officer </p><p>- Asset </p><p>- Security need </p><p>- Security approach </p><p>- Selection criterion</p>|<p>- Confidentiality </p><p>- Integrity </p><p>- Availability </p><p>- Accountability</p>|<p>- Prevention </p><p>- Detection </p><p>- Response</p>|<p>- Assets are irreplaceable </p><p>- Asset loss prevents operations of critical business processes </p><p>- Accountability is needed in case of legal ramifications </p><p>- Assets must be repaired/ restored as soon as detection occurs</p><p>- … (see implementation section)</p>|

*Table 40: elements of selecting enterprise security approaches*

## **Dynamics**
The process introduced in the Solution section is illustrated in the next figure. The process comprises three basic steps: collect information, identify security risk criteria, and determine security approaches for each asset type. The second step varies depending on whether sufficient risk information is available to understand the risk criteria that affect the security approach. If it is not available, some qualitative level of criteria must be developed.

![](./Images/enterprise_security_approaches_dynamics.png)

*Figure 199: The process for selecting security approaches*

The figure also shows an analysis and feedback process. Decisions must be revisited because the world changes continuously. The figure shows feedback to the ‘collect inputs’ step, but feedback can go to any of the steps. In addition, if circumstances change sufficiently, feedback can extend beyond the scope of this pattern, to re-apply previous patterns such as RISK DETERMINATION.

## **Implementation**
This section first provides further detail on the process, then presents criteria for selecting security approaches.

### **Process guidelines**
1. Collect necessary input information: 
   1. Critical enterprise asset types 
   1. Basic security needs or properties for each asset type 
   1. Specific security risks for each asset type 
   Note that asset types and basic security needs might be obtained as a result of applying SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS. Similarly, specific security risk information obtained as a result of applying RISK DETERMINATION.

1. Identify security risk criteria that influence approaches: 
   1. If detailed risk information is available (for example, by applying RISK DETERMINATION), those criteria can be used here to determine which approaches to use: prevention, detection, response (also planning, operational diligence). 
   1. If such detailed risk information is not available, qualitative risk criteria such as criticality, ease of replacement, cost of replacement, and harm to reputation can be defined and used here. 
1. Determine which approaches to use for each asset type. More details about the association of types of security needed, risk criteria, and approaches are provided below. 
1. Revisit approaches for each asset type as circumstances change. 
   1. Decisions to revisit may be time-driven, for example annually. 
   1. Decisions to revisit may be event driven. Examples are: (1) an organization makes a significant change to its business process, (2) a major law is passed that requires specific security measures, (3) an organization experiences a major security incident that calls into question its security approaches.
 
### **Approach Criteria**
For each asset type, appropriate security approaches and their suggested business priorities are determined based on desired security properties and risks. If detailed risks are available, for example, from applying the risk management pattern, they can be used to determine approaches. If such risks are not known or available, the qualitative selection criteria shown in Tables 41–44 can be used.

For example, Table 41 would be used to help determine approaches. If accountability is needed for an asset type due to legal ramifications, then detection is an indicated security approach with a high priority.

In using the above tables, it is important to understand that the information is generated from an overall organization perspective. In addition, the tables are not intended to cover all situations for a given organization. The example resolved in the next section will illustrate both of these points.

The focus on security approaches is typically documented as part of a security concept of operations. A security concept of operations presents approaches for addressing security properties and how the approaches work together to address security across the organization. The result should balance prevention, detection, and response into an appropriately layered set of defenses. Balance is needed among layered asset protections, such as entrances to museum spaces and gem display cases. Balance is also needed for the focus on approaches, such as prevention versus detection and response.

Business factors tend to present conflicting forces regarding appropriate balance. Some, such as laws and regulations, sensitivity of certain assets, and the desire to be viewed as a secure enterprise, encourage a high level of prevention. Others, such as cost constraints, the need for financial health, and a desire to be viewed as open and accessible, encourage a minimum degree of prevention with reliance on detection/ response. In cases in which the risk is sufficiently low, a ’no action’ approach may be selected, that is, the approach is to take no measures of prevention, detection, or response. For example, theft of expensive clothes from a shop can be detected by security tags that sound an alarm when the goods are taken outside. But for very inexpensive clothes, the cost of security tags may exceed the cost of a few stolen items. The shop owner therefore may decide to make no response and just write off the loss.

The process of balancing these forces requires assets to be differentiated according to their importance to the organization. An investment in prevention is needed for critical assets, while a greater degree of risk may be accepted for non-critical assets.

- Critical assets typically are those whose loss or damage would cause significant harm to the organization, such as assets whose protection is required by law or strategic plans. Other critical assets are those that offer competitive advantage, are irreplaceable items, can impact the reputation of an organization, or whose loss would entail significant cost impact. 
- Non-critical assets are those whose loss or damage would cause little or no harm to the organization, such as easily replaceable items, or information that could be divulged with little or no effect.

Obviously, there are many possible asset value gradations between non-critical and critical assets. Balancing forces and approaches can also exploit a fact that was mentioned in the discussion of forces: some detection mechanisms can also provide a measure of prevention. These are typically cases in which potential violators are made aware of detection mechanisms and possible accountability, such as prominently displayed surveillance cameras or loud alarms.

It is well known that many considerations are brought to bear at this level in determining an appropriate enterprise security strategy. Management may sometimes levy a requirement to address something specific for security that is realistically beyond what can be accounted for in this pattern. It is strongly recommended that such items be captured, so that when appropriate, they can be tracked through to implementation. In cases in which they are inappropriate, developers of the system model will be forewarned that these requirements will need to be revisited with management.

|**Security Approach**|**Business Priority**|**Criteria Indicating Selection of Approach and Priority**|
| :- | :- | :- |
|Detection|High|Accountability is needed in the case of legal ramifications. |
||Medium|<p>Validity of business communications and their signatures/sources must be ensured. </p><p></p><p>Validity of business process flow/workflow (for example, chain of responsibility or signature) must be ensured. </p><p></p><p>Assets are in a single or limited number of controllable/ observable locations.</p>|
|Response|High|<p>Means of unauthorized asset access must be closed immediately. </p><p></p><p>Intrusion claims must be substantiated in order to pursue administrative or legal actions against unauthorized access to assets.</p>|
||Low|Information asset is non-critical and does not require accountability.|

*Table 41: Criteria for approaches to achieve accountability*

|**Security Approach**|**Business Priority**|**Criteria Indicating Selection of Approach and Priority**|
| :- | :- | :- |
|Prevention|High|<p>Asset loss prevents operations of critical business processes.</p><p></p><p>Asset loss could result in irreparable harm to enterprise reputation. </p>|
||Medium|<p>Asset loss severely impacts operations of critical business processes. </p><p></p><p>Asset loss could result in serious damage to enterprise reputation. </p>|
||Low|<p>Asset loss will impact business processes. </p><p></p><p>Asset loss could result in ill will in client and/or customer base.</p>|
|Detection|High|<p>Total prevention of loss or alteration of assets is not possible. </p><p></p><p>Detection is cost-effective and prevention is not. </p><p></p><p>Asset can be replaced though very costly.</p>|
||Medium|Assets are in a single or limited number of controllable/ observable locations.|
|Response|High|<p>Assets must be repaired/restored as soon as detection occurs. </p><p></p><p>Alterations to assets or other asset characteristics (for example functionality for software assets) must be completely identifiable for repair/replacement. </p><p></p><p>Means of unauthorized asset access must be closed immediately. </p><p></p><p>Intrusion claims must be substantiated in order to pursue administrative or legal actions against unauthorized access to assets.</p>|
||Medium|Assets are of moderate importance to enterprise functions and do not require confidentiality.|
||Low|Particular enterprise assets interact only with non-critical functions.|

*Table 42: Criteria for approaches to achieve availability*

|**Security Approach**|**Business Priority**|**Criteria Indicating Selection of Approach and Priority**|
| :- | :- | :- |
|Prevention|High|Asset reveals highly confidential or sensitive information. |
||Medium|Asset reveals valuable information. |
||Low|Asset reveals information.|
|Detection|<p>Medium</p><p></p>|Information assets can be made available in forms in which no damage can be done (for example, read-only forms, or ‘sanitized’ versions). Since tools to provide such forms are subject to risk, some protection is still needed. |
||Low|Intrusions (that is, unauthorized attempts to read or write protected assets) denied, but awareness of them is needed.|

*Table 43: Criteria for approaches to achieving confidentiality*

|**Security Approach**|**Business Priority**|**Criteria Indicating Selection of Approach and Priority**|
| :- | :- | :- |
|Prevention|High|<p>Asset critical and non-replaceable if corrupted or otherwise damaged. </p><p></p><p>Asset extremely costly to replace or repair. </p><p></p><p>Asset loss could result in irreparable harm to enterprise reputation. </p>|
||Medium|<p>Asset very significant and requires long-lead time to replace or repair. </p><p></p><p>Asset cost to replace very high. </p><p></p><p>Asset loss could result in serious damage to enterprise reputation. </p>|
||Low|<p>Asset significant but replaceable. </p><p></p><p>Asset cost to replace or repair moderate. </p><p></p><p>Asset loss could result in ill will in client and/or customer base.</p>|
|Detection|High|<p>Permanent asset alteration will significantly impair enterprise or operation of critical business processes. </p><p></p><p>Total prevention of loss or alteration of assets is not possible. </p><p></p><p>Detection is cost-effective and prevention is not. </p><p></p><p>Asset can be replaced although very costly. </p>|
||Medium|<p>Validity of business communications and their signatures/sources must be ensured. </p><p></p><p>Validity of business process flow/workflow (for example, chain of responsibility or signature) must be ensured. </p><p></p><p>Assets are in a single or limited number of controllable/ observable locations. </p><p></p><p>Information assets can be made available in forms in which no damage can be done (for example, read only forms or ‘sanitized’ versions). Since tools to provide such forms are subject to risk, some protection is still needed. </p>|
||Low|<p>Enterprise information assets need to be accurate and support any/ all legal needs.</p><p></p><p>Intrusions (that is, unauthorized attempts to read or write protected assets) denied, but awareness of them is needed.</p>|
|Response|High|<p>Assets must be repaired/restored as soon as detection occurs. </p><p></p><p>Alterations to assets or other asset characteristics (for example, functionality for software assets) must be completely identifiable for repair/replacement. </p><p></p><p>Means of unauthorized asset access must be closed immediately. </p><p></p><p>Intrusion claims must be substantiated in order to pursue administrative or legal actions against unauthorized access to assets. </p>|
||Medium|<p>Assets are repaired/replaced normally within three days of problem detection. </p><p></p><p>Assets are of moderate importance to business functions and do not require integrity. </p>|
||Low|<p>Assets should be restored within a week, but longer periods will not impair enterprise operations. </p><p></p><p>Information asset is non-critical and does not require integrity. </p><p></p><p>Particular enterprise assets interact only with non-critical functions.</p>|

*Table 44: Criteria for approaches to achieve integrity*

## **Example Resolved**
This section outlines portions of the result of applying the solution to a museum of gemstones. Identification of museum assets and their security properties is available from the Example Resolved section of SECURITY NEEDS IDENTIFICATION FOR ENTERPRISE ASSETS. Museum enterprise architects and planners have completed a business unit risk assessment for the new wing of the museum. The architects and planners must now work to identify security approaches for which the museum will be willing to allocate the resources necessary to achieve the security properties identified.

An example outcome is summarized in Table 45. The column for ‘Special notes’ has been included to show examples of special considerations and decisions that might be made by management while considering general security approaches.

Note that in the integration perspective, approaches are coordinated. For example, when prevention fails (thief grabs gem), detection and response act as a fallback (laser beam was interrupted, causing automatic doors to close before thief can leave the building).

|**Properties and Applicability**|**Security Approach**|**Business Priority for Approach**|**Special Notes**|
| :- | :- | :- | :- |
|<p>Protect Integrity of museum data: </p><p>- Employee </p><p>- Contractual </p><p>- Financial </p><p>- Partner financial</p>|<p>- Prevent </p><p>- Detect </p><p>- Respond</p>|<p>- High</p><p>- High</p><p>- High</p>|Employee data should only be available to HR, staff, & management|
|<p>Protect Integrity of all other museum data: </p><p>- Insurance </p><p>- Business planning </p><p>- Public data</p>|<p>- Prevent </p><p>- Detect </p><p>- Respond</p>|<p>- Moderate </p><p>- High </p><p>- Low</p>|While this information is very important, modifications can be detected and emended without high consequences|
|<p>Protect Integrity of physical assets: </p><p>- Buildings </p><p>- Collections/exhibits</p>|<p>- Prevent </p><p>- Detect </p><p>- Respond</p>|<p>- High</p><p>- High</p><p>- High</p>|This is a critical cost driver|
|<p>Protect Confidentiality of museum data: </p><p>- Financial/insurance </p><p>- Partner financial </p><p>- Contractual </p><p>- Exhibit plans </p><p>- Research and its data</p>|<p>- Prevent </p><p>- Detect </p><p>- Respond</p>|<p>- High</p><p>- High</p><p>- Moderate</p>|These are critical to business operations. Management wants a focus on prevention and detection with high quality encryption.|
|Protect Confidentiality of employee data|<p>- Prevent </p><p>- Detect </p><p>- Respond</p>|<p>- Moderate</p><p>- Moderate</p><p>- Moderate</p>|Not as critical to business operations. Restrict access to HR, staff & management|
|Protect Availability of museum employee data|<p>- Prevent </p><p>- Detect </p>|<p>- Moderate</p><p>- Moderate</p>|HR is only user with critical availability concerns|

*Table 45: Security approaches established for desired security properties*

## **Consequences**
The following benefits may be expected from applying this pattern: 

- The pattern fosters management level awareness: all enterprise security patterns help management better understand security as an overall issue and gives them terminology and simple understanding of the underlying concepts without relying on details of the technology used to implement them. 
- It facilitates conscious and informed decision-making about security approaches to satisfy identified security needs. 
- It promotes sensible resource allocation to protect assets. 
- It allows feedback in the decision process, to better adjust security approaches to the situation at hand by traceability back to business factors and security needs. 
- It encourages better balance among the security, cost, and usability of an asset. 
- It shows that you can combine approaches to better and more cheaply protect an asset.

The following potential liabilities may result from applying this pattern: 

- It requires an investment of resources to apply the pattern. In some cases, the cost of applying the pattern may exceed its benefits.
- It requires the involvement of people who have intimate knowledge of assets, and basic knowledge of asset security needs and security approaches. These people typically have high positions in the enterprise and their time is valuable. 
- It is possible for an organization to assign people to this task who have a less than adequate knowledge of assets, security needs, or approaches, because they may have more available time or are less expensive. If the people applying the pattern do not have a good knowledge of enterprise assets and their value, the pattern results may be inaccurate or not useful. 
- Perception of security needs can differ throughout an organization. This may make it difficult to reach agreement on priorities of approaches. On the other hand, bringing such disagreements to the surface may be a benefit, because they can then be properly discussed and resolved.

## **Known Uses**
The prevention-detection-response approaches identified in this pattern, and the process of associating them with risk criteria, are well-established functions in the security community. [1] refers to ‘the commonly mentioned prevention-detection response philosophy…’ In a security course description, [2] states that ‘general security practitioners, system administrators, and security architects will benefit by understanding how to design, build, and operate their systems to prevent, detect, and respond to attacks.’ Sometimes these approaches are included in a broader list of security functions or safeguards. [3] identifies these categories: planning, prevention, detection, diligence, and response. [4] states on page 44, ‘In general, safeguards may provide one or more of the following types of protection: prevention, deterrence, detection, reduction, recovery, correction, monitoring, and awareness.’ Criteria details in the Implementation section of this pattern are based on extensive MITRE Corporation experience with our customers.

## **See Also**
After applying this solution, the next step typically is to apply ENTERPRISE SECURITY SERVICES to select security services that support the approaches selected in this pattern.

## **References**

[1] Chuvakin, A. (2002, April). Intrusion Detection Response. http://www.linuxsecurity.com/feature\_stories/ids-active-response.html

[2] SANS Institute. (n.d.). Track 4: Hacker Techniques, Exploits and Incident Handling. <http://www.sans.org/kansascity04/description.php?track=t4>

[3] DiDuro, J., Crosslin, R., Dennie, D., Jung, P., & Louden, C. (2002). *A Practical Approach to Integrating Information Security into Federal Enterprise Architecture*. LOGISTICS MANAGEMENT INST MCLEAN VA.

[4] “Technical Report ISO13335-3 Part 3: Techniques for the Management of IT Security” International Organization for Standardization, American National Standards Institute, 2002

## **Source**
Schumacher, M., Fernandez-Buglioni, E., Hybertson, D., Buschmann, F., & Sommerlad, P. (2006). Security Patterns: Integrating Security and Systems Engineering (1st ed.). Wiley.

