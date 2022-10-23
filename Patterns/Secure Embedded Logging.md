# **Secure Embedded Logging**

## **Intent**
Answering to the security needs by extending the data logging capabilities in the embedded platforms by adding security modules and services. The proposed pattern can be used to add the logging security features together with a design-focused logging solution. 

## **Example**
Electric vehicles contain specialized embedded platforms called Battery Management System (BMS), dedicated for control and management of battery cells used to power up the engine and other components [1,2]. Different derivations of BMS exist, with the modular and distributed BMS being more common than the others. Each Battery pack contains several dedicated sensors alongside battery cells [1]. The battery packs are controlled through Battery Cell Controllers (BCC), which are assigned to handle the immediate data control and throughput of these individual packs. A central BMS receives individual battery packs data from the BCCs. This data ranges from the sensor data (e.g., temperature data) to the voltage and current of a particular cell. They are used to extract information like state of charge (SoC) or state of health (SoH) [3]. Based on the data received, BMS can also store and handle error events. 

When a battery pack gets depleted, it needs to be replaced. The replaced battery pack can often still be used as an active component for some other appliances, e.g., power grids. Here, battery packs are aimed to be shipped together with their assigned BCCs. In case the BCCs are to remain as part of the vehicle and its BMS, a design compromise needs to be established to enable the logged operational data to be shipped with the battery pack as well. Since BMS would be mass-produced, a design needs to be made in the earlier phases of the development.

It is desirable to enable the porting of the stored memory units together with the battery packs as to be able to track the health of the used battery packs or for any additional data post-processing. It is of critical importance that only valid battery packs are being transported and that it is possible to authenticate the memory units used with the previous battery packs. This constraint is essential to make sure that no malicious attacks through a modified memory unit are possible. Additionally, the data that is stored needs also to be secured. The reason for making it secure is to guard it against any potential malicious attacks or even faults that can arise from oversights during service and maintenance. The constraint is still present to handle these design steps in the initial development phase. This is done for the fact that the battery packs would be mass-produced. Any change that would otherwise be done later could jeopardize the security of the battery packs and add an additional cost.

## **Context**
An embedded platform is being designed which uses local memory devices to handle the storage of lifetime logging data. For the reasons of the cost and memory size limitations, as well as not having, or having limited, access to a wide network, it is intended for the platform to rely on local solutions rather than remote services. Often, these types of systems are closed and protected under a specific group. It is critical that the stored data maintains its integrity and is only managed through authorized handlers in this environment. The embedded system would consist of a selection of hardware modules, interfaces, and implemented software functions. The hardware modules are divided by their respective tasks and placement. These are usually tied to a specific architecture and their upgrade can be very difficult, or sometimes not even possible.

## **Problem**
How to design an embedded platform that is able to securely handle, but also port and verify, logging data from its source to a designated entity? 

In embedded platforms that use distributed module placement, logging process and porting of the logged data often introduce security risks. Porting would need to be done either by using a manual external device or having a connection to a network, both of which might be difficult, and even not feasible, under the platform constraints. Additionally, changes introduced to the system on a physical layer may hamper security during the transfer of the saved data and present a high level of porting complexity. Modules used would need to account for security functionality and have pre-defined elements that supplement them. These considerations result in making it a very challenging and expensive task. 

Different malicious attacks can be mounted aimed directly at the content of the logged data during both the active logging period and during the offload transfer period. It is challenging to derive a definite list of threats, as these are usually use-case or application dependent. Here we focus primarily on generic threats that are found in embedded logging systems. Specifically, we consider the following main threats: 

- Spying on the targeted process: If not properly secured, an attacker can derive information, and even knowledge, from the stored log data by a direct port access. 
- Logged data tampering: Unauthorized change of the current, or previously stored, log content. This includes active attacks on the communication points during the ongoing logging process, but also direct tamper attacks on the devices. 
- Counterfeited sources: Each logged data is tied to an affiliated monitored device that is also supplied from a certified manufacturer. During the offload transfer period of the device, i.e., when the change of the targeted monitored device happens, the device can be replaced with a counterfeited or a malicious one. A different attack would be by using the same device but replacing the data inside it. 

