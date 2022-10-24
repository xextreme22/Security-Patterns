# **Fault Container**

## **Intent**
The FAULT CONTAINER pattern aggregates the fault containment techniques described by the INPUT GUARD and the OUTPUT GUARD patterns. This pattern proposes the use of a wrapper that transforms a software component into its fault containing counterpart.

## **Example**

## **Context**
The FAULT CONTAINER pattern applies to a system which has the following characteristics: 

- The system is composed from distinguishable components, which can play the role of fault compartments, and which interact with each other by feeding one’s output into another’s input. 
- It is possible, either directly or implicitly, to validate the input and output of a component against the legitimate input and output described in the component’s specification. 
- The errors that occur in the system are expressed as erroneous input (i.e. input whose content or timing does not conform to the system specification) or as erroneous output (i.e. output whose content or timing does not conform to the system specification). 

The second characteristic implies that the FAULT CONTAINER pattern cannot deal with the types of errors that are not addressed by the INPUT GUARD and the OUTPUT GUARD patterns. In other words, this pattern does not deal with internal errors of a component (e.g., changes to the internal state due to electromagnetic disturbances in the environment where the system operates). Moreover, this pattern does not address cases where the input or the output of a component conforms to the component specification but still contains errors according to the specification of the system in which the component operates (e.g., errors due to design faults). For example, the FAULT CONTAINER pattern cannot provide fault containment when applied either to GETIN or to T2N components in Figure 68.

![](./Images/fault_container_context.png)

*Figure 68: Example of error that may occur to system S and its constituent components.*

## **Problem**
In the above context, the FAULT CONTAINER pattern solves the problem of prohibiting the propagation of an error both from inside a component to the rest of the system and from the component’s environment into the component itself, by balancing the following forces: 

- A system may be composed of components that are developed independently from each other, without keeping strict consistency with each other’s specifications. 
- A system may infect with errors its environment or be infected with errors from its environment despite the fact that the exchanged data that propagate the error is not perceived as erroneous by the system’s environment. 
- Different systems have different requirements regarding size impact of the fault containment mechanism. 
- Different systems have different requirements regarding the time penalty of the fault containment mechanism.
- Fault containment is usually integrated with other solutions provided for other fault tolerance constituents (e.g., error masking, error detection, fault diagnosis) in order to provide wider fault tolerance guarantees. 
- The system to which the FAULT CONTAINER pattern is applied must be indistinguishable from the mechanism that implements the FAULT CONTAINER pattern, i.e., it must not be possible to address or access system without addressing or accessing that mechanism at the same time.

## **Solution**
As stated by its name, the FAULT CONTAINER pattern provides a container that embraces the system on which the pattern is applied. This container forms the fence that stops the propagation of errors in both ways, from inside the container to the outside and vice versa. Whenever the system receives input from its environment or produces output to be delivered to its environment, the container checks it against the specification of the system. If and only if the input (or the output) confirms with the system specification, then it is allowed to reach the system (or reach the system’s environment respectively).

A similarity of this pattern with the other two patterns presented previously in this paper is the fact that the behavior of the container is not defined in the presence of an error, besides the fact that it does not allow it to enter or leave the system. This behavior is intentionally left undefined in order to allow implementations of the FAULT CONTAINER pattern to be combined with error detection, fault diagnosis and system repair mechanisms. It is worth mentioning that in practice there has not been any pure implementation of the FAULT CONTAINER pattern as described here in the fault tolerance literature. Rather, very often the functionality of this pattern is combined with other functionalities. For example, the container functionality is combined with error detection and fault repair through reflection functionalities to result in a mechanism that provides fault containment and repair for the Chorus OS [1]. In another case, the container functionality has been combined with the functionality described by the Adapter pattern [2] in order to result in a fault containing Hive cell made out of four (or more) modified Unix kernels for shared memory multiprocessor architectures [3].

Strictly speaking, from the fault containment perspective the FAULT CONTAINER pattern provides the same benefits as the combination of the INPUT GUARD and the OUTPUT GUARD patterns, i.e., it prevents an error from being propagated inside and outside a given component. From a conceptual perspective however, this pattern contains more than the mere aggregation of the guarding of the access and exit points of a component. It also contains the potential of coordinating these two functionalities, i.e., the possibility to correlate an error in the output of the component to the input that, when processed by the component, led to the erroneous output. This additional functionality becomes visible when combining the FAULT CONTAINER pattern with some fault diagnosis, error detection, fault repair, or system recovery mechanism. In practical terms, an implementation of the FAULT CONTAINER pattern will provide to the environment of the contained component an interface similar to the aggregation of the interfaces provided by implementations of the INPUT GUARD and OUTPUT GUARD patterns. However, behind this interface and in addition to the functionality of the guards corresponding to the aforementioned patterns, the functionality of their coordination may be provided.

