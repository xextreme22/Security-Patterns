# **Protection Rings**

## **Intent**
The PROTECTION RINGS pattern allows control of how processes call other processes and how they access data. Crossing of rings is done through gates that check the rights of the crossing process. A process calling another process or accessing data in a higher ring must go through a gate.

## **Example**
The ChronOS designers found that for applications that use programs with a variety of origins, there is a high overhead in applying elaborate checks to all of them. It would be more efficient to apply the checks selectively, depending on how much they trust the programs making the calls, but this is not usually known at execution time. If they could find a way to classify processes according to trust, they could improve the application of checks. It is not enough to rely on program features to enforce entering the right entry points, because applications may come in a variety of languages, some of which may allow skipping entry points.

## **Context**
Executing processes in a computing system. Processes need to call other processes to ask for services or to collaborate in the computation of an algorithm, and usually share data and other resources. Some processes may be malicious or contain errors that may affect process execution. This pattern applies only to centralized environments, as opposed to distributed systems.

## **Problem**
Defining a set of protected entry points is not enough if we cannot enforce their use. How can we prevent a process from calling another on an entry point that has no checks? We cannot rely on language features unless we only use a restricted set of languages, which is not practical in general. If all processes are alike, we also need to apply the same checks to all of them, which may be overkill. 

The solution to this problem must resolve the following forces:

- We want to be able to enforce the application of protected entry points, at least for some processes. In this way, requests from suspicious processes can always be controlled. 
- We would like to separate processes according to their level of trust and check only calls from a low-level to a higher-level process. This can reduce execution-time overhead considerably. 
- In each higher level we can check signature validity, as well as control access or apply reliability tests. These actions should result in a more secure execution environment.
 
## **Solution**
Define a set of hierarchical protection domains, called protection rings (typically 4 to 32) with different levels of trust. Assign processes to rings based on their level of trust. Ring crossing is performed through gates that enforce protected entry points: a process calling a higher-level process or accessing data at a higher level can only call this process or access data at predesigned entry points with controlled parameters. Additional checks for security or reliability can be applied at the entry points.

## **Structure**
Figure 141 shows a class diagram for this pattern. The CallingProcess requests services from a CalledProcess. To do so, it must enter a CallGate, which applies protected entry points that check the correct use of signatures. CallRules define the requirements for inter-level calls. The CallingProcess can access Data according to a set of DataAccessRules.

![](./Images/protection_rings_structure.png)

*Figure 141: Class diagram for the PROTECTION RINGS pattern*

## **Dynamics**
Figure 142 shows a sequence diagram for a call to a higher-privilege ring. If the call fails an exception may be raised.

![](./Images/protection_rings_dynamics.png)

*Figure 142: Sequence diagram for a successful call to a higher-privilege ring*

## **Implementation**
The call rules and the data access rules are usually implemented in the call instruction microcode [1]. Figure 143 shows a typical use of rings. Processes are assigned to rings based on their level of trust; for example, we could assign four rings in decreasing order of privilege and trust, to supervisor, utilities, trusted user programs, untrusted user programs.

![](./Images/protection_rings_implementation.png)

*Figure 143: Assignment of protection rings*

The program status word of the process indicates its current ring, and data descriptors also indicate their assigned rings. The values of the calling and called processes are compared to apply the transfer rules. 

The Intel X86 architecture [1] applies two rules:

- Calls are allowed only in a more privileged direction, with possible restriction of a minimum calling level. 
- Data at level p can be accessed only by a program executing at a more privileged level (<= p).

Another possibility for improving security is to allow calls only within a range of rings: in other words, jumping many rings is considered suspicious. Multics defined a call bracket, where calls are allowed only within rings in the bracket. More precisely, for a call from procedure i to a procedure with bracket (n1, n2, n3) the following rules apply:

- If n2<i<=n3, the call is allowed to specific entry points. 
- If < n3, the call is not allowed.
- If < n1, any entry point is valid.

This extension only makes sense for systems that have many rings.

## **Example Resolved**
Now we can preassign processes to levels according to their trust. All calls to processes of higher privilege are checked. Processes of low trust get more checks.

## **Consequences**
The PROTECTION RINGS pattern offers the following benefits: 

- We can separate processes according to their level of trust. 
- Level transfers happen only through gates where we can apply the PROTECTED ENTRY POINTS pattern; that is, we have enforced protected entry points for upward calls. 
- We can control procedure calls as well as data access across levels. 

The pattern also has some potential liabilities: 

- Crossing rings take time. Because of this delay, some operating systems use fewer rings. For example, Windows uses two rings, IBM’s OS/2 uses three rings [2]. Using fewer rings improves performance at the expense of security. 
- Without hardware support the crossing ring overhead is unacceptable, which means that this approach is only practical for operating systems and for centralized environments.

## **Variants**
- Rings don’t need to be strictly hierarchical; partial orders are possible and convenient for some applications. For example, a system that includes a secure database could assign a level to the database equal to but separated from system utilities; the highest level is for the kernel, and the lowest level is for user programs. This was done in a design involving an IBM 370 [3]. 
- In some systems, such as the MV8000, rings are associated with memory locations. 
- Multics used the concept of the call bracket, where a call can be made within a range of rings.

## **Known Uses**
- Multics introduced this concept and used 32 rings, as well as call brackets [4]. 
- The Intel Series X86 and Pentium [1]. 
- MV8000 [5,6]. 
- Hitachi HITAC. 
- ICL 2900, VAX 11 and MARA, described in [7], which also describes Multics and the Intel series. 
- [8] shows a use of rings to protect against malicious mobile code. 
- An IBM S/370 was modified to have non-hierarchical rings [3]. 
- Rings have been used for fault-tolerant applications [9].

## **See Also**
- A combination (process, domain) corresponds to a row of the Access Matrix pattern. 
- MULTILEVEL SECURE PARTITIONS pattern is an alternative for distributed environments, in which processes are assigned levels based on MULTILEVEL SECURITY models. 
- PROTECTED ENTRY POINTS.

## **References**

[1] Intel Corporation. (n.d.). Intel Architecture Software Developer’s Manual. Volume 3: System Programming-

[2] Wikipedia contributors. (n.d.). Protection ring. Wikipedia. https://en.wikipedia.org/wiki/Protection\_ring#Supervisor\_mode

[3] Fernandez, E. B., Summers, R. C., Lang, T., & Coleman, C. D. (1978). Architectural support for system protection and database security. *IEEE Transactions on Computers*, *27*(08), 767-771.

[4] Graham, R. M. (1968). Protection in an information processing utility. *Communications of the ACM*, *11*(5), 365-369.

[5] (n.d.). MV8000 principles of operation. <http://cide17ca7e5bcaa1096.skydrive.live.com/self.aspx/P%c3%bablico/01400648_MV8000_PrincOps_Apr80.pdf> 

[6] Wallach, S. (1981). 32-bit Minicomputer Achieves Full 16-bit compatibility.

[7] Frosini, G., & Lazzerini, B. (1985). Ring-protection mechanisms: general properties and significant implementations. *IEE Proceedings E-Computers and Digital Techniques*, *132*(4), 203-210.

[8] Shinagawa, T., Kono, K., & Masuda, T. (2000). *Exploiting segmentation mechanism for protecting against malicious mobile code*. University of Tokyo. Department of Information Science.

[9] Ozaki, B. M., Fernandez, E. B., & Gudes, E. (1988). Software fault tolerance in architectures with hierarchical protection levels. *IEEE Micro*, *8*(4), 30-43.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
