# **Layered Operating System**

## **Intent**
The LAYERED OPERATING SYSTEM pattern allows the overall features and functionality of the operating system to be decomposed and assigned to hierarchical layers. This provides clearly defined interfaces between each section of the operating system and between user applications and the operating system functions. Layer i uses the services of a lower layer i-1 and does not know of the existence of a higher layer, i+1.

## **Example**
Our operating system is very complex, and we would like to separate different aspects in order to handle them in a more systematic way. Complexity brings vulnerability. We also want to control the calls between operating system components and services to improve security and reliability. Finally, we would like to hide critical modules. We tried a modular architecture, but it did not have enough structure to do all this systematically.

## **Context**
A variety of applications with diverse requirements that need to execute together sharing hardware resources.

## **Problem**
Unstructured modules, as in the MODULAR OPERATING SYSTEM pattern, have the problem that all modules can reach all other modules, which facilitates attacks. We need to conceal the existence of some critical modules. 

The solution to this problem must resolve the following forces: 

- Interfaces should be stable and well-defined. Going through any interface could imply authorization checks. 
- Parts of the system should be exchangeable or removable without affecting the rest of the system. For example, we could have modules that perform more security checks than others. 
- Similar responsibilities should be grouped, to help understandability and maintainability. This contributes indirectly to improved security. 
- We should control module visibility to avoid possible attacks from other modules. 
- Complex components need further decomposition. This makes the design simpler and clearer and also improves security.

## **Solution**
Define a hierarchical set of layers and assign components to each layer. Each layer presents an abstract machine (a set of operations) to the layer above it, hiding the implementation details of the lower layers.

## **Structure**
Figure 128 shows a class diagram for the LAYERED OPERATING SYSTEM pattern. LayerN represents the highest level of abstraction, and Layer1 is the lowest level of abstraction. The main structural characteristic is that the services of LayerN are used only by LayerN+1. Each layer may contain complex entities consisting of different components.

![](./Images/layered_operating_system_structure.png)

*Figure 128: Class diagram for LAYERED OPERATING SYSTEM pattern*

## **Dynamics**
Figure 129 shows the sequence diagram for the use case ‘Open and read a disk file’:

- A user sends an openFile() request to the OSInterface. 
- The OSInterface interprets the openFile() request. 
- The openFile() request is sent from the OSInterface to the FileManager. 
- The FileManager sends a readDisk() request to the DiskDriver.

![](./Images/layered_operating_system_dynamics.png)

*Figure 129: Sequence diagram for the use case ‘Open and read a disk file’*

## **Implementation**
1. List all units in the system and define their dependencies. 
1. Assign units to levels such that units in higher levels depend only on units of lower levels. 
1. Once the modules in a given level are assigned, define a language (set of commands) for the level. This language includes the operations that we want to make visible to the next level above. Add well-defined operation signatures and security checks in these operations to assure the proper use of the level. 
1. Hide those modules that control critical security functions in lower levels.

## **Example Resolved**
We structured the functions of our system as in shown in Figure 130, and now we have a way to control interactions and enforce abstraction. For example, the file system can use the operations of the disk drivers and enforce similar restrictions in the storage of data.

![](./Images/layered_operating_system_example_resolved.png)

*Figure 130: An example of the use of a layered OS architecture*

The user of a file cannot take advantage of the implementation details of the disk driver to attack the system.

## **Consequences**
The LAYERED OPERATING SYSTEM pattern offers the following benefits:

- Lower levels can be changed without affecting higher layers. We can add or remove security functions as needed. 
- There are clearly defined interfaces between each operating system layer and the user applications, which improves security. 
- Control of information is possible using layer hierarchical rules and enforcement of security policies between layers. 
- The fact that layers hide implementation aspects is useful for security, in that possible attackers cannot exploit lower-level details. 

The pattern also has the following potential liabilities: 

- It may not be clear what to put in each layer. In particular, related modules may be hard to allocate. There may be conflicts between functional and security needs when allocating modules. 
- Performance may decrease due to the indirection of calls through several layers. If we try to improve performance, we may sacrifice security.

## **Known Uses**
- The Symbian operating system (Figure 131) uses a variation of the layered approach [1].

![](./Images/layered_operating_system_known_uses_1.png)

*Figure 131: Symbian operating system layered architecture*

- The UNIX operating system (Figure 132) is separated into four layers, with clear interfaces between the system calls to the kernel and between the kernel and the hardware.

![](./Images/layered_operating_system_known_uses_2.png)

*Figure 132: UNIX layered architecture*

- IBM’s OS/2 also uses this approach [2].

## **Variants**
Layer skipping. In this architecture there are special applications that are able to skip layers for added performance. This structure requires a tradeoff between performance and security. By deviating from the strict hierarchy of the layered system, there may not be enforcement of security policies between layers for such applications.

## **See Also**
This pattern is a specialization of the Layers architectural pattern [3].

## **References**

[1] Symbian. (n.d.). <http://www.symbian.com/developer/>

[2] IBM. (n.d.). <http://www-306.ibm.com/software/os/warp/>

[3] Buschmann, F., Meunier, R., Rohnert, H., Sommerlad, P., & Stal, M. (1996). Pattern-Oriented Software Architecture: A System of Patterns, Volume 1. John Wiley & Sons Inc.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
