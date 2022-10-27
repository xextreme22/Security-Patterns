# **Virtual Machine Operating System**

## **Intent**
The VIRTUAL MACHINE OPERATING SYSTEM pattern describes how to provide a set of replicas of the hardware architecture (virtual machines) that can be used to execute multiple and possibly different operating systems with strong isolation between them.

## **Example**
A web server is hosting applications for two competing companies. These companies use different operating systems. We want to ensure that neither of them can access the other company’s files or launch attacks against the other system.

## **Context**
Mutually suspicious sets of applications that need to execute in the same hardware. Each set requires isolation from the other sets.

## **Problem**
Sometimes we need to execute different operating systems on the same hardware. How can we keep those operating systems isolated in such a way that their executions don’t interfere with each other? The solution to this problem must resolve the following forces: 

- Each operating system needs to have access to a complete set of hardware features to support its execution. 
- Each operating system has its own set of machine-dependent features, such as interrupt handlers. In other words, each operating system uses the hardware in different ways. 
- When an operating system crashes or it is penetrated by a hacker, the effects of this situation should not propagate to other operating systems running on the same hardware. 
- There should be no way for a malicious user in one virtual machine to get access to the data or functions of another virtual machine.

## **Solution**
Define an architectural layer that is in control of the hardware and supervises and coordinates the execution of each operating system environment. This extra layer, usually called a virtual machine monitor (VMM) or hypervisor, presents to each operating system a replica of the hardware. The VMM intercepts all system calls and interprets them according to the operating system from which they came.

## **Structure**
Figure 173 shows a class diagram for the VIRTUAL MACHINE OPERATING SYSTEM (VMOS) pattern. The VMOS contains one VirtualMachineMonitor (VMM) and multiple virtual machines (VM). Each VM can run a local operating system (LocalOS). The VirtualMachineMonitor supports each LocalOS and is able to interpret its system calls. As a LocalProcess runs on a LocalOS, the VM passes the operating system calls to the VMM, which executes them in the hardware.

![](./Images/virtual_machine_operating_system_structure.png)

*Figure 173: Class diagram for the VIRTUAL MACHINE OPERATING SYSTEM pattern*

## **Dynamics**
Figure 174 shows the sequence diagram for the use case ‘Perform an OS call on a virtual machine’. A local process wishing to perform a system operation uses the following sequence:

1. A LocalProcess makes an operating system call to the LocalOS. 
1. The LocalOS maps the operating system call to the VMM (by executing a privileged operation). 
1. The VMM interprets the call according to the local operating system from which it came, and it executes the operation in hardware. 
1. The VMM sends return codes to the LocalOS to indicate successful instruction execution, as well as the results of the instruction execution. 
1. The LocalOS sends the return code and data to the LocalProcess.

![](./Images/virtual_machine_operating_system_dynamics.png)

*Figure 174: Sequence diagram for the use case ‘Perform an OS call on virtual machine’*

## **Implementation**
1. Select the hardware that will be virtualized. All of its privileged instructions must trap when executed in user mode (this is the usual way to intercept system calls). 
1. Define a representation (data structure) for describing operating system features that map to hardware aspects, such as meaning of interrupts, disk space distribution, and so on, and build tables for each operating system to be supported. 
1. Enumerate the system calls for each supported operating system and associate them with specific hardware instructions.

## **Example Resolved**
In the example shown in Figure 175, two companies using UNIX and Linux can execute their applications in different virtual machines. The VMM provides strong isolation between these two execution environments.

![](./Images/virtual_machine_operating_system_example_resolved.png)

*Figure 175: Virtual Machine operating system example*

## **Consequences**
The VIRTUAL MACHINE OPERATING SYSTEM pattern offers the following benefits: 

- The VMM intercepts and checks all system calls. The VMM is in effect a REFERENCE MONITOR and provides total mediation for the use of the hardware. This can provide strong isolation between virtual machines [1]. 
- Each environment (virtual machine) does not know about the other virtual machine(s), this helps prevent cross-VM attacks. 
- There is a well-defined interface between the VMM and the virtual machines. 
- The VMM is small and simple and can be checked for security. 
- The architecture defined by this pattern is orthogonal to the other three architectures discussed earlier and can execute any of them as local operating systems. 

The pattern also has the following potential liabilities: 

- All the virtual machines are treated equally. If virtual machines with different security categories are required, it is necessary to build specialized versions. This approach is followed in KVM/370 (see Variants). 
- Extra overhead in the use of privileged instructions. 
- It is complex to let virtual machines communicate with each other if this is needed.

## **Variants**
- The architecture defined by this pattern is orthogonal to the other three architectures discussed earlier and can execute any of them as local operating systems. 
- KVM/370 was a secure extension of VM/370 [2]. This system included a formally verified security kernel, and its virtual machines executed in different security levels, for example top secret, confidential, and so on. In addition to the isolation provided by the VMM, this system also applied the MULTILEVEL SECURITY model.

## **Known Uses**
- IBM VM/370 [3]. This was the first VMOS and provided virtual machines for an IBM 370 mainframe. 
- VMware [4]. This is a current range of products that provide virtual machines for Intel X86 hardware. 
- Solaris 10 [5] calls the virtual machines ‘containers’, and one or more applications execute in each container. 
- Connectix [6] produces virtual PCs to run Windows and other operating systems. 
- Xen is a VMM for the Intel x86 developed as a project at the University of Cambridge, UK [7]. 
- Some smart phone operating systems use virtual machines to separate users’ private system from their work environment. These include the L4 Microvisor and RIM’s BlackBerry 10 OS [8].

## **See Also**
- REFERENCE MONITOR. The VMM is a concrete version of a reference monitor. 
- The operating system patterns described can be used to implement the structure of a VMOS architecture. 

## **References**

[1] Rosenblum, M., & Garfinkel, T. (2005). Virtual machine monitors: Current technology and future trends. *Computer*, *38*(5), 39-47.

[2] Gold, B. D., Linde, R. R., Peeler, R. J., Schaefer, M., Scheid, J. F., & Ward, P. D. (1979, June). A security retrofit of VM/370. In *1979 International Workshop on Managing Requirements Knowledge (MARK)* (pp. 335-344). IEEE.

[3] Creasy, R. J. (1981). The origin of the VM/370 time-sharing system. *IBM Journal of Research and Development*, *25*(5), 483-490.

[4] Nieh, J., & Leonard, O. C. (2000). Examining vmware. *Dr. Dobb’s Journal*, *25*(8), 70.

[5] Trusted Solaris Operating System. (n.d.). <http://www.sun.com/software/solaris/trustedsolaris/>

[6] Connectix Corporation. (n.d.). The Technology of Virtual Machines. white paper. San Mateo. CA. <http://www.connectix.com>

[7] Barham, P., Dragovic, B., Fraser, K., Hand, S., Harris, T., Ho, A., ... & Warfield, A. (2003). Xen and the art of virtualization. *ACM SIGOPS operating systems review*, *37*(5), 164-177.

[8] Wikipedia contributors. (n.d.). QNX. Wikipedia. https://en.wikipedia.org/wiki/QNX

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
