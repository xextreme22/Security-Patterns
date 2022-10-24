# **Output Guard**

## **Intent**
Whereas the INPUT GUARD pattern prevents an error in the input of the guarded component from contaminating that component, it is often highly desirable stop the error when it occurs rather when it is about to be propagated to another component. The OUTPUT GUARD pattern describes how to confine an error in the component that contains the fault which led to that error. The technique described by the OUTPUT GUARD pattern is very similar to the one described by the INPUT GUARD pattern: the output of a component is checked against the specification of the component to ensure conformance. Despite the similarity of the technique, these two patterns serve different purposes. The INPUT GUARD pattern prevents the contamination of a component from an error occurred elsewhere while the OUTPUT GUARD pattern confines an error inside the component where that error occurred.

## **Example**

## **Context**
The OUTPUT GUARD pattern applies to a system that has the following characteristics: 

- The system is composed from distinguishable components, which can play the role of fault compartments, and which interact with each other by feeding one’s output into another’s input. 
- It is possible, either directly or implicitly, to validate the output of a component against the legitimate output described in the component’s specification. 
- The errors that occur in a system are expressed as erroneous system output according to its specification, i.e., output whose content or timing does not conform to the system specification. 

Similar observations as for the INPUT GUARD pattern apply here. The second characteristic implies that internal errors (e.g., changes to the internal state due to electromagnetic disturbances in the environment where the system operates) are not considered by this pattern unless they result in erroneous output according to the system specification. Moreover, this pattern does not deal with cases where the output of the system conforms with the system specification, but it still contains errors according to the specification of the system’s environment (e.g., see case (a) in Figure 65).

## **Problem**
In the above context, the OUTPUT GUARD pattern solves the problem of confining an error inside the component where it occurs by balancing the following forces:

- A system may be composed of components that are developed independently from each other, without keeping strict consistency with each other’s specifications. 
- A system may infect with errors its environment when the environment is not able to distinguish the erroneous output from a correct one. 
- Different systems have different requirements regarding size impact of the fault containment mechanism. 
- Different systems have different requirements regarding the time penalty of the fault containment mechanism. 
- Fault containment is usually integrated with other solutions provided for other fault tolerance constituents (e.g., error masking, error detection and fault diagnosis) in order to provide wider fault tolerance guarantees.

## **Solution**
To stop an error from being propagated outside the component where it occurred, a guard is placed at every exit point of the component (be it message emission or invocation return point). Each such guard checks the produced output against the specification of the component. If and only if the output conforms with the component specification, then it is allowed to reach the component’s environment.

Notice the above solution does not define the behavior of the guard in the presence of erroneous output, besides the fact that it does not allow it to reach the component’s environment. Similarly, to the INPUT GUARD pattern, this behavior is intentionally left undefined in order to allow implementations of the OUTPUT GUARD pattern to be combined with error detection mechanisms (e.g., when a check fails, a notification is sent to the error detection mechanism [1]). Hence, the behavior of the guard when the checks performed on the output fail depends on the other fault tolerance constituents with which the INPUT GUARD is combined.

## **Structure**
Since the technique captured by the OUTPUT GUARD pattern is very similar to the one captured by the INPUT GUARD pattern, it does not come as a surprise that the entities the former introduces are similar to those introduced by the latter: 

- The guarded component, which is the part of the system, which is guarded against the occurrence of errors, and in which occurred errors will be confined. 
- The guard which is responsible to check for errors the output to the guarded component against its specification.

Similarly, to the INPUT GUARD pattern, there may be many guards for the same guarded component, depending on the system design and on the number of different exit points the guarded component may have. For example, a software component with a number of interfaces and a number of operations declared in its of them can have one guard per interface, or one guard per message-send and return operation, or any possible combination of those. Figure 67a illustrates graphically the structure of the OUTPUT GUARD pattern for a guarded component with a single exit point. Figure 67b contains the activity diagram that describes the functionality of the guard.

![](./Images/output_guard_structure.png)

*Figure 67: The structure (a) and the activity diagram (b) of the OUTPUT GUARD pattern.*

One way to implement the guards is as separate components in the system. This approach has the same advantages and inconveniences as its counterpart in the INPUT GUARD pattern. It allows to have a number of guards proportional only to the number of the access points of the guarded component. However, the time overhead is quite high since it includes the invocation of an additional component (i.e., the guard). Also, the space overhead of this approach is rather elevated since it increases the number of the components in a system by the number of guards that are implemented. Furthermore, in the case where components are mapped to individual units of failure (i.e., each component can fail as a whole and independently of other components) the problem of “guarding the guards” raises again as for the INPUT GUARD pattern. This dilemma has also well-known consequences, which are difficult and very costly to resolve (e.g., redundant instances of the guards, distributed among the constituent components of a system, which have their own synchronization protocol).

