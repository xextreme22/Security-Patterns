# **Dynamic Integrity Properties**

## **Intent**
The intent of DYNAMIC INTEGRITY PROPERTIES is to reflect the integrity of a system by detection of abnormal behavior.

## **Example**
Figure 9 illustrates an exemplary embedded control system. It shows a rudimentary control loop where a control device controls the environment by measuring it through sensors and manipulating the controlled process through actuators. The control device is an embedded computing system that runs different types of software components. The Basic Software block comprises components for hardware abstraction, the operating system and services and libraries that are commonly used. The Application blocks are the actual functional software components that are used for the control function. These applications are connected to the sensors via a network connection. A message sent over the connection is called Protocol Data Unit (PDU).

![](./Images/dynamic_integrity_properties_example.png)

*Figure 9: Exemplary embedded control system: The control devices, together with the sensors and actuators in the controlled process form a control loop.*

In this scenario, we encounter a number of potential integrity problems. Here, we want to consider a small subset of all possible caveats to explain the different combinations of the presented patterns.

1. Application. An application component is an executable file that is stored on the Non-Volatile Memory (NVM) of the control device. If this file is altered, the integrity of the application is violated. In real world, not every change of an executable file results in a functional change of the component [1]. However, such functional equivalence is not trivial to show and therefore we assume that a change in the binary results in a change in the function. The control system therefore has to ensure, that it does not execute such modified binaries.
1. Control Device. Even if the application components (or even all software components) of the control device are intact, one cannot be sure about the integrity of the overall device. As an example, the hardware integrity may be violated in such a way that the input signal from the sensor is not correctly interpreted. This may lead to a false control decision even when the software works as expected. Therefore, such types of integrity violations have to be identified on the system level, where the behavior of the overall device is observable.

Basically, we want to verify and enforce the integrity of the system or, more specific, of the two explained modules. Therefore, we introduce two mitigation strategies. The control device should be able to check the integrity of its applications autonomously. This is done by the Application Integrity Verifier. Moreover, we want to introduce a Plausibility Checker that mediates the signal to the actuator. This entity also listens to the sensor PDUs and is able to decide, whether a control decision is plausible. An implausible control decision hints to a violation of the control device’s integrity. All two mitigation strategies depend on integrity properties, which are used to decide whether the integrity is violated.

## **Context**
A system consists of multiple interacting components. Therefore, the occurrence of different types of faults is very likely. Faults may be of natural (e.g., hardware faults due to external events or material wear out) or human origin. Human-made faults can be non-malicious (e.g., accidental) or malicious (i.e., a security violation). All types of faults can result in an unintended manipulation of a component or data.

## **Problem**
How to ensure that a state transition of a component or a change of a data value does not harm the integrity of the overall system?

The following forces apply:

- Component Integrity: The system’s integrity would be violated when the integrity of the component that is going to be executed or data value that is going to be used is violated. 
- Integrity Properties: Integrity of a component or data cannot be computed directly.
- Online Violation: An integrity violation may happen during run-time and result in a behavioral change of a system component or in forged information. 
- Offline Verification Not Possible: The component is not examinable from outside before it is used. Therefore, static properties cannot be used. 
- Dynamic Properties: The properties are present before execution, and change, vanish or appear over time. Thus, a single evaluation is thus not reasonable.

## **Solution**
An integrity representation of the component is generated periodically or event-triggered during run-time. Integrity cannot be computed directly, therefore other properties that reflect the integrity of the component are used. The properties are used to reflect the behavior of the component (e.g., state transitions) and therefore have to be examinable during run-time. Moreover, it has to be possible to formulate a policy, that enables a decision concerning the component’s integrity based on the presence, absence, or values of these properties.

The properties have to be identifiable and verifiable. This means that it has to be possible to deterministically compute these properties with the given data or component as input. Moreover, it has to be possible to formulate a policy that enables a decision concerning the component’s integrity based on the presence, absence, or values of these properties. The properties have to be examinable before the component is executed.

As illustrated in Figure 10, the properties change over time. Therefore, one evaluation prior to each verification has to be done. At time (1), for example, the value of Property 1 differs compared to time (2). Moreover, Property 3 vanishes completely during time (1) and (2). The integrity policy, however, requires the existence of this property and therefore the component’s integrity is considered violated at time (2).

![](./Images/dynamic_integrity_properties_solution.png)

*Figure 10: Dynamic properties may change over time. Therefore, they have to be re-evaluated prior to each verification.*

## **Structure**

## **Dynamics**

