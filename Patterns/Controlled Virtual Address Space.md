# **Controlled Virtual Address Space**

## **Intent**
This pattern addresses how to control access by processes to specific areas of their virtual address space (VAS) according to a set of predefined access types. Divide the VAS into segments that correspond to logical units in the programs. Use special words (descriptors) to represent access rights for these segments.

## **Example**
Our operating system improved by using a REFERENCE MONITOR. However, hackers discovered that the unit of access control to memory was coarse. By taking advantage of the lack of precision in controlling access they were able to access other processes’ areas.

## **Context**
Multiprogramming systems with a variety of users. Processes executing on behalf of these users must be able to share memory areas in a controlled way. Each process runs in its own address space. The total VAS at a given moment includes the union of the VASs of the individual processes, including user and system processes. Typical allowed accesses are read, write, and execute, although finer typing is possible.

## **Problem**
Processes must be controlled when accessing memory, otherwise they could overwrite each other’s memory areas or gain access to private information. While relatively small amounts of data can be directly compromised, illegal access to system areas could allow a process to force a higher execution privilege level and thus access files and other resources. 

The solution to this problem must resolve the following forces: 

- There is a need for a variety of access rights for each separate logical unit of VAS (segment). In this way security and controlled sharing are possible. 
- There is a variety of virtual memory address space structures: some systems use a set of separate address spaces, others a single-level address space. Further, the VAS may be split between the users and the operating system. We would like to control access to all of these types in a uniform manner.
- For any approach to be efficient, hardware assistance is necessary. This implies that an implementation of the solution will require a specific hardware architecture. However, the generic solution must be hardware independent.
 
## **Solution**
Divide the VAS into segments that correspond to logical units in the programs. Use special words (descriptors) to indicate access rights that show the starting address of the accessible segment, the limit of the accessible segment, and the type of access permitted (read, write, execute).

## **Structure**
Figure 180 below shows a class diagram for the solution. A process (the Process class) must have a descriptor (the Descriptor class) to access segments in the VAS.

![](./Images/controlled_virtual_address_space_structure.png)

*Figure 180: Class diagram for CONTROLLED VIRTUAL ADDRESS SPACE pattern*

## **Dynamics**

## **Implementation**
Some implementation aspects include: 

- The limit check when accessing an address must be done by the instruction microcode or the overhead would not be acceptable. This check is part of an instance of REFERENCE MONITOR. 
- The same idea applies to purely paging systems, except that the limit in the descriptor is defined by the page size. In paged systems pages do not correspond to logical units and cannot perform a fine security control.
- There are two basic ways to implement this pattern: 
  - Property descriptor systems. The descriptors are loaded at process creation by the operating system. The descriptors are handled through special registers and disappear at the end of execution. 
  - Capability systems. A special trusted portion of the operating system distributes capabilities to programs. Programs own these capabilities. To use them, the operating system loads them into special registers or memory segments. 
  - In both cases, access to files is derived from their ACLs.

## **Example Resolved**
Descriptors can control areas of memory of any size. A process without a descriptor for an area cannot access it. If sharing is required, several processes can have a descriptor with the same addresses but with different access rights.

## **Consequences**
The following benefits may be expected from applying this pattern: 

- The pattern provides the required segment protection because a process cannot access a segment without a descriptor for it. Two processes with descriptors with the same memory address base–limit pair can conveniently share a segment. 
- The pattern applies to any type of virtual address space: single, segregated, or split. 
- If all resources are mapped to the virtual address space, the pattern can control access to any type of resource, including files.

The following potential liabilities may arise from applying this pattern: 

- Segmentation makes storage allocation inefficient because of external fragmentation [1]. In most systems segments are paged for convenient allocation. 
- Hardware support is needed, which puts an extra requirement on this solution. 
- In systems that use multiple separate address spaces, it is necessary to add an extra identifier to the descriptor registers to indicate the address space number.

## **Known Uses**
The Plessey 250 [2], Multics [3], IBM S/38, IBM S/6000, Intel X86 [4], and Intel Pentium use some type of descriptors for memory access control. The operating systems in these machines must use this approach for memory management. Specific uses include the Choices operating system [5] and AIX [6].

## **See Also**
This pattern is a direct application of AUTHORIZATION to the processes’ address space.

## **References**

[1] Silberschatz, A., Galvin, P., Gagne, G. (2003). Operating System Concepts (6th Edition). John Wiley & Sons.

[2] Hodges, K. H. (1973). A fault-tolerant multiprocessor design for real-time control. *Computer Design*, *12*(12), 75-81.

[3] Graham, R. M. (1968). Protection in an information processing utility. *Communications of the ACM*, *11*(5), 365-369.

[4] Childs, R. E., Crawford, J. O. H. N., House, D. L., & Noyce, R. N. (1984). A processor family for personal computers. *Proceedings of the IEEE*, *72*(3), 363-376.

[5] Russo, V. F., & Campbell, R. H. (1989). Virtual memory and backing storage management in multiprocessor operating systems using object-oriented design techniques. *ACM Sigplan Notices*, *24*(10), 267-278.

[6] Camillone, N. A., Steves, D. H., & Witte, K. C. (1990). AIX operating system: A trustworthy computing system. *IBM RISC System/6000 Technology*, 168-172.

## **Source**
Schumacher, M., Fernandez-Buglioni, E., Hybertson, D., Buschmann, F., & Sommerlad, P. (2006). Security Patterns: Integrating Security and Systems Engineering (1st ed.). Wiley.
