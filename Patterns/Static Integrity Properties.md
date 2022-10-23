# **Static Integrity Properties**

## **Intent**
The intent of STATIC INTEGRITY PROPERTIES is to reflect the integrity of a system by detection of changes in static system parts.

## **Example**
Figure 7 illustrates an exemplary embedded control system. It shows a rudimentary control loop where a control device controls the environment by measuring it through sensors and manipulating the controlled process through actuators. The control device is an embedded computing system that runs different types of software components. The Basic Software block comprises components for hardware abstraction, the operating system and services and libraries that are commonly used. The Application blocks are the actual functional software components that are used for the control function. These applications are connected to the sensors via a network connection. A message sent over the connection is called Protocol Data Unit (PDU).

![](./Images/static_integrity_properties_example.png)

*Figure 7: Exemplary embedded control system: The control devices, together with the sensors and actuators in the controlled process form a control loop.*

In this scenario, we encounter a number of potential integrity problems. Here, we want to consider a small subset of all possible caveats to explain the different combinations of the presented patterns.

1. Application. An application component is an executable file that is stored on the Non-Volatile Memory (NVM) of the control device. If this file is altered, the integrity of the application is violated. In real world, not every change of an executable file results in a functional change of the component [1]. However, such functional equivalence is not trivial to show and therefore we assume that a change in the binary results in a change in the function. The control system therefore has to ensure, that it does not execute such modified binaries.
1. PDUs. An altered PDU can also lead to a wrong control decision or falsify a control value that is sent to an actuator. The bus system itself is exposed to a possible adverse and harsh environment. Faults at this level are thus very likely and have to be detected.

Basically, we want to verify and enforce the integrity of the system or, more specific, of the two explained modules. Therefore, we introduce two mitigation strategies. The control device should be able to check the integrity of its applications autonomously. This is done by the Application Integrity Verifier. All communication endpoints should be able to verify the integrity of the PDUs with the help of the PDU Integrity Verifier. All two mitigation strategies depend on integrity properties, which are used to decide whether the integrity is violated.

## **Context**
A system consists of multiple interacting components. Therefore, the occurrence of different types of faults is very likely. Faults may be of natural (e.g., hardware faults due to external events or material wear out) or human origin. Human-made faults can be non-malicious (e.g., accidental) or malicious (i.e., a security violation). All types of faults can result in an unintended manipulation of a component or data. Additionally, parts of the system may be constrained in terms of energy or computing resources.

## **Problem**
How to detect whether a component would violate your system’s integrity in case it would be executed? How to detect whether the usage of specific data value (i.e., executing a computation based on this value) would violate the system’s integrity?

The following forces apply:

- Component Integrity: The system’s integrity would be violated when the integrity of the component that is going to be executed or data value that is going to be used is violated. 
- Integrity Properties: Integrity of a component or data cannot be computed directly. 
- Offline Identification: Using components or data with violated integrity compromise the overall systems integrity. Such components should not be executed in the first place and low-integrity data must not be used since they would also lower the integrity of the using component. The integrity thus has to be ensured prior to any use. 
- Performance Constraints: The system has strict resource constraints at run-time and additional overhead should be minimized.

## **Solution**
An integrity representation of the component is generated before it is executed. Integrity cannot be computed directly, therefore other properties that reflect the integrity of the component are used. Since the evaluation is done prior the execution of the component, the properties have to be static. This means that these properties must not change during run-time and must not reflect any behavioral aspect of the component (behavior can only be examined during execution). The properties have to be identifiable and verifiable. This means that it has to be possible to deterministically compute these properties with the given data or component as input. Moreover, it has to be possible to formulate a policy that enables a decision concerning the component’s integrity based on the presence, absence, or values of these properties. The properties have to be examinable before the component is executed.

As illustrated in Figure 8, these properties do not change during run-time. Therefore, only one point of evaluation (1) is needed to gain a representation of the system’s integrity. The integrity representation is a list of properties and their corresponding values. At some point in time (before execution or during execution), the integrity representation is verified (2, 3). The verification is done by comparing the values of the integrity representation with an integrity policy that defines allowed values. During run-time, no re-evaluation is needed since the properties do not change anymore. Therefore, no additional computing overhead for evaluation is needed. However, the verification of the integrity can be done multiple times by different entities.

![](./Images/static_integrity_properties_solution.png)

