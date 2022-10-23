# **Relays**

## **Intent**
This pattern defines the interactions with electronically controlled switches.

## **Example**
Building management wants to be able to open the main gate for visitors that do not have credentials. Moreover, doors should be opened when someone presents valid credentials.

## **Context**
Physical environment with an access control system where we want to be able turn on/off devices; and lock/unlock doors.

## **Problem**
A relay is an electronically controlled switch. Similar to a light switch on the wall being used to turn on or off a light, a relay can be used to turn on or off other devices. Relays are used to activate the electric door lock, or to activate a variety of other items such as: bells, sirens, turn lights on and off, trip a digital dialer and many other uses. Typically, one relay is used to control the electric strike of doors. The others, usually called auxiliary relays, can be used as needed. When a relay is activated or deactivated, the device wired to it is turned on or off.

The following forces will affect the solution: 

- We need to distinguish between door relays and auxiliary relays. 
- Relays can be activated and deactivated. 
- Relays can be activated indefinitely or for a defined period of time. 
- Relays have two possible states: on and off.

## **Solution**
Have a relay entity that describes the general concepts of a relay and use generalization to separate door relays from auxiliary relays and their particular characteristics.

## **Structure**
Figure 37 shows a UML class diagram for the RELAYS pattern. Abstract class Relay indicates a general class for any type of relay. The DoorRelay class and the AuxRelay class inherit the Relay class. These classes allow for relays to be controlled.

![](./Images/relays_structure.png)

*Figure 37: Class Diagram for RELAYS pattern*

## **Dynamics**
The sequence diagram is trivial. The request to set the relay active is sent by an external actor. The relay is activated for the previously defined “on time.” If the relay was already active the timer is restarted. Once the timer expires the relay is turned off.

## **Implementation**
Relays could be activated or deactivated by a variety of events. An alarm input, a valid credential, an egress button being pushed, or a time event, can all activate a relay.

## **Example Resolved**
Doors that have relays defined can be opened when a valid credential is presented. Also, the main gate can be assigned an auxiliary relay that can be activated at will.

## **Consequences**
Advantages include: 

- We can distinguish between auxiliary and door relays. 
- We can activate relays for a predefined amount of time. 
- This model provides the basic structure for supporting relays in an access control system. 

A disadvantage is: 

- We might want the same kind of functionality for devices that are not exactly door or aux relays.

## **Known Uses**
Many commercial Access Control systems have the concept of relay management and distinction between door and aux relays.

## **See Also**
The ACCESS CONTROL TO PHYSICAL STRUCTURES pattern complements this pattern by adding the concept of Zones that control Relays.

## **References**

## **Sources**
Fernandez, E. B., Ballesteros, J., Desouza-Doucet, A. C., & Larrondo-Petrie, M. M. (2007, July). Security patterns for physical access control systems. In *IFIP Annual Conference on Data and Applications Security and Privacy* (pp. 259-274). Springer, Berlin, Heidelberg.