The following forces apply to the possible solution: 

- Streamlined HW/SW integration: Implementation of the hardware and software elements associated with the security functionality need to be easily replicated across multiple devices and vendors. 
- Production cost: Changes made to the hardware and software design of the embedded systems can result in an increased manufacturing cost. 
- Limited resources: The embedded system needs to be able to execute all necessary functions under different constraints. 
- Security - confidentiality and integrity: Necessary measures need to be taken which should prevent the logged data to be tampered or spied on. 
- Security - authenticity: The logged data that is stored needs to be able to be properly identified and verified that it comes from a valid source entity. This authenticity is also necessary each time the data needs to be accessed during the active period, i.e., when the data is retrieved for the analytic or other operational purposes

## **Solution**
Ensure that the monitored logged data will be securely protected through an integrated security module relaying data to the memory module and authenticated by using necessary hardware and software critical components embedded during the deployment phase. 

When implementing a logging procedure as part of the constrained embedded platform, the security requirement is achieved by integrating a Security Module (SM) as part of the EC. While adding the SM to individual EC devices adds to the overall cost, it does make the system more decentralized. Furthermore, this ensures that the security operations are distributed without heavily impacting the performance. EC device vendors could also not guarantee that the logged data would be secured since the EC itself would not handle that constraint. Therefore, it is necessary to also couple the security operations as part of the EC to appropriately address the security design and attest that the information stored will be protected. 

## **Structure**
Figure 1 depicts the design behind the solution and shows the recommended building blocks. The following components are listed:

- Embedded Controller (EC): Contains necessary interfaces for the communication, main driver logic, and the control bridge between the data that is to be stored and the security driver. 
- Logging Memory Unit (LMU): Dedicated device for storing the encrypted data; contains necessary description data, encrypted security keys, and the encrypted data. 
- Security Module (SM): Provides security operations; works as a security bridge between the EC and the LMU. 
- Source Verification Device (SVD): Device tied to a particular LMU and used for the authentication purpose; can contain necessary authentication data, i.e., private-public key pair, and/or a certificate. It is also generally seen as the device from which logging process data is retrieved (data source).

![](./Images/secure_embedded_logging_structure.png)

*Figure 1: Design-based solution in task separation for handling security logging by providing secure operations and device authentication.*

Software functions and associated security data would be handled by the SM itself. At the same time, it would use the logic controller of the EC to drive the overall processing and data preparation when storing it as part of the logging. It is necessary to keep the SM cheap in design. As a minimum-security requirement when storing the logging data, we propose to use encryption and authentication for the stored data. These can either be achieved by using separate security functions or applying a suite like Authenticated Encryption (AE) to handle this process. The data integrity check (additional AE data or a separate operation) would be saved in a separate memory block inside the LMU. These, however, do not need to be secured, but they do need to be checked by the EC from the SM each time a new LMU is authenticated. They also need to be periodically updated from the SM after new data is written. The SM should also offer the functionality of storing and handling the key data used by the security operations. Additionally, an EC together with its SM could also provide a Key Derivation Function (KDF). The basic principle of deriving and delivering the keys between the parties is left to the designers. The keys are generally securely encrypted and stored in the LMU secure section. The authentication operations can either be managed using symmetric-based authentication, e.g., AES challenge/response mechanism or by using asymmetric authentication, e.g., Public Key Infrastructure (PKI). The security operations can be handled entirely through software or be hardware-derived, where the hardware operations usually offer better performance, e.g., hardware implementation of the Advanced Encryption Standard (AES). While we consider using the integrated security engine through a dedicated SM as the most cost-effective solution, other dedicated hardware security components can also be examined. These include Secure Elements (SE) and Trusted Platform Module (TPM). However, unlike the integrated secure engine, SE and TPM are more complex to incorporate and much more costly. 

## **Dynamics**
The pattern is aimed at providing an affordable and secure solution when transporting and then replacing an LMU.

