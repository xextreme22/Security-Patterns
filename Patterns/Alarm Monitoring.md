# **Alarm Monitoring**

## **Intent**
Defines a way to raise events in the system that might require special attention, such as someone tampering with a door.

## **Example**
Building management wants to raise alarm events when someone opens a door without using the right credentials or when someone tries to use a card that was reported as lost.

## **Context**
Physical environment with an access control system where we want to be able to raise filtered alarm events and we want to differentiate between alarms that are generated because of a physical violation of the system and alarms generated because of a system rule violation.

## **Problem**
In an Access control system, there are two types of alarms, physical and logical. Many times, we might want to inform more than one subsystem that a change of state happened in order to generate different types of configurable actions that can vary. Physical alarm inputs are used to monitor various devices. These alarms can be shunted. Logical alarms are used to monitor various system rules. For example, a system may generate an alarm after three invalid tries to get access with the same credentials.

The following forces will affect any solution: 

- There are two types of alarms, physical and logical. 
- Alarms can be ignored. 
- Logical alarms have two possible states, reset and alarm.
- Physical alarms have four possible states: reset, alarm, cut or short. The last two states are known as trouble states, caused by faulty wiring or tampering. 
- We may need to know what alarms have been set and when they were reset. 
- We need a way to inform interested parties about a change of a state of an alarm.

## **Solution**
Have an alarm entity that describes the general concepts of an alarm and use generalization to separate logical alarms from physical alarms and their particular characteristics. Add information about the time the alarm occurred. Use the Observer Pattern [1] to advise any interested party of a change of state in an Alarm.

## **Structure**
Figure 35 shows a UML class diagram for the ALARM MONITORING pattern. Abstract class Alarm indicates a general class for any type of alarm. The PhysicalAlarm class and the LogicalAlarm class inherit the Alarm class. These classes allow for alarms to be controlled and monitored. By implementing the AlarmObserver interface any class (not shown here) interested on this alarm can be added to the list of observers for the alarm and will be advised when a change of state happens. Moreover, we log every alarm activity with a time stamp.

![](./Images/alarm_monitoring_structure.png)

*Figure 35: Class Diagram for ALARM MONITORING pattern*

## **Dynamics**
Figure 36 shows the main success scenario for the use case “activate an alarm”. The request to set the alarm active is sent by an external actor that detected the need to raise the alarm. The change of state is logged and only if the alarm is being monitored interested parties are advised of the change, otherwise the alarm is ignored.

![](./Images/alarm_monitoring_dynamics.png)

*Figure 36: Sequence Diagram for Activating an Alarm*

## **Implementation**
There are many ways to create alarms in a physical access control system. For example, when a card reader is installed on a door, an alarm contact is usually installed as well. The alarm point is used to monitor whether the door was forced or held for too long after a valid access was granted. Logical Alarms can be generated for maximum tries with invalid credential, invalid credential, and communication errors. The call to change the state of an alarm could in turn generate other actions when observers are notified, like displaying a message, activating a siren, etc.

## **Example Resolved**
Every time someone opens a door without proper permission an alarm can be created and if a person uses a lost badge the system can generate a logical alarm.

## **Consequences**
Advantages include: 

- We can only pay attention to the alarms we are interested in.
- Every alarm change of state is logged, and interested parties are advised. 
- This model provides the basic structure for supporting alarms in an access control system. 
- We make a distinction between logical and physical alarms, which supports the creation of any alarm independently of the existing hardware.

Disadvantages include: 

- The pattern may create overhead for systems that only care about logging the alarm.

## **Known Uses**
Many commercial Access Control systems have the concept of logical and physical alarms that generate some actions when the states are changed.

## **See Also**
This pattern is based on the Observer Pattern [1]. The ACCESS CONTROL FOR PHYSICAL STRUCTURES [2] complements this pattern by adding the concept of Zones that control Alarms.

## **References**

[1] Gamma, E., Helm, R., Johnson, R., Johnson, R. E., & Vlissides, J. (1995). *Design patterns: elements of reusable object-oriented software*. Pearson Deutschland GmbH.

[2] Desouza-Doucet, A. C. (2006). *Controlling access to physical locations*. Florida Atlantic University.

## **Sources**
Fernandez, E. B., Ballesteros, J., Desouza-Doucet, A. C., & Larrondo-Petrie, M. M. (2007, July). Security patterns for physical access control systems. In *IFIP Annual Conference on Data and Applications Security and Privacy* (pp. 259-274). Springer, Berlin, Heidelberg.
