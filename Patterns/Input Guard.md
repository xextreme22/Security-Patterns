# **Input Guard**

## **Intent**
Often, large scale systems are an assembly of independently developed components, like in the case of systems built out of Commercial off the Shelf (COTS) components. In such cases the developer of an individual component wants to prevent errors that may occur elsewhere in the system from infecting the component he developed. One way to achieve this is to verify that every input fed to his component by the system conforms to the input for his component as described in the system specification.

## **Example**

## **Context**
The INPUT GUARD pattern applies to a system which has the following characteristics: 

- The system is composed from distinguishable components, which can play the role of fault compartments, and which interact with each other by feeding one’s output into another’s input. 
- It is possible, either directly or implicitly, to validate the input of a component against the legitimate input described in the component’s specification. 
- The errors that can be propagated into a system component have the form of erroneous input, i.e., input whose content or timing does not conform to the system specification. 

The second characteristic implies that internal errors (e.g., changes to the internal state due to electromagnetic disturbances in the environment where the system operates) are not considered by this pattern since they are not expressed as erroneous input according to the system specification. Moreover, this pattern does not deal with cases where the input to the system conforms with the system specification, but it still contains errors according to the specification of the system’s environment (e.g., see case (b) in Figure 65).

![](./Images/input_guard_context.png)

*Figure 65: Example of errors that may occur to system S and its constituent components.*

## **Problem**
In the above context, the INPUT GUARD pattern solves the problem of stopping the propagation of an error from the outside to the inside of the guarded component by balancing the following forces: 

- A system may be composed of components that are developed independently from each other, without keeping strict consistency with each other’s specifications. 
- A system may be infected with errors from its environment even when the environment is not experiencing any errors according to its specification. 
- Different systems have different requirements regarding size impact of the fault containment mechanism. 
- Different systems have different requirements regarding the time penalty of the fault containment mechanism. 
- Fault containment is usually integrated with other solutions provided for other fault tolerance constituents (e.g., error masking, error detection and fault diagnosis) in order to provide wider fault tolerance guarantees.

## **Solution**
To stop erroneous input from propagating the error inside a component a guard is placed at every access point of the component to check the validity of the input. Every input to the guarded component is checked by the guard against the component specification. If and only if the input conforms with that specification, then it is forwarded to the guarded component.

Notice that the above solution does not define the behavior of the guard in the presence of erroneous input, besides the fact that it does not forward it to the guarded component. This is intentionally left undefined in order to allow implementations of the INPUT GUARD to be combined with error detection mechanisms (e.g., when a check fails, an error notification is sent to the part of the system responsible for fault diagnosis) or with the implementations of error masking mechanisms (e.g. the comparator entity of the Active Replication pattern [1]). Hence, the behavior of the guard when the checks performed on the input fail depends on the other fault tolerance constituents with which the INPUT GUARD is combined.

## **Structure**
The INPUT GUARD pattern introduces two entities: 

- The guarded component which is the part of the system that is protected against the fault contamination from external errors propagated to it through its input. 
- The guard which is responsible to check for errors in the input to the guarded component against its specification.

There may be many instances of the guard entity for the same guarded component, depending on the system design and on the number of different access points the guarded component may have. For example, a software component with a number of interfaces and a number of operations declared in each interface may have one guard per interface or one guard per operation declared in its interfaces or any possible combination of those. Figure 66a illustrates graphically the structure of the INPUT GUARD pattern for a guarded component with a SINGLE ACCESS POINT. Figure 66b contains the activity diagram that describes the functionality of the guard.

![](./Images/input_guard_structure.png)

*Figure 66: The structure (a) and the activity diagram (b) of the INPUT GUARD pattern.*

