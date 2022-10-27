# **Virtual Address Space Structure Selection**

## **Intent**
The VIRTUAL ADDRESS SPACE STRUCTURE SELECTION pattern describes how to select the virtual address space for operating systems that have special security needs. Some systems emphasize isolation, others information sharing, others good performance. The organization of each process’ virtual address space (VAS) is defined by the hardware architecture and has an effect on performance and security. The pattern enables all the hardware possibilities to be considered and selected according to need.

## **Example**
We have a system running applications using images that require large graphic files. The application also has stringent security requirements because some of the images are sensitive and should be only accessed by authorized users. We need to decide on an appropriate VAS structure.

## **Context**
Virtual memory allows the total size of the memory used by processes to exceed the size of physical memory. Upon use, the virtual address is translated by the address translation unit (usually the memory management unit (MMU) in microprocessors) to obtain a physical address that is used to access physical memory. To execute a process, the kernel creates a per-process virtual address space. We have a multiprogramming system with a variety of users and applications. Processes execute on behalf of users and at times must be able to share memory areas, at other times must be isolated, and in all cases, we need access control. Performance may also be an issue.

## **Problem**
We need to select the virtual address space for processes depending on the majority of the applications we intend to execute, otherwise we can have mismatches that may result in poor security or performance. 

The solution to this problem must resolve the following forces: 

- Each process needs to be assigned a relatively large VAS to hold its data, stack, space for temporary variables, variables to keep the status of its devices, and other information. 
- In multiprogramming environments processes have diverse requirements; some require isolation, others information sharing, others good performance. 
- Data typing is useful to prevent errors and improve security. Several attacks occur by executing data and modifying code [1]. 
- Sharing between address spaces should be convenient, otherwise performance may suffer.

## **Solution**
Select from four basic approaches that differ in their security features:

- One address space per process (Figure 146). The supervisor (kernel plus utilities) and each user process get their own address spaces. Using one VAS per process has the following trade-offs:
  - Good process isolation. 
  - Some protection against possible illegal actions by a compromised operating system. 
  - Simplicity. 
  - Sharing is complex (special instructions to cross spaces are needed).

![](./Images/virtual_address_space_structure_selection_solution_1.png)

*Figure 146: One address space per process*

- Two address spaces per process (Figure 147). Each process gets a data and a code (program) virtual address space. Use of two VASs per process has the following trade-offs:
  - Good process isolation. 
  - Some protection against possible illegal actions by a compromised operating system. 
  - Data and instructions can be separated for better protection (some attacks take advantage of execution of data or modification of code). Data typing is also good for reliability. 
  - A disadvantage is complex sharing, plus poor address space utilization.

![](./Images/virtual_address_space_structure_selection_solution_2.png)

*Figure 147: Two address spaces per process*

- One address space per user process, all of them shared with one address space for the operating system (Figure 148). The operating system (supervisor) can be shared between all processes. This scheme has the following trade-offs:
  - Good process isolation. 
  - Good sharing of resources and services. 
  - Suboptimal with respect to security (the supervisor has complete access to the user processes, and it must be trusted). 
  - The address space available to each user process has been halved.

![](./Images/virtual_address_space_structure_selection_solution_3.png)

*Figure 148: One address space per user process, all of them shared with one address space for the operating system*

- A single-level address space (Figure 149). Everything, including files, is mapped to one memory space. Use of a single-level address space has the following trade-offs:
  - Good process isolation. 
  - Logical simplicity. 
  - Uniform protection (all I/O is mapped to memory). 
  - Offers the most elegant solution (only one mechanism to protect memory and files) and is potentially the most secure if capabilities are also used. 
  - Hard to implement in hardware due to the large address space required.

![](./Images/virtual_address_space_structure_selection_solution_4.png)

*Figure 149: A single-level address space*

## **Structure**

## **Dynamics**

## **Implementation**
The VAS is implemented by the hardware architecture. The operating system designer can choose one of the architectures based on the requirements of the applications, according to the trade-offs discussed above. In a particular case, the choice may be influenced by company policies, cost, performance, and other factors, as well as security.

## **Example Resolved**

## **Consequences**
In addition to the specific benefits described as part of the solution (tradeoffs), the VIRTUAL ADDRESS SPACE STRUCTURE SELECTION pattern has the following general liabilities: 

- Without hardware support it is not feasible to separate the virtual address spaces of the processes. Most processors use register pairs or descriptors that indicate the base (start) of a memory unit (segment) and its length or limit [2]. 
- If the mix of applications is not well-defined, it is hard to select the best solution. Considerations other than security then become more important.
 
## **Known Uses**
- One address space per process. The NS32000, WE32100 and Clipper microprocessors [3]. Several versions of UNIX were implemented in these processors. 
- Two address spaces per process. Used in the Motorola 68000 series. The Minix 2 operating system uses this approach [4]. 
- One address space per user process, all of them shared with one address space for the operating system. Used in the VAX series and in Intel processors. Windows runs in this type of address space. 
- Single-level address space. Multics, IBM S/38, IBM S/6000, and HP PA-RISC use this approach. Multics had its own operating system. IBM AIX ran in S/6000 [5]. The PA-RISC architectures ran a version of UNIX.

## **See Also**
- SECURE PROCESS/THREAD. The interaction between processes depends strongly on the virtual address space configuration, which can affect security, performance and sharing properties of the processes. 
- VIRTUAL ADDRESS SPACE ACCESS CONTROL. A VAS is assigned to each process that can be accessed according to the rights of the process. The VIRTUAL ADDRESS SPACE STRUCTURE SELECTION pattern is applied first to select the appropriate structure. Once selected, the VAS is secured using the CONTROLLED VIRTUAL ADDRESS SPACE pattern. 
- The SECURE STORAGE pattern for rebooting.

## **References**

[1] Gollmann, D. (2010). Computer security. *Wiley Interdisciplinary Reviews: Computational Statistics*, *2*(5), 544-554.

[2] Silberschatz, A., Galvin, P., & Gagne, G. (2008). Operating System Concepts. 8th edition. John Wiley & Sons Inc.

[3] Fernandez, E. B. (1985). Microprocessor architecture: The 32-bit generation. *VLSI Systems Design*, 34-44.

[4] Tanenbaum, A. S., Herder, J. N., & Bos, H. (2006). Can we make operating systems reliable and secure?. *Computer*, *39*(5), 44-51.

[5] Camillone, N. A., Steves, D. H., & Witte, K. C. (1990). AIX operating system: A trustworthy computing system. *IBM RISC System/6000 Technology*, 168-172.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