## **Structure**
The FAULT CONTAINER pattern introduces two entities, similar to those of the INPUT GUARD and OUTPUT GUARD patterns: 

- The contained component, which is the part of the system that is shielded from getting contaminated by errors propagated from its environment through its input and from contaminating its environment with errors propagated through its output. 
- The container, which is responsible to check against the contained component specification for errors in the input the component receives and in the output the component produces. In addition, the container may correlate the output checks with the checks of the input that led to that output in order to derive causal relations between input and erroneous output.

In practice, the container is a new component in the system which embeds the contained component and also implements the error fencing functionality. That makes the container and the contained component a single addressable constituent in the system composition. Figure 69a illustrates graphically the structure of the FAULT CONTAINER pattern for a contained component with a single access and a single exit point. Figure 69b contains the activity diagram that describes the functionality of the container.

![](./Images/fault_container_structure.png)

*Figure 69: The structure (a) and the activity diagram (b) of the FAULT CONTAINER pattern.*

The implementation approach of the FAULT CONTAINER pattern that is most widely used is the tight coupling of the container with the contained component. This approach is a straightforward application of the solution described above and it has appeared in two variants. In the first variant, usually applied to custom-made software, the container functionality is integrated with the contained component. Every time the latter receives some input or is about to deliver some output, the container functionality is engaged to check that input or output against the contained component specification. In the second variant, usually applied to systems composed from black-box COTS software, the container is a new component that embeds the contained component. Every input sends to the contained component and every output produced by it are intercepted by the container, which checks them against the specification of the contained component. Both variants of this implementation approach are applicable in many cases of software system development, ranging the composition of the system from custom-made software component to the use of black-box COTS software. As long as the specification of the contained component is available, this implementation approach can be applied to transform a software component into its fault containing counterpart. The main characteristic of this approach is that the container introduces a fixed time and space overhead. In cases where the input and output of a component is already checked by other means, the error fencing functionality provided by the container is redundant. Consequently, the system performance is unnecessarily penalized in terms on time and space overhead.

Another implementation alternative is to integrate the container with the environment of the contained component. This results in redundant instances of the container inside the components interacting with the contained component, which introduce a higher space overhead to the system compared to the first implementation alternative. Moreover, with this approach the contained component is entirely shielded from its environment but only shielded from those parts of its environment that contain the container. At the same time however, this approach allows to integrate the container only with selected component from the environment of the contained component. Consequently, the system developer has better possibilities for tuning the time overhead introduced by the FAULT CONTAINER pattern since the container functionality will not be engaged in every single interaction of the contained component with its environment.

This approach has an important drawback: it allows the contained component to be addressed separately from the container. This contradicts the second force of this pattern which requires the contained component to be indistinguishable from its container and results in a partially wrapped component. Nevertheless, this approach can be favored in practice when performance issues are of primary importance. The choice and the responsibility of partially wrapping a component in order to gain performance benefits lies entirely with the system designer. Another drawback of this approach is the additional communication overhead that is introduced by the coordination messages that need to be exchanged among the distributed instances of the container in order to deduce the causal relations between input and erroneous output of the contained component. Some of this overhead can be amortized by integrating these messages with other coordination messages for fault tolerance specific purposes (e.g., with acknowledge messages sent for error detection purposes).

In theory, a third implementation alternative is to place the container in a separate component which acts as proxy for the contained component. However, this approach is very similar to the second variant of the first implementation alternative, and there are no application cases where the first approach does not apply and this one does. On the other hand, the implementation of the container as a separate component bears two drawbacks: it has an elevated space overhead due to the additional component implementing the container, and it also has an elevated time overhead due to the indirection through the component implementing the container of every interaction of the contained component with its environment. For these reasons, this implementation approach has very low practical interest.

## **Dynamics**

## **Implementation**

## **Example Resolved**

## **Consequences**
The FAULT CONTAINER pattern has the following benefits: 

- It stops of errors expressed as input and output content or timing that does not conform to a component specification from entering or exiting that component. 
- The undefined behavior of the container in the presence of errors allows its combination with error detection and error masking patterns (e.g., see [1] and [3]). Whenever this is applicable, the system benefits in terms of reduced run-time overhead introduced by the implementation of the fault tolerant mechanism (e.g., the combination of fault containment and error detection in the context of system recovery from errors). 
- The eminent similarities of this pattern with the Adapter pattern [2] allow the straightforward combination of the two patterns in the same implementation. Employing the Adapter pattern to adjust the interface of a component that is otherwise incompatible with the interface its environment expects from it may introduce faults in the system. The FAULT CONTAINER pattern compliments the Adapter by ensuring that the adaptation of the component interface to the interface expected by its environment does not cause erroneous input and output to propagate from the component to its environment and vice versa. 
- The different implementation alternatives allow the system developer to choose the aptest way to apply the FAULT CONTAINER pattern in a given system. By embedding the contained component inside the container, the former is completely shielded from its environment for a fixed space and time overhead. When appropriate however, the system developer may choose to integrate the container with selected parts of the contained component’s environment. This decreases the time overhead introduced by the FAULT CONTAINER pattern for the price of a higher space overhead (redundant instances of the container) and for lower shielding guarantees since some selected interactions of the contained component will not be checked by the container. 

The FAULT CONTAINER pattern also imposes some liabilities: 

- It is not possible to minimize both the time and the space overhead of this pattern. To keep low the time overhead introduced by the FAULT CONTAINER pattern, the functionality of the container can be integrated with selected components in the environment of the contained component. In addition to reducing the fault containment guarantees only to those selected parts of the system, this approach also increases the space overhead because of the redundant instances of the container. On the other hand, to keep low the space overhead introduced by the FAULT CONTAINER pattern one container can be used, which will embed the contained component. Then, every interaction between the contained component and its environment will have to go through the container, causing an elevated and fixed time penalty for the system execution. 
- For certain systems that require the container to embed the contained component (e.g., systems composed from black-box COTS software), the FAULT CONTAINER pattern results to an elevated implementation overhead. This is because the system developer has to implement the code necessary for embedding one component into another, in addition to the implementation of the container functionality. 
- The FAULT CONTAINER pattern cannot prevent the propagation of errors that do conform with the specification of the contained component as is illustrated in Figure 66 and 69. Such errors are not due to a malfunction of the contained component. Despite the fact that these errors do not have their source in the contained component which is checked to receive input and to produce output according to its specification, they can cause a failure on the rest of the system. Such a failure of the entire system will be traced back to an error detected in the contained component. As a result, recovery actions targeted at the guarded component will probably be taken. However, such recovery actions do not deal with the source of the problem, which is the fault that caused the initial error (e.g., a design fault or an error in the input of the guarded component). To allow effective system recovery, sophisticated (and thus space and time consuming) error detection and fault diagnosis techniques must be employed which will allow the error tracing to be continued through the guarded component until the real source of the propagated error is revealed. 
- The FAULT CONTAINER pattern can effectively protect a component from being contaminated by erroneous input according to its specification. It also prevents the component from delivering to its environment erroneous output according to the component specification. However, unless it is combined with some error detection and system recovery mechanisms, this pattern will result in send- or receive-omission failures (i.e., failure to send output or receive input) of the contained component. For certain systems, such a failure of one of their components may cause a failure on the entire system. Hence, the FAULT CONTAINER pattern has limited applicability to such systems if it is not combined with other fault tolerance patterns.

## **Known Uses**
The FAULT CONTAINER pattern has been applied in practice in the implementation of fault containing Hive cell made out of four (or more) modified Unix kernels for shared memory multiprocessor architectures [3]. Other cases where the FAULT CONTAINER pattern has been used to shield the propagation of errors to and from an operating system kernel are the cases of fault-tolerant Mach [4] and the Chorus kernel [1].

## **See Also**
The FAULT CONTAINER pattern has obvious similarities with both the INPUT GUARD and OUTPUT GUARD patterns. Also, the FAULT CONTAINER pattern complements the Adapter pattern [2] with fault containing properties. Finally, this pattern can be seen as the customization of the Proxy pattern [2] to meet the fault containment requirements of a system.

The POLICY ENFORCEMENT pattern is a generalization of this pattern.

The SINGLE ACCESS POINT pattern can help implement this pattern by only needing to protect one entry point to the application.

## **References**

[1] Salles, F., Rodriguez, M., Fabre, J. C., & Arlat, J. (1999, June). Metakernels and fault containment wrappers. In *Digest of Papers. Twenty-Ninth Annual International Symposium on Fault-Tolerant Computing (Cat. No. 99CB36352)* (pp. 22-29). IEEE.

[2] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[3] Rosenblum, M., Chapin, J., Teodosiu, D., Devine, S., Lahiri, T., & Gupta, A. (1996). Implementing efficient fault containment for multiprocessors: confining faults in a shared-memory multiprocessor environment. *Communications of the ACM*, *39*(9), 52-61.

[4] Russinovich, M., Segall, Z., & Siewiorek, D. (1994). Application transparent fault management in Fault Tolerant Mach. In *Foundations of Dependable Computing* (pp. 215-241). Springer, Boston, MA.

## **Source**
Saridakis, T. (2003, June). Design Patterns for Fault Containment. In *EuroPLoP* (pp. 493-520).