Despite the above inconveniences, this implementation approach is valuable in the case of COTS-based systems composed out of black-box components where the system composer does not have access to the internals of the components. Also, this approach can be applied when fault containment comes as a late- or after-thought in the system development and a quick fix is needed in form of a patch. This implementation approach does not require any modification on existing components of a system; rather, guards are introduced as separate add-on components to the existing system.

Another implementation approach is to make the guard part of the implementation of the guarded component. This practice is often employed in programming where a method checks the validity of the output it produced before returning it to its environment (e.g., to its caller). This allows the coupling of the guard(s) and the guarded component. By integrating the guard with the guarded component, the space overhead of the OUTPUT GUARD implementation is kept low since it does not introduce another component in the system. Coupling the guard and guarded component implementation is usually applied when developing software components that are meant to upgrade an existing system. In these cases, the developer of the new software component wants to make sure that the produced output will not introduce any errors to the existing system. To archive this, the developer of the component introduces self-checking code for the output the component produces (e.g., see [2] and [3] for more information on self-checking code). The drawback of this implementation approach is the fact that the time overhead is high and fixed. This is because the guard is engaged every time the guarded component produces output, even when that output will be anyway checked as whether it is valid input for the component which will receive it (e.g., see the INPUT GUARD pattern).

A third possibility is to place the guard inside each of the components which may receive the output produced by the guarded component. This approach allows the integration of the guard with other fault tolerance mechanisms (e.g., the guard of the INPUT GUARD pattern for each component that receives the output produced by the guarded component). Furthermore, this approach allows the elimination of redundant checks for errors which can increase the time and space overhead of fault tolerance solutions in a system. This is the case when the guard entities of the INPUT GUARD and OUTPUT GUARD patterns are integrated. Then, each guard will perform on the same data its own checks with respect to different component specification: the former guard checks the data against the specification of the component that produces the output and the latter against the specification of the component that receives the input.

On the other hand, this approach is not applicable to COTS software. Third party developers may not have information about the specification of the other components from which they will receive input, hence they do not know what conditions to check in the guard. A drawback of this implementation approach is the elevated space overhead; the number of guards is not only proportional to the exit points of the guarded component but also to the number of components that receive as input the output produced by the guarded component. On the positive side however, when this implementation approach is applicable it also protects the environment of the guarded component from communication errors that occurred during the forward of the produced output to the guard. Attention is required when this approach is combined with an error detection and fault diagnosis mechanism, because it is difficult to deduce the source of the error, i.e., whether it is a communication error or it is due to a fault in the guarded component. Another advantage of this approach is the fact that the guard can be selectively integrated only with those components which are not robust enough to resist the propagation of errors occurred in the guarded component. This can be used to reduce the elevated space overhead of the approach.

## **Dynamics**

## **Implementation**

## **Example Resolved**

## **Consequences**
The OUTPUT GUARD pattern has the following benefits: 

- It confines an error to the component where it occurred by forwarding to the component’s environment only output that conforms to the specification of the component. 
- The undefined behavior of the guard in the presence of errors allows its combination with error detection and error masking patterns, and fault diagnosis mechanisms (e.g., see [1]). Whenever this is applicable, the system benefits in terms of reduced run-time overhead introduced by the implementation of the fault tolerant mechanism (e.g., the combination of fault containment and error detection in the context of system recovery from errors). 
- The similarities between the guard entities of the INPUT GUARD and OUTPUT GUARD patterns allow the combination of the two in a single entity. This entity will operate on the same data and will perform two checks: one against the specification of the component that produced the data as output and the other against the specification of the component that will consume the data as input. When applicable, this combination can provide significant benefits in terms of time and space overhead since two separate checks will be performed by the same piece of code. 
- There are various ways that the OUTPUT GUARD pattern can be implemented, each providing different benefits with respect to the time or space overhead introduced by the guard. It is also possible to integrate the guard with an existing system without having to modify the internals of the system components (first implementation alternative). That significantly reduced the amount of system re-engineering required for applying the OUTPUT GUARD pattern to COTS-based systems made of black-box components.

The OUTPUT GUARD pattern also imposes some liabilities, similar to those of the INPUT GUARD pattern: 