## **Implementation**
One method to examine the integrity of system components is to measure latency and set a REALISTIC THRESHOLD [2]. Another possibility is to use calls to critical system functions as integrity properties, as in CONTROLLED VIRTUAL ADDRESS SPACE pattern. In order to detect security violations, the control flow of a program can be logged and verified with a model of valid control flow transitions. In terms of data integrity, dynamic properties could depend on different data sources. For example, a difference between two data sets has to meet a certain criteria. Whenever one data set changes, the property (i.e., the difference) is updated and an integrity violation can be revealed, even if the STATIC INTEGRITY PROPERTIES of the data sets are intact.

## **Example Resolved**
With the help of this pattern, we can complete the example introduced. For each mitigation strategy, we will discuss the integrity requirements and show what type of integrity properties can be applied with an exemplary implementation.

1. Application Integrity Verifier. During the development of our system, we start to realize that run-time faults in our hardware platform can occur that could violate the integrity of our applications. Also, the risk of potential adverse packets that may lead to a corruption of the behavior cannot be neglected. Therefore, we have to use DYNAMIC INTEGRITY PROPERTIES to monitor our applications at run-time. In our case, we use a shadow stack. We store the current program counter value at each function call. After each return, we can check whether the return address matches the address that called the function. With this measure, we are able to ensure basic control flow integrity and can thus detect a part of malicious and non-malicious faults at run-time.
1. Plausibility Checker. The plausibility checker is used to identify integrity violations of the control device that this device is not able to identify by itself. However, the plausibility checker only has limited access to the control device. It is only able to intercept the actuator and sensor PDUs. The plausibility checker is not able to identify any properties before the control device is used. Therefore, DYNAMIC INTEGRITY PROPERTIES have to be used. One implementation could be a check whether the actuator value is in a specific range or a check whether a specific relation between the sensor and actuator value is given.

## **Consequences**
The benefits of the pattern are: 

- Component Integrity: The integrity of the component is reflected by the integrity representation that contains STATIC INTEGRITY PROPERTIES. 
- Integrity Properties: Assuming a properly crafted policy, the identified properties reflect the behavior of the components and data and thus the integrity of overall system is verifiable. 
- Online Violation, Dynamic Properties: A violation of the system’s integrity or a changed property during run-time is reflected by the identified properties at the next evaluation point. 
- Offline Verification Not Possible: The pattern uses dynamic properties that reflect the behavior. The integrity of the component can be checked later on.

The liabilities are: 

- Integrity Properties: Even with a very good policy, there exists no property or combination of properties that can ensure the integrity of the component or data without doubt. However, based on the type of the property, this remaining uncertainty can be minimized or adjusted to the needs of the actual system. 
- Online Violation: The properties may reflect violations of the components/data that occurred prior to the execution of the component or the data usage. However, especially if this violation is the result of a malicious fault, it may also mask itself or disable run-time evaluations. 
- Online Violation: Run-time checks require additional resources such as CPU-time and thus may interfere with the actual function of the observed component.

## **Known Uses**
Canaries (magic sequences) are widely used to detect hardware faults or malicious faults such as buffer overflows. Here, a specific value has to be present at a specific address in memory. VOTING [2] calculates the correctness of computation results by exploiting redundancy. Similar concepts are applied in the DISTRIBUTED SAFETY pattern [3] Many systems use the property of what critical system functions are accessed to protect the integrity of other software components [4,5].

## **See Also**
- INTEGRITY PROTECTION pattern.
- STATIC INTEGRITY PROPERTIES pattern.

## **References**

[1] Höller, A., Kajtazovic, N., Rauter, T., Römer, K., & Kreiner, C. (2015, March). Evaluation of diverse compiling for software-fault detection. In *2015 Design, Automation & Test in Europe Conference & Exhibition (DATE)* (pp. 531-536). IEEE.

[2] Hanmer, R. S. (2013). *Patterns for fault tolerant software*. John Wiley & Sons.

[3] Eloranta, V. P., Koskinen, J., Leppänen, M., & Reijonen, V. (2010). A pattern language for distributed machine control systems. *Tampere University of Technology, Department of Software Systems*.

[4] Loscocco, P., & Smalley, S. (2001). Integrating flexible support for security policies into the Linux operating system. In *2001 USENIX Annual Technical Conference (USENIX ATC 01)*.

[5] Cowan, C., Beattie, S., Kroah-Hartman, G., Pu, C., Wagle, P., & Gligor, V. (2000). {SubDomain}: Parsimonious Server Security. In *14th Systems Administration Conference (LISA 2000)*.

## **Sources**
Rauter, T., Höller, A., Iber, J., & Kreiner, C. (2016, July). Static and dynamic integrity properties patterns. In Proceedings of the 21st European Conference on Pattern Languages of Programs (pp. 1-11).
