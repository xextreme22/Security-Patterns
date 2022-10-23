# **Secure Boot (Authenticated Boot)**

## **Intent**
This pattern addresses how to ensure that violations of integrity properties of the software stack that is booted on a platform can be either prevented (SECURE BOOT) or detected (AUTHENTICATED BOOT).

AUTHENTICATED BOOT refers to an architecture that ensures that local or remote parties can verify properties of the software that has been booted. While SECURE BOOT interrupts the boot process if a modification of the loaded modules is detected, AUTHENTICATED BOOT continues the boot process even in that case. However, any modification is detected and recorded securely for later inspection or verification.

## **Example**
Consider a user who wants to use a computing device that was left unattended or that was used by another person before. How can the user be sure that the system software is in the intended operational state, i.e., that no critical component of the operating system or other software applications has been modified in a malicious or unauthorized way? Typically, a file integrity checker program can check the integrity of system and application files. However, any file integrity checker program must rely on trusted reference values and that those values have not been tampered with. Moreover, the user wants also to be sure that the file integrity checker itself is not tampered with or deactivated at all.

## **Context**
Users of security-sensitive applications want to be sure about the operational integrity of their applications and execution environment. Unauthorized changes to the application code or the operating system may lead to unintentional program behavior or violation of security goals. Users trust the hardware, but they need a way to verify that the software loaded on this hardware has not been tampered with.

## **Problem**
On conventional platforms, software can be manipulated or exchanged. Local users cannot verify if they are interacting with the “correct” software, and remote platforms cannot verify the software of their communication partner.

Before applications can be used on a computer system, the system has to be bootstrapped. Typically, the hardware starts by loading a piece of code (firmware, or BIOS on PCs), which in turn loads the bootloader from a pre-defined place from main storage (the disk drives). The bootloader loads the operating system kernel, and the operating system kernel loads system services, device drivers, and other applications.

At any stage of the bootstrap process, software components could have been exchanged or modified by another user or by malicious software that has been executed before.

The following forces have to be resolved: 

- You want to ensure the integrity of the loaded software on the system, otherwise malicious software could run without being noticed. 
- You want the computer system to always boot in a well-defined secure state. Otherwise, attackers could violate security goals by putting it into an insecure state. 
- You want to allow modifications of the operating system or application binaries. Otherwise, software installation and updates would be problematic.
  
## **Solution**
Based on the assumption that the hardware of the computer system is correct, the integrity of lower layer boot modules is checked, and control is transferred to the next stage if and only if the integrity of that stage is valid. Hence, every stage is responsible for checking the integrity of the next stage. Integrity checking can be performed in different ways: two common methods are comparing hash values or verifying digital signatures. In the first case, hash values of program binaries are computed using a cryptographic hash function (e.g., SHA-1) and compared to reference values. If the computed value does not match the reference value, the binary has been modified. In the second case, each program binary is cryptographically signed by its vendor with a signature key that is only known to the vendor. The signature of the binary can be verified to check that it has not been modified since its signature generation.

If one stage detects an integrity violation, execution is stopped and the system halts. The sequence of these integrity checks builds the chain of trust. Thus, the user knows implicitly that the system has booted valid program code if it is running at all.

If the first module of such a sequence of integrity checks has already been modified in an unauthorized or malicious way, then the user cannot trust on subsequent integrity checks. The modified module could cheat or even skip any integrity checking. Therefore, the very first boot module is the root of trust for the whole chain of integrity measurements and needs to be protected against unauthorized modifications. The integrity verification data (hash reference values, or signature verification keys) must be protected, too.

To protect the initial boot module (and its verification data) and to reliably build the chain of trust, the root of trust is realized in hardware. Hardware is assumed to be more secure than software because it cannot be changed or read out as easily as software. Moreover, hardware security modules can be protected against various physical attacks – at least to some extent.

## **Structure**
Figure 31 shows the elements of the SECURE BOOT pattern. The Root of Trust for Measurement is the first module in the bootstrap chain and realized and protected by hardware. A Bootstrap Module has a link to the next Bootstrap Module, which is “measured” for its integrity (typically, by computing a hash value) before control is transferred. Each Bootstrap Module has to maintain (and protect) its corresponding integrity verification data. The verification data of the Root of Trust module is also protected by the hardware, e.g., stored in protected memory registers.

![](./Images/secure_boot_structure.png)

*Figure 31: Elements of the SECURE BOOT pattern.*

Usually, the last Bootstrap Module is the operating system, which can load several different applications and not only one. In addition, applications can also start other applications or load libraries. In this case, applications also have to measure the integrity of the corresponding components.

## **Dynamics**
When the computer system is started, the Root of Trust for Measurement is executed. It loads and measures the program code of the subsequent Bootstrap Module and verifies the integrity of this code. If this fails, the Root of Trust stops the execution, and the system is halted. Otherwise, the Root of Trust transfers control to the subsequent Bootstrap Module.

The Bootstrap Module loads and measures the code of the next Bootstrap Module and verifies the integrity of this code based on the verification data. If this fails, the system is halted, otherwise control is transferred to the next Bootstrap Module. This continues until the last module in the chain of trust has received control (typically, applications).

## **Implementation**
AUTHENTICATED BOOT can be implemented using a hardware security module in the following way: The first module in the bootstrap chain is trusted by assumption (e.g., implemented in hardware), and measures the integrity of the next module. This integrity measurement is then stored in protected hardware registers. The measurements taken at load-time of the software can later be reported by the security module. For this, the module signs the stored values.

## **Example Resolved**
Based on SECURE BOOT, the user can be sure that the correct system has been booted on the computer without changes, because any modifications would have caused the boot process to abort. Moreover, it is not possible to boot a completely different system, because the boot chain starts with a root of trust protected by hardware.

## **Consequences**
The benefits from the SECURE BOOT pattern include: 

- The software integrity state is verified at boot time, and only software that passed this verification is booted. 
- With the variant AUTHENTICATED BOOT, it is possible to boot any software, however, the integrity state of the software at boot time can be checked later. 

The liabilities from the SECURE BOOT pattern include: 

- Integrity verification data must be installed and updated (e.g., due to revocation) in a secure fashion (preserving integrity, authenticity, and freshness of the data). 
- Software updates could be an issue, because after an update, the integrity state is changed (the software is modified). Hence, specific mechanisms for updates are needed to allow the system to boot properly afterwards. 
- The integrity of modules during runtime needs to be ensured by additional mechanisms. 
- The integrity verification of large modules (e.g., an entire OS) may be time consuming. Hence, the OS may require adaption to include integrity verification of its modules and applications. 
- This pattern adds complexity and overhead. It needs to be coordinated with the other protection mechanisms.
 
## **Known Uses**
There are various implementations of SECURE BOOT: 

- AEGIS [1] is a secure boot architecture for PCs, where a chain of trust is constructed at boot time by hashing components and comparing the result with digitally signed reference values. An expansion card implements the necessary hardware root of trust for measurement. 
- The Cell Broadband Engine processor [2] implements a secure boot mechanism to load application code into an isolated execution mode of one of its multiple processor cores. The Cell processor provides a hardware root of trust that verifies the integrity of the loaded code cryptographically. Only if the verification succeeds, the code is executed. Moreover, every time an application wants to use this isolated execution environment, it has to pass the secure boot process. Within the isolated mode, the application code is protected from software running on other cores and even from the operating system running on the supervisor core. The Cell Broadband Engine is used in IBM Cell blade servers and also in the Sony Playstation3 game console. 
- For mobile devices, requirements for secure boot have been collected in Open Mobile Terminal Platform (OMTP) recommendations [3] that consider different industrial standards and specifications, including specifications from the Open Mobile Alliance (OMA), the TCG, and the 3rd Generation Partnership Project (3GPP). The document describes the mechanisms for secure boot and authenticated boot in an abstract way, and hardware manufacturers implement them differently. 

There is a well-known example for AUTHENTICATED BOOT:

- The TCG specified authenticated boot based on the TPM [4]. The TPM contains special-purpose registers called Platform Configuration Registers (PCRs) that can be used to store hash values. These PCRs cannot be overwritten but only “extended” by computing hash values. Starting from the root of trust for measurement, all software modules in the boot chain of a TCG-enabled PC (BIOS, boot loader, operating system kernel, etc.) are first hashed, a PCR is extended accordingly, and then control is transferred. By this, the entire bootstrap chain is measured and recorded in PCRs. The TPM also offers a function TPM\_Quote, where the recorded hash values are digitally signed and reported to the caller.

## **See Also**
Boot Loader [5] describes the boot process as a sequence of single bootstrap stages. Each stage loads the image of the next one and transfers control after validation of the image according to certain requirements, typically verifying error correction checksums. In contrast, SECURE BOOT requires a secure verification of the integrity of the image in each stage, e.g., based on cryptographic hash functions. In addition, SECURE BOOT defines a root of trust for measurement and requires its protection by trusted hardware to establish a chain of trust throughout all stages.

AUTHENTICATOR verifies the identity of a subject and creates a proof of identity for later use, e.g., in access control decisions. However, it is typically intended to authenticate users and it does not include a protection for the AUTHENTICATOR itself. Hence, it cannot provide a chain of trust. But AUTHENTICATOR can be combined with SECURE BOOT to extend the proof of identity to the underlying system components.

In [6], the authors introduce patterns for TPM usage. Their patterns concern the communication of software with the TPM, whereas the SECURE BOOT pattern describes more abstract concepts that can be realized based on TPMs, or based on alternatives, such as secure co-processors.

STATIC INTEGRITY PROPERTIES pattern is a generalization of this pattern.

## **References**
[1] Arbaugh, W. A., Farber, D. J., & Smith, J. M. (1997, May). A secure and reliable bootstrap architecture. In *Proceedings. 1997 IEEE Symposium on Security and Privacy (Cat. No. 97CB36097)* (pp. 65-71). IEEE.

[2] Shimizu, K. (2006). The cell broadband engine processor security architecture. *IBM DeveloperWorks*. http://www.ibm.com/developerworks/power/library/ pa-cellsecurity/

[3] Open Mobile Terminal Platform Consortium. (2009). OMTP advanced trusted environment: OMTP TR1 (v1.1). OMTP. http://www.omtp.org/Publications.aspx

[4] Trusted Computing Group. (2007). TCG TPM specification version 1.2, revision 103. https://www.trustedcomputinggroup.org/specs/TPM

[5] Schutz, D., (2006). *Boot loader.* Proceedings of the 11th European Conference on Pattern of Languages of Programs.

[6] Gurgens, S., Rudolph, C., Mana, A., & Munoz, A. (2007, September). Facilitating the use of TPM technologies through S&D patterns. In *18th International Workshop on Database and Expert Systems Applications (DEXA 2007)* (pp. 765-769). IEEE.

## **Sources**
Löhr, H., Sadeghi, A. R., & Winandy, M. (2010, February). Patterns for secure boot and secure storage in computer systems. In 2010 International Conference on Availability, Reliability and Security (pp. 569-573). IEEE.
