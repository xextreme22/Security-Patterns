# **Access Control to Physical Structures**

## **Intent**
Applies authentication and authorization to the control of access to physical units including ALARM MONITORING, RELAYS, and time schedules that can control when things will happen.

## **Example**
Building management wants to put in place an access control system to control access to certain zones and to control who can access the zones. They need to deny all access to certain zones after 5pm. They want to generate alarms when someone tries to access a zone for which they do not have permission and start monitoring alarms for all the exterior doors at 8pm. Moreover, they want to turn on the main door light at 7pm.

## **Context**
Physical environment with access control system where we need to control access and turn on/off devices based on time constrains.

## **Problem**
We need a way to enforce business rules in an access control system that take effect at a given time and day of the week. For example, a user may have different access needs on different days, as in the following example: 

- 08:30 - 17:00 - Monday, Wednesday, Friday - Standard Hours 
- 08:30 - 19:00 - Tuesday, Thursday - Stays Late 2 Days a Week 
- 08:00 - 12:00 - half a day on Saturday.

The following forces affect the solution: 

- We need a way to automatically cancel access for everyone to some areas of a building at given time and day of the week. Some users might have access to some areas only during a time range of the day. 
- We need to provide a way to automatically activate devices based on the time and day of the week. 
- We need a way to represent a day of the week and time. 
- This pattern expands the ACCESS CONTROL FOR PHYSICAL STRUCTURES Pattern in [1]. Therefore, all the forces presented in that pattern are present in this pattern as well. 
- We need to restrict access to some users depending on the identity or other characteristics of the visitor. 
- The boundaries of the unit of access must be clearly defined. 
- There is a variety of users such as employees, contractors, repairpersons, etc., that require access to different parts of a building. 
- Since some units will be normally closed, it is necessary to create policies and plans in case of an emergency, such as earthquake or fire. 
- We may need to keep track of who entered each unit and when

## **Solution**
Define the structure of an access control system using the Role Based Access Control pattern. Integrate the Alarms Monitoring and RELAYS patterns and introduce the concept of a time schedule to control when things can/must happen. Time Schedules have two uses; one is to control access times and the other is to configure automatic actions.

## **Structure**
Figure 38 shows a UML class diagram for the ACCESS CONTROL TO PHYSICAL STRUCTURES pattern. We can see how the Alarms Monitoring pattern can be integrated so that Zones can control Alarms. We also use the RELAYS pattern so that each door has its own Door Relay, and the Aux RELAYS can be used to turn on/off devices. Zones can control RELAYS.

We introduce a TimeSchedule class that consists of a few time intervals. A Time Interval consists of a start time, a stop time, and selected day of the week. When the system clock activates a Time Schedule, it can automatically perform some actions. RELAYS can be turned on and off, alarm zones can be activated or deactivated, and doors can be unlocked. To control access times, roles are combined with Time Schedule to determine both where and when a user can gain access to zones, and we also need to be able to assign a Time Schedule for the entire zone that applies to all users.

![](./Images/access_control_to_physical_structures_structure.png)

*Figure 38: Class Diagram for ACCESS CONTROL TO PHYSICAL STRUCTURES pattern*

## **Dynamics**
Figure 39 shows a main success scenario for the use case “enter a zone”. When the control is passed to the zone, the Zone and the Role will consider their Time Schedules before authorizing a person. Also, when the Zone opens or closes a door, the Door calls its Relay to perform the action. Other use cases include: “zone access denied by zone time schedule”, “role access denied by time schedule”, and “activate alarm by role access denied”; they are not shown for conciseness.

![](./Images/access_control_to_physical_structures_dynamics.png)

*Figure 39: Sequence Diagram for Entering a Zone*

## **Implementation**
Access control systems use centralized processing, distributed processing, or hybrid arrangement. The system architecture should be taken into consideration when designing an access control system, since it can have a significant effect upon operation during a catastrophic system failure.

Centralized Processing. In computer dependent processing systems, all events are gathered by the field panels, and are then sent to the computer for processing. For example, if a credential is presented at a door, the door sends the credentials to the central computer or processor. The computer checks the credential against its programming and determines if it is allowed through that door at that time. If valid the computer sends a command back to the panel to release the door. In these systems if the computer goes down, or if communications between the panel and computer is lost, the system can no longer function, to verify proper access, and to process alarms.

Distributed Processing. In distributed processing systems the database is loaded to the field panel. All decisions are made at the field panel and are passed to the computer or logging printer for storage. In these systems if communications are lost, access control continues uninhibited. Furthermore, the events can sometimes be stored in the panels, and can be sent up to the computer once communications are restored. Due to their architecture, systems which employ distributed processing generally offer better reliability, and faster response than systems that rely on central computers for all decision making.

## **Example Resolved**
Building management can configure time schedules and assign them to Zones and Roles, that a way a Zone could have a time schedule from 8-5pm. Moreover, time schedules can be assigned to RELAYS and Alarms so that they can be activated and monitored respectively when the system clock activates these time schedules.

## **Consequences**
Advantages include: 

- We can activate RELAYS for a predefined amount of time. 
- Every alarm change of state is logged, and interested parties are advised. 

A disadvantage is: 

- We might want the same kind of functionality for devices that are not exactly door or aux RELAYS.
- The pattern may create overhead for systems.

## **Known Uses**
Many commercial or institutional buildings control access to restricted units using the concepts presented here. This model corresponds to an architecture that is seen in commercial products, such as: Secure Perfect, Picture Perfect and Diamond II. BACnet is a standard that includes access control to buildings and incorporates RBAC, zones, and schedules [2].

## **See Also**
This pattern is a combination of the Role based access control (or another suitable authorization model), the ACCESS CONTROL FOR PHYSICAL STRUCTURES pattern [1], the ALARM MONITORING pattern, and the RELAYS pattern.

See SINGLE ACCESS POINT pattern.

## **References**

[1] Desouza-Doucet, A. C. (2006). *Controlling access to physical locations*. Florida Atlantic University.

[2] Ritter, D., Isler, B., Mundt, H., & Treado, S. (2006). Access Control In BACnet®. *Ashrae Journal*, *48*(11), B26.

## **Sources**
Fernandez, E. B., Ballesteros, J., Desouza-Doucet, A. C., & Larrondo-Petrie, M. M. (2007, July). Security patterns for physical access control systems. In *IFIP Annual Conference on Data and Applications Security and Privacy* (pp. 259-274). Springer, Berlin, Heidelberg.