- It is not possible to minimize both the time and the space overhead of this pattern. To keep low the time overhead introduced by the OUTPUT GUARD pattern, the functionality of the guard must not be very time consuming. This results in a tendency to introduce a separate guard for each different exit point (e.g., one guard per invocation-return or per message-send) of the guarded component. Each such guard checks only a small part of the specification of the guarded component, minimizing thus the execution time of an individual guard. However, this results in a large number of guards, hence in an elevated space overhead. On the other hand, to keep low the space overhead introduced by the OUTPUT GUARD pattern, the number of guards needs to remain as small as possible. This implies that each guard will have to check a larger number of output for the guarded component, becoming a potential bottleneck and thus penalizing the performance of the system with elevated time overhead. 
- For certain systems that require guards to be implemented as components (e.g., systems composed from black-box COTS software), the OUTPUT GUARD pattern results unavoidably to an elevated time and space overhead. The space overhead is due to the introduction of the new components implementing the guards. The time overhead is due to the fact that passing output from the guarded component to its environment requires one additional indirection through the component implementing the guard that check the given output. 
- The OUTPUT GUARD pattern cannot prevent the propagation of errors that do conform with the specification of the guarded component (e.g., see case (a) in Figure 65). Such errors are not due to a malfunction of the guarded component and do not affect its internal state. Although these errors do not have their source in the guarded component which is checked to produce output according to its specification, they can cause a failure on the rest of the system. Such a failure of the entire system will be traced back to an error detected in the guarded component. Unless the error detection and fault diagnosis capabilities of the system allow the detection of faults in the system design, it is highly probable that inappropriate recovery actions will be taken targeted at the guarded component, which, nonetheless, has been operating correctly according to its specification. 
- The OUTPUT GUARD pattern can effectively protect the component’s environment from being contaminated by erroneous output produced by the component according to its specification. However, unless it is combined with some error detection and system recovery mechanisms, this pattern will result in a send-omission failure (i.e., failure to deliver output) of the guarded component. For certain systems, such a failure of one of their components may cause a failure on the entire system. Hence, the OUTPUT GUARD pattern has limited applicability to such systems if it is not combined with other fault tolerance patterns.

## **Known Uses**
Contrary to the relation of the INPUT GUARD with the design by contract principles [4] where the guard maps to the pre-conditions, the OUTPUT GUARD pattern has little relation with them. In the design by contract the post-conditions are properties guaranteed at the end of a successful process execution, rather than being conditions checked to verify whether the execution was successful or not. Applications of the OUTPUT GUARD pattern are found in Double Modular Redundant and Triple Modular Redundant systems where the guard checks whether the output of two or three replicas of the guarded component is identical and, if not, it does not deliver it to the environment of the guarded system. A special case of application for the OUTPUT GUARD pattern is the use of exceptions in various programming languages (e.g., C++, Java) and distributed computing frameworks (e.g., various implementations of the CORBA specifications). In those cases, the guard explicitly knows about output of the guarded component that is exceptional and must be treated by the exception handlers rather than been let through to the environment of the guarded component.

## **See Also**
Besides the obvious similarities with the INPUT GUARD pattern, the OUTPUT GUARD pattern relates to the Fail Stop Processor (FSP) pattern [5]. In the FSP pattern a comparator entity receives the output of two or more identical processor entities and compares it. If all output received is identical, then the output is forward to the environment; otherwise, the comparator stops delivering any further output to the environment. A combination of the guard and comparator entities would enable the self-checking code to identify which of the processors has failed and notify the error detection and system recovery mechanisms in order for them to take the appropriate actions.

The POLICY ENFORCEMENT pattern is a generalization of this pattern.

## **References**

[1] Leveson, N. G., Cha, S. S., Knight, J. C., & Shimeall, T. J. (1990). The use of self checks and voting in software error detection: An empirical study. *IEEE Transactions on Software Engineering*, *16*(4), 432-443.

[2] Yau, S. S., & Cheung, R. C. (1975). Design of self-checking software. *ACM SIGPLAN Notices*, *10*(6), 450-455.

[3] Rosenblum, D. S. (1992). Towards a method of programming with assertions. In *Proceedings of the 14th international conference on Software engineering* (pp. 92-104).

[4] Meyer, B. (1997). *Object-oriented software construction* (Vol. 2, pp. 331-410). Englewood Cliffs: Prentice hall.

[5] Saridakis, T. (2002). A System of Patterns for Fault Tolerance. In *EuroPLoP* (pp. 535-582).

## **Source**
Saridakis, T. (2003, June). Design Patterns for Fault Containment. In *EuroPLoP* (pp. 493-520).