*Figure 8: Static properties are identified before a component or data is actually used and do not change over time. Therefore, the evaluated integrity representation is valid during the whole time of execution or use and there is no need for re-evaluation for each verification.*

## **Structure**


## **Dynamics**


## **Implementation**
For data integrity, often CHECKSUM [2] is used for communication channels. A checksum represents the computable property that has to match a reference value. The policy is generated by the data source (i.e., a reference checksum is generated). Another component, which is using the data for computation (i.e., the data sink), generates a new checksum (the integrity representation). Before it uses the data value, the sink verifies the integrity of the value by comparing the two checksums. Similar results (but with strong properties regarding security) are achieved with DIGITAL SIGNATURE WITH HASHING pattern.

## **Example Resolved**
With the help of this pattern, we can complete the example introduced. For each mitigation strategy, we will discuss the integrity requirements and show what type of integrity properties can be applied with an exemplary implementation.

1. Application Integrity Verifier. Basically, we want to verify the integrity of the executables. We consider faults (or malicious changes) on the non-volatile memory. We do not consider run-time faults in RAM or CPU that could change the execution of the application (see DYNAMIC INTEGRITY PROPERTIES pattern). We thus should use STATIC INTEGRITY PROPERTIES. For software binaries, hash values and reference values that are compared prior to the execution are state-of-the-art [3].
1. PDU Integrity Verifier. The PDUs are generated at one endpoint and used at another. There is no intended modification of PDUs between these points. Therefore, we can use STATIC INTEGRITY PROPERTIES. We can argue that our communication channels are protected physically, and we do not consider malicious access to the bus. Only unintended or spontaneous faults can lead to an integrity violation. Therefore, we can use simple CRC checks here.

## **Consequences**
The benefits of the pattern are: 

- Component Integrity: The integrity of the component is reflected by the integrity representation that contains STATIC INTEGRITY PROPERTIES. 
- Integrity Properties: Assuming a properly crafted policy, the identified properties reflect the integrity state of the components and data and therefore of the overall system. 
- Offline Identification: The selected properties can be computed before execution and remain during run-time. Therefore, the computation of the properties can be done safely once, prior run-time.
- Performance Constraints: Since the properties do not change during run-time, re-evaluation is not necessary and therefore the identification of these properties do not cause any run-time overhead.

The liabilities are: 

- Integrity Properties: Even with a very good policy, there exists no property or combination of properties that can ensure the integrity of the component or data without doubt. However, based on the type of the property, this remaining uncertainty can be minimized or adjusted to the needs of the actual system, e.g., by performing a risk assessment process with threat modeling. 
- Offline Identification: Since the properties do not change during run-time and no additional checks are performed here, integrity violations during run-time are not detected.

## **Known Uses**
Many protocols, such as CAN or USB use Cyclic Redundancy Check (CRC) to ensure data integrity. This check is done before the packet is forwarded to the actual application (i.e., before it is used by the application). In TLS, during the verification of the message authentication code, the receiver calculates a hash value (static property) of the received message and compares it with a reference value generated by the sender (policy). Systems that implement SECURE BOOT [4] are using signatures or hashes to statically ensure the integrity of software components.

## **See Also**
- Secure boot is a specification of this pattern for OS.
- INTEGRITY PROTECTION pattern.
- DYNAMIC INTEGRITY PROPERTIES pattern.

## **References**

[1] Höller, A., Kajtazovic, N., Rauter, T., Römer, K., & Kreiner, C. (2015, March). Evaluation of diverse compiling for software-fault detection. In *2015 Design, Automation & Test in Europe Conference & Exhibition (DATE)* (pp. 531-536). IEEE.
[2] Hanmer, R. S. (2013). *Patterns for fault tolerant software*. John Wiley & Sons.
[3] Sailer, R., Zhang, X., Jaeger, T., & Van Doorn, L. (2004, August). Design and implementation of a TCG-based integrity measurement architecture. In *USENIX Security symposium* (Vol. 13, No. 2004, pp. 223-238).
[4] Löhr, H., Sadeghi, A. R., & Winandy, M. (2010, February). Patterns for secure boot and secure storage in computer systems. In *2010 International Conference on Availability, Reliability and Security* (pp. 569-573). IEEE.

## **Sources**
Rauter, T., Höller, A., Iber, J., & Kreiner, C. (2016, July). Static and dynamic integrity properties patterns. In Proceedings of the 21st European Conference on Pattern Languages of Programs (pp. 1-11).