One possibility is to implement the guards as separate components in the system. This approach allows to have a number of guards proportional only to the number of the access points of the guarded component. The time overhead introduced by this approach is quite high since it includes the invocation of an additional component (i.e., the guard). Also, the space overhead of this approach is rather elevated since it increases the number of the components in a system by the number of guards that are implemented. Furthermore, in the case where components are mapped to individual units of failure (i.e., each component can fail as a whole and independently of other components) this approach introduces a well-known dilemma in fault tolerance: “QUIS CUSTODIET IPOS CUSTODES?” (“who shall guard the guards?”). This dilemma has also well-known consequences, which are difficult and very costly to resolve (e.g., redundant instances of the guards, distributed among the constituent components of a system, which have their own synchronization protocol).

Despite the above inconveniences, this implementation approach is valuable in the case of COTS-based systems composed from black-box components where the system composer does not have access to the internals of the components. Also, this approach can be applied when fault containment comes as a late- or after-thought in the system development and a quick fix is needed in form of a patch. This implementation approach does not require any modification on existing components of a system; rather, guards are introduced as separate add-on components to the existing system.

Another implementation approach is to make the guard part of the implementation of the guarded component. This practice is often employed in programming where a method checks its arguments before using them to perform its designated task. This allows the coupling of the guard(s) and the guarded component. By integrating the guard with the guarded component, the space overhead of the INPUT GUARD implementation is kept low since it does not introduce another component in the system. Coupling the guard and guarded component implementation is usually applied in the development of COTS software where the developer has no knowledge about the rest of the system in which the component will be integrated. Hence, in order to assure robust functioning of a component, the developer checks the input of the component on every call. The drawback of this implementation approach is the fact that the time overhead is high and fixed. This is because the guard is engaged on every call to the guarded component, even when the supplied input has already been checked by other fault tolerance means.

A third implementation possibility is to place the guard inside each of the components which may provide input to the guarded component. This approach allows the integration of the guard with other fault tolerance mechanisms (e.g., the guard of the OUTPUT GUARD pattern for each component that provides input to the guarded component). Furthermore, this approach allows the elimination of redundant checks for errors which can increase the time and space overhead of fault tolerance solutions in a system. On the other hand, this approach is not applicable to COTS software. Third party developers may not have information about the specification of the other components to which they will feed their output, hence they do not know what conditions to check in the guard. A drawback of this implementation approach is the elevated space overhead; the number of guards is not only proportional to the access points of the guarded component but also to the number of components that provide input to the guarded component. Another drawback is that this guard cannot protect the guarded component from communication errors that occurred during the forward of the checked input from the guard to the guarded component. On the positive side however, this approach allows the guard to be selectively integrated only with those components that are considered not robust enough and subject to produce erroneous input for the guarded component. This can be used to reduce the elevated space overhead of the approach.

## **Dynamics**

## **Implementation**

## **Example Resolved**

## **Consequences**
The INPUT GUARD pattern has the following benefits:

- It stops the contamination of the guarded component from erroneous input that does not conform to the specification of the guarded component. 
- The undefined behavior of the guard in the presence of errors allows its combination with error detection and error masking patterns, and fault diagnosis mechanisms. Whenever this is applicable, the system benefits in terms of reduced run-time overhead introduced by the implementation of the fault tolerant mechanism (e.g., the combination of fault containment and error detection in the context of system recovery from errors). 
- The similarities between the guard entities of the INPUT GUARD pattern and OUTPUT GUARD pattern allow the combination of the two in a single entity. This entity will operate on the same data and will perform two checks: one against the specification of the component that produced the data as output and the other against the specification of the component that will consume the data as input. When applicable, this combination can provide significant benefits in terms of time and space overhead since two separate checks will be performed by the same piece of code. 
- There are various ways that the INPUT GUARD pattern can be implemented, each providing different benefits with respect to the time or space overhead introduced by the guard. It is also possible to integrate the guard with an existing system without having to modify the internals of the system components (first implementation alternative). That significantly reduces the amount of system re-engineering required for applying the INPUT GUARD pattern to COTS-based systems made of black-box components.

The INPUT GUARD pattern also imposes some liabilities:

