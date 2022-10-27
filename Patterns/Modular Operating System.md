# **Modular Operating System**

## **Intent**
The MODULAR OPERATING SYSTEM pattern describes how to separate operating system services into modules, each representing a basic function or component. The basic core kernel only has the required components to start itself and the ability to load modules. The core is the one module always in memory. Whenever the services of any additional modules are required, the module loader loads the appropriate module. Each module performs a function and may take parameters.

## **Example**
Our group is building a new operating system that should support various types of devices requiring dynamic services with a wide variety of security requirements. We want to dynamically add operating system components, functions, and services, as well as tailor their security aspects according to the type of application. For example, a media player may require support to prevent copying of its contents, or a module for which a vulnerability alert has been issued could be removed.

## **Context**
A variety of applications with diverse requirements that need to execute together, sharing hardware resources.

## **Problem**
We need to be able to add or remove functions easily so that we can accommodate applications with a number of security requirements. How can we structure the operating system functions for this purpose? 

The solution to this problem must resolve the following forces: 

- Operating systems for PCs and other types of uses require a large variety of plugins. New plugins appear frequently, and we need the ability to add and remove them without disrupting normal operation. 
- Some of the plugins may contain malware; we need to isolate their execution, so they do not affect other processes. 
- We would like to hide security-critical modules from other modules to avoid possible attacks. 
- Modules can call each other, which is a possible source of attacks.

## **Solution**
Define a core module that can load, and link modules dynamically as needed.

## **Structure**
Figure 137 shows a class diagram for this pattern. The KernelCore is the core of the modular operating system. A set of LoadableModules is associated with the KernelCore, indicating the modules that can be loaded according to their applications and the functions required. Any LoadableModule can call any other LoadableModule.

![](./Images/modular_operating_system_structure.png)

*Figure 137: Class diagram for the MODULAR OPERATING SYSTEM pattern*

## **Dynamics**

## **Implementation**
1. Separate the functions of the operating system into independent modules according to whether: 
   1. They are complete functional units. 
   1. They are critical with respect to security. 
   1. They should execute in their own process for security reasons, or their own thread for performance reasons. 
   1. They should be isolated during execution because they may contain malware. 
1. Define a set of loadable modules. New modules can be added later, according to the needs of specific applications. 
1. Define a communication structure for the resultant modules. Operations should have well-defined call signatures and all calls should be checked. 
1. Define a preferred order for loading some basic modules. Modules that are critical for security should be loaded only when needed to reduce exposure to attacks.

## **Example Resolved**
We structured the functions of our system following the MODULAR OPERATING SYSTEM pattern. Because each module could have its own address space, we can isolate its execution. Because each module can be designed independently, they can have different security constraints in their structure. This structure gives us flexibility with a good degree of security

## **Consequences**
The MODULAR OPERATING SYSTEM pattern offers the following benefits: 

- Flexibility to add and remove functions contributes to security, in that we can add new versions of modules with better security. 
- Each module is separate and communicates with other modules over known interfaces. We can introduce controls in these interfaces. 
- It is possible to partially hide critical modules by loading them only when needed and removing them after use. 
- By giving each executing module its own address space, we can isolate the effects of a rogue module. 

This pattern also has the following potential liabilities: 

- Any module can ‘see’ all the others and potentially interfere with their execution. 
- Uniformity of call interfaces between modules makes it difficult to apply stronger security restrictions to critical modules.

## **Variants**
Monolithic Kernel. The operating system is a collection of procedures. Each procedure has a well-defined interface in terms of parameters and results and each one is free to call any other [1]. There is no organization relating the operating system, components, services, and user applications: all the modules are at the same level. The difference between monolithic and modular operating system architectures is that in the monolithic approach, all the modules are loaded together at installation time, instead of on demand. This approach is not very attractive for secure systems.

## **Known Uses**
The Solaris 10 operating system (Figure 138) is designed following this pattern. Its kernel is dynamic and composed of a core system that is always resident in memory [2]. The various types of Solaris 10 loadable modules are shown in Figure 138 as loaded by the kernel core: the diagram does not represent the communication links between individual modules.

![](./Images/modular_operating_system_known_uses.png)

*Figure 138: The modular design of the Solaris 10 operating system*

- Extreme Ware from Extreme Networks [3]. 
- Some versions of Linux use a combination of modular and monolithic architectures.

## **See Also**

## **References**

[1] Tanenbaum, A. (2009). *Modern operating systems*. Pearson Education, Inc.

[2] Trusted Solaris Operating System. (n.d.). <http://www.sun.com/software/solaris/trustedsolaris/>

[3] Extreme Networks. (n.d.). <http://www.extremenetworks.com/products/OS/> 

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