This process is depicted in Figure 2. Here, a user would receive the LMU together with the SVD from a previous socket. When integrating it into the new system, it might be necessary to verify this memory unit alongside the newly installed SVD, which has been formerly taken out from the older system. This is achieved by using the previously explained security verification functions that the new EC, through its design with SM, would possess as well. The verification process needs to be successfully completed for the LMU to be further used, be it just for the analytic or for continuing operations.

![](./Images/secure_embedded_logging_dynamics.png)

*Figure 2: Sequence diagram describing the verification process during the porting of LMU and a SVD from a previous to a new embedded device platform.*

## **Implementation**

## **Example Resolved**
Based on the open design question presented in Section 1.2, we present a solution in form of a module extension. Here, security is applied to guarantee: (i) confidentiality - protecting necessary system data by only providing data associated to the BMS operational cycle, (ii) repudiation – an action can be tied to the entity that caused it, and (iii) integrity – data has not been modified. The resulted block design is shown in Figure 3. 

The security functionality is controlled by an internal SM service engine. The SM communicates directly with the EC and the memory interface.

![](./Images/secure_embedded_logging_example_resolved.png)

*Figure 3: Realized example based on the SECURE EMBEDDED LOGGING pattern. Applied on a BCC of a BMS by extending its applicability for the secure logging process*

When the need arises for a battery pack to be replaced, using this design, it is possible to also port the whole BCC easily or just the memory module with the logged data, as indicated in the pattern solution. Based on the implementation, the BMS would verify the newly added BCCs, while a BCC can independently run the verification operation on the connected battery pack and memory module.

## **Consequences**
This section lists the benefits and liabilities found when applying the SEL architectural pattern based on the Forces from Section 1.4. The benefits of the SEL pattern are: 

- As the pattern suggests using a dedicated security module and predefined security functions per EC, the general production design can be applied on a larger scale. 
- The pattern proposes the use of a dedicated SM that should allow, at a minimum, encryption, and integrity check for handling the security of the stored data. 
- Additionally, the SM needs to allow for a method of authentication and verification of individual memory modules that were previously tied to a specific pair of EC and MED. 

The liabilities when using the SEL pattern are: 

- As long as the security logging is only handled in a closed local embedded platform, further system updates and configurations are not handled with the proposed pattern. 
- Each device in the suggested embedded platform is handled as a separate unit, meaning that each embedded controller comes with their own security module. This advantage at flexibility comes also with a drawback, and that is the increase of the general production cost. 
- Many embedded devices today are limited in terms of the extension capabilities, i.e., either not containing their own security modules or not providing additional interfaces.

## **Known Uses**

## **See Also**
A pattern that is similar in design but different in intent and context would be the SECURITY LOGGER AND AUDITOR. It is focused on logging security-sensitive actions from different users. Hence, this pattern offers a security solution in tying recorded information with the particular users of a system on an architectural level. Another similar pattern would be the Secure Logger which is traditionally used for capturing targeted application events [4]. It is an implementation design pattern that can be applied on the software level in situations where otherwise system constraints are of no concern and are not taken into the design consideration.

## **References**

[1] Andrea, D. (2010). *Battery management systems for large lithium-ion battery packs*. Artech house.
[2] Deng, J., Li, K., Laverty, D., Deng, W., & Xue, Y. (2014). Li-ion battery management system for electric vehicles-a practical guide. In *Intelligent Computing in Smart Grid and Electrical Vehicles* (pp. 32-44). Springer, Berlin, Heidelberg.
[3] Xiong, R., Cao, J., Yu, Q., He, H., & Sun, F. (2017). Critical review on the battery state of charge estimation methods for electric vehicles. *Ieee Access*, *6*, 1832-1843.
[4] Steel, C., & Nagappan, R. (2006). *Core Security Patterns: Best Practices and Strategies for J2EE", Web Services, and Identity Management*. Pearson Education India.

## **Sources**
Basic, F., Steger, C., & Kofler, R. (2021, July). Embedded Platform Patterns for Distributed and Secure Logging. In *26th European Conference on Pattern Languages of Programs* (pp. 1-9).