- It is not possible to minimize both the time and the space overhead of this pattern. To keep low the time overhead introduced by the INPUT GUARD pattern, the functionality of the guard must not be very time consuming. This results in a tendency to introduce a separate guard for each different access point (e.g., one guard per interface or even per operation declared in an interface) of the guarded component. Each such guard checks only a small part of the specification of the guarded component, minimizing thus the execution time of an individual guard. However, this results in a large number of guards, hence in an elevated space overhead. On the other hand, to keep low the space overhead introduced by the INPUT GUARD pattern, the number of guards needs to remain as small as possible. This implies that each guard will have to check a larger number of input for the guarded component, becoming a potential bottleneck and thus penalizing the performance of the system with elevated time overhead.
- For certain systems that require guards to be implemented as components (e.g., systems composed from black-box COTS software), the INPUT GUARD pattern results unavoidably to an elevated time and space overhead. The space overhead is due to the introduction of the new components implementing the guards. The time overhead is due to the fact that passing input to the guarded component requires one additional indirection through the component implementing the guard that check the given input.
- The INPUT GUARD pattern cannot prevent the propagation of errors that do conform with the specification of the guarded component (e.g., see case (b) in Figure 65). Such errors may contaminate the state of the guarded component if it has one. Although these errors cannot cause a failure on the guarded component since it operates according to its specification, they can cause a failure on the rest of the system. Such a failure of the entire system will be traced back to an error detected in the contaminated guarded component. Unless the error detection and fault diagnosis capabilities of the system allow to continue tracing the error until the initial fault that caused it, it is possible that inappropriate recovery actions will be taken targeted only at the guarded component, which, nonetheless, has been operating correctly according to its specification.
- The INPUT GUARD pattern can effectively protect a component from being contaminated by erroneous input according to its specification. However, unless it is combined with some error detection and system recovery mechanisms, this pattern will result in a receive-omission failure (i.e., failure to receive input) of the guarded component. For certain systems, such a failure of one of their components may cause a failure on the entire system. Hence, the INPUT GUARD pattern has limited applicability to such systems if it is not combined with other fault tolerance patterns.

## **Known Uses**
The guard entity can be seen as one possible realization of the pre-conditions validation prior to the execution of a piece of code, as this is described in the design by contract principles [2]. The concept of conditions guarding the execution of tasks has been introduced as monitors already in the ’70s [3]. Monitors, however, would not prevent the flow of erroneous input to the guarded component; rather, they would prevent the guarded task from being executed if the starting/input conditions were not met. Nowadays, the majority of the books that introduce a programming or scripting language also instruct the validation of input arguments upon the call of a function, procedure, method, routine or whatever the name of a functionality block in the corresponding language.

## **See Also**
The INPUT GUARD pattern has similarities with the OUTPUT GUARD pattern. Also, the guard entity of this pattern complements the acknowledger entity of the Acknowledgment pattern [1] in combining fault containment and error detection. The acknowledger is responsible to inform the sender of some input about the reception of it. The combination of the acknowledger and the guard will provide a confirmation of the reception of correct input and a notification in the case of reception of erroneous input.

The POLICY ENFORCEMENT pattern is a generalization of this pattern.

The SINGLE ACCESS POINT pattern can help implement this pattern by only needing to protect one entry point to the application.

The CHECK POINT pattern is a generalization of the INPUT GUARD pattern.

## **References**

[1] Saridakis, T. (2002, July). A System of Patterns for Fault Tolerance. In *EuroPLoP* (pp. 535-582).

[2] Meyer, B. (1997). *Object-oriented software construction* (Vol. 2, pp. 331-410). Englewood Cliffs: Prentice hall.

[3] Hoare, C. A. R., & Monitors, C. A. R. An operating system structuring concept. *CACM. october 1974*.

## **Source**
Saridakis, T. (2003, June). Design Patterns for Fault Containment. In *EuroPLoP* (pp. 493-520).