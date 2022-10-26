# **Secure Storage**

## **Intent**
SECURE STORAGE provides confidentiality and integrity for stored data, and additionally enforces access restrictions on entities that want to access data. In particular, the SECURE STORAGE can verify the integrity of software components, and it grants access to the data only to authorized, unmodified components that meet these restrictions.

## **Example**
Consider the problem of storing passwords (e.g., for web services) securely on a computer. Users want to protect their passwords, such that attackers who might install malicious software on the computer (e.g., phishers using malware) cannot access them. In this case, simply encrypting the passwords with a passphrase does not help: The software installed by the attacker might look exactly like the legitimate software (i.e., the password manager they normally use) to users, so they enter their passwords. Thus, the attacker obtains the passphrase, and can decrypt and steal all passwords. We need to store passwords in such a way that only the legitimate password manager can access them.

## **Context**
You need to provide storage that protects the confidentiality and integrity of stored data, and which verifies the integrity of the software before granting access to the data. You trust the hardware, but you need to be able to verify that only authorized, unmodified software can access data stored in the SECURE STORAGE.

## **Problem**
Cryptographic techniques exist to protect the confidentiality and integrity of data, e.g., encryption and digital signatures. However, the secret keys need to be protected against unauthorized usage. Operating system access controls are not sufficient because the access control component could be manipulated or replaced by an attacker.

The following forces have to be resolved: 

- You need to protect the confidentiality and integrity of data, even when the system is not running. Otherwise, an attacker could read or modify protected data. 
- You need to protect secret cryptographic keys from unauthorized access and usage. Otherwise, an attacker could use the keys, e.g., to decrypt confidential data. 
- You want to allow modifications of the operating system or application binaries. Otherwise, software installation and updates would be problematic.

## **Solution**
A Root Key is used to encrypt and decrypt data and other keys (which in turn can protect data or other keys). The usage of the Root Key is controlled by a Root Key Control component. Root Key and Root Key Control are both protected by trusted hardware, i.e., the secret part of the Root Key never leaves the hardware, and Root Key Control cannot be manipulated or replaced by users or other software programs. Root Key Control verifies the integrity of Applications and their Execution Environment before it performs cryptographic operations on behalf of an application.

## **Structure**
Figure 87 shows the elements of the SECURE STORAGE pattern. Root Key Control has solely access to the Root Key, and both are protected by hardware. The Root Key can protect an arbitrary amount of Data. A special case of Data is Application Keys, which Applications can use to protect their application-specific data. The Root Key Control maintains a mapping of what Data can be accessed by which Application. It uses integrity verification data (hash reference values, or digital signatures) to verify the integrity of Applications and their Execution Environment.

![](./Images/secure_storage_structure.png)

*Figure 87: Elements of the SECURE STORAGE pattern.*

## **Dynamics**
When an application requests to access Data, the Root Key Control verifies first the integrity of the Application and of the software components of the Execution Environment the Application is executed in (typically, the operating system components). Only if the integrity verification succeeds, Root Key Control uses the Root Key to decrypt (or encrypt) the Data. Note that the Root Key is never passed to an application. Instead, the Root Key Control performs the corresponding encryption and decryption operations and only returns the result.

## **Implementation**

## **Example Resolved**
Passwords are stored in secure storage, ensuring that the integrity of the application and its trusted computing base (TCB) are verified before the application can access the passwords. Access is granted only to the authorized password manager, and only if it is running in a secure execution environment on a secure operating system. It is important to include the TCB (here: operating system) into the verification process, because on an insecure system, malware might be able to read the memory of the password manager and hence gain access to the passwords.

## **Consequences**
The benefits from the SECURE STORAGE pattern include: 

- Only software where the integrity verification succeeded can access the protected data. In combination with a secure operating system, this can prevent malware from accessing sensitive data. 
- Data can be stored on a system, such that it can be accessed only when the authorized operating system and software has been started. 

The liabilities from the SECURE STORAGE pattern include: 

- Backup strategies become more involved because the encryption keys are protected in hardware. Data must be encrypted with a different key for the backup system, or a mechanism is needed to backup hardware-protected keys to other secure hardware. 
- After an update, software cannot access protected data anymore, if no additional mechanisms are in place. Thus, software updates become more difficult. 
- This pattern adds complexity and overhead. It needs to be coordinated with the other protection mechanisms.

## **Known Uses**
There are various implementations of SECURE STORAGE: 

- The Cell processor [1] features storage that can only be accessed when the processor is in a “secure state”. In this state, software that is running on the processor has been measured by a SECURE BOOT mechanism. This way, the Cell processor can provide SECURE STORAGE. 
- TPM sealing [2] is a mechanism specified by the TCG and implemented in the TPM. With this functionality, a key which can be used only inside the TPM can be restricted in its use by defining specific values for the PCRs. Since the PCRs securely store the recorded measurements from the authenticated boot process, usage of the key can be restricted to software that passes the integrity verification. 
- The OMTP TR1 recommendation [3] includes detailed requirements for SECURE STORAGE on mobile devices.

## **See Also**
SECURE STORAGE requires SECURE BOOT pattern to protect the integrity verification data. If Applications and their Execution Environment are also part of the SECURE BOOT, then Root Key Control can rely on the integrity verification during SECURE BOOT and does not need to perform it explicitly.

## **References**

[1] Shimizu, K. (2006). The cell broadband engine processor security architecture. *IBM DeveloperWorks*.

[2] Trusted Computing Group. (2007). TCG TPM specification version 1.2, revision 103. https://www.trustedcomputinggroup.org/specs/TPM. 

[3] Open Mobile Terminal Platform Consortium. (2009). OMTP advanced trusted environment: OMTP TR1 (v1.1). recommendation document. <http://www.omtp.org/Publications.aspx>. 

## **Sources**
Löhr, H., Sadeghi, A. R., & Winandy, M. (2010, February). Patterns for secure boot and secure storage in computer systems. In 2010 International Conference on Availability, Reliability and Security (pp. 569-573). IEEE.
