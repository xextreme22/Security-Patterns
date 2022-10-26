# **Secure GUI System**

## **Intent**
Provide a trusted path between the user and the application the user intends to use. Security-critical applications require integrity and confidentiality of user input and displayed output. All input and output devices (keyboard/mouse, display) have to be shared by all applications, but security-critical applications need exclusive access to avoid interference with other applications.

## **Example**
Consider a system login dialog on a graphical window system where the user has to enter a password. The user should be sure that the dialog being displayed actually corresponds to the system login and not to a faked application. Additionally, the data transmitted between the login dialog and the user (keyboard input and displayed graphical output) should not be possible to be eavesdropped.

## **Context**
You need to provide a graphical user interface for several applications with different trust and security requirements for one or more users. You need to ensure that the application the user in front of the input/output devices is currently interacting with cannot fake to be an arbitrary other (possibly trusted) application. You also need to ensure that the input of the user and the output of an application cannot be eavesdropped or manipulated by other applications.

## **Problem**
Providing security for user interfaces is non-trivial because they often rely on and communicate with several components. The problem is to combine the individual goals (security and usability) in such a way that efficient interaction with the user interface remains possible.

Experience shows that commodity operating systems cannot provide sufficient security for the GUI of applications (e.g. [1]). Applications can impose other applications by adapting their look and feel. An attack on the user’s privacy can be performed by applications that eavesdrop everything the user enters, e.g., keyloggers typically install themselves as device drivers to intercept all keyboard events.

In particular, the solution to this problem has to resolve the following forces:

- You need to provide a graphical user interface system that can be used by several applications simultaneously. 
- You need the flexibility that applications can draw any content without constraints (application flexibility). 
- Users need to know with which application they are currently interacting with (application authentication). 
- Users need to be able to invoke interaction with a certain trusted service at any time (trusted path). 
- You need protected input/output of GUIs, i.e., unauthorized applications should not be able to eavesdrop or manipulate user input directed to or display output coming from other applications. 
- You (optionally) need controlled communication between user interfaces of different applications – for example, in a multi-level security system copy&paste has to adhere to the information flow policy.

## **Solution**
The central idea is to mediate all user input/output through a Secure User Interface (SUI) system, and to separate the content drawn by applications from what is actually displayed on the screen. The SUI controls solely the graphics rendering hardware and the input events from the user input devices (typically, keyboard and mouse). The SUI receives the user input and sends it to the currently active application. Only one application can be active at a certain time, i.e., it has the “input focus”. On the other hand, the SUI receives the graphical output of all applications and multiplexes it to the graphics hardware, composed with a visible labeling of applications according to a policy. Hence, only the SUI decides what is going to be displayed on the screen and how.

Besides the input routing and the visible labeling, the SUI has to detect trusted path invocation, i.e., when the user requests to interact with a trusted service, and implement certain trusted path features, e.g., when the user wants to switch the input focus to another application.

## **Structure**
The SUI system interacts with Applications on the one hand, and Users via Graphics Hardware (screen, graphics processing unit (GPU)) and Input Devices (typically, keyboard and mouse) on the other hand. Applications are defined as interactive programs that want to display a GUI on the screen and receive user input. Depending on the operating system, Applications can be single processes, groups of processes (e.g., belonging to the same security level in an MLS system), or entire virtual machines.

The main elements of the SUI system are the Input Manager, Display Manager, Copy&Paste Manager, a Policy, and the SUI Control (see Figure 88).

![](./Images/secure_gui_system_structure.png)

*Figure 88: Participating elements of the SECURE GUI SYSTEM pattern.*

The Input Manager is responsible for the input routing and detection of trusted path invocation. It monitors all input events and forwards them to the Application which currently has the focus. Certain input events are not forwarded, they are used as Secure Attention Key, e.g., a special key sequence, to invoke trusted path features of the SUI Control.

The Display Manager multiplexes the GUI output and is responsible for drawing visible labeling. Visible labels allow the User to reliably identify Applications, especially the one having the input focus. It is important to note that the visible labeling must be drawn in a way that arbitrary Applications cannot fake it. For example, the Display Manager could only display the content of one application at a time on the screen and keep a reserved area, e.g., on the top of the screen, in which no application can draw. This area is used to display the identity of the current application. The screen could also be divided into fixed regions belonging to different groups of applications, e.g., of the same security level. To allow most flexibility and to display all application windows simultaneously, the Display Manager could apply a window labeling policy [2], i.e., all windows are decorated with colored border where each color corresponds to a security level. In addition to colors, the name of the application (or its security level) can be shown in the window border, and a reserved area on the screen shows the current input focus.

The SUI Control implements trusted path features, in particular, to switch the input focus to another Application.

GraphicalContexts represent the GUI windows or screens of Applications. They are composed of two parts: a Label, which represents the identity of the Application (e.g., its name or security level), and the actual graphical Content. The Content is the only region to which the Application can draw its GUI elements. The Label is under control of the SUI. This prevents that an application can fake the visible identity of another Application since it has no access to the Label, and the Label is finally visually “attached” on the screen by the Display Manager.

The Copy&Paste Manager acts as trusted intermediate component to enable Applications to exchange data in compliance to the information flow policy of the system.

Finally, the Policy of the SUI defines the labeling strategy of the Display Manager, constraints to allow to switch the input focus, constraints to allow copy&paste, and the secure attention key events and corresponding reactions.

Note that the Display Manager and the Input Manager are the only components connected to the hardware input/output devices. This has to be enforced by the underlying operating system (OS), e.g., by allowing access to the corresponding device drivers exclusively to these components. Moreover, the SUI system relies on proper authentication of Applications and Users, which has to be done by the OS as well.

## **Dynamics**
When an application wants to update its GUI, it draws into its Content object and requests to update the corresponding GraphicalContext. Figure 89 shows the corresponding sequence diagram. The important step in this process is the composition of the labeling information with the basic display content provided by the Application. The Application cannot alter the Label itself, as it is defined by the Policy based on the application identity and composed within the GraphicalContext by the Display Manager.

![](./Images/secure_gui_system_dynamics_1.png)

*Figure 89: Sequence diagram for updating GraphicalContext*

Users can request to switch the input focus by invoking the trusted path to the SUI Control (e.g., via a hot key).

Applications may also request to switch the focus to themselves or another Application, e.g., to request a password input or a confirmation from the user when they do not have the focus. In this case, the SUI Control asks the Policy and determines whether to allow or deny the request (see Figure 90). Alternatively, Policy can contain rules that effectively result in asking the user for a password in order to authorize the switch. Once authorized, SUI Control changes the input focus and updates the reserved area if required.

![](./Images/secure_gui_system_dynamics_2.png)

*Figure 90: Sequence diagram for switching application*

To enable copy&paste, the Policy must define access rules to the Copy&Paste Manager (CPM) which are complying with the information flow policy of the operating system. For example, in an MLS system it is not allowed to write data from high to low security levels. Therefore, all data transfer via GUI elements must go through the CPM. An application can copy (write) data to the CPM, and other Applications may request to paste (read) data from the CPM. The CPM has to enforce the policy on such requests accordingly.

## **Implementation**

## **Example Resolved**
By applying the SECURE GUI SYSTEM pattern, a Policy can be defined to keep a reserved area on the screen to display the active application’s name. The Display Manager controls solely the graphics hardware and multiplexes the output to the display. Hence, no application can modify the reserved area in order to change the displayed name. This allows the user to identify the login application and to distinguish it from faked ones. Moreover, since the Input Manager routes the user input only to the one application that has the focus, the input cannot be eavesdropped by any other application.

## **Consequences**
Applying this pattern is expected to result in the following benefits: 

- You will get a trusted path for the user input and the application output. This channel is secure against eavesdropping and manipulation by unauthorized applications.
- Security-critical applications need not to implement their own protection mechanisms against faked dialogs because they can rely on the SUI to show the user the identity of applications via labels. 
- The resulting SUI system is very flexible since labeling and access decisions can be delegated to the policy. 

However, this pattern may also have the following liabilities as consequences: 

- The SUI might become a single point of failure. If the SUI does not work correctly, the whole system might become unusable for users, or the security of user intentions is compromised. 
- There might be usability issues due to security constraints of the GUI system. For example, when applying a window labeling policy (based on colors), users might need extra training and education to understand the meaning of the colors. 
- You need to trust the SUI. For a high assurance system, the components within the SUI (cf. Figure 88) are critical and require high assurance for their development and INTEGRITY PROTECTION during runtime. 
- The increasing usage of graphics accelerator functions in commodity operating systems and 3D game applications may pose an implementation problem since any direct access to the GPU could circumvent the SUI.

Today’s GPUs lack of proper virtualization functionality. However, there are approaches to virtualize the 3D OpenGL graphics language [3], [4].

The actual implementation of the labeling in the SUI can be fitted to many scenarios. For example, one can implement different labeling methods for color blind people, and another can use a special braille external interface with separated “secure label zone” for blind people. It is also possible to decouple the label from the screen at all. For example, an additional small display attached to the keyboard could display the security level of the application that has currently the input focus.

Another area of extension is to control audio input/output in the same way as it is done with keyboard inputs. However, details are beyond the scope of this pattern.

## **Known Uses**
The SECURE GUI SYSTEM pattern can be found in several existing implementations: 

- Trusted X [5], [6] is a multi-level secure X window system. It encapsulates the untrusted functionality of an ordinary X server and polyinstantiates it for each security level. Trusted X multiplexes the windows of all levels and attaches a colored label on each window to indicate its security level. A reserved area on the screen always shows the level having the input focus. A dedicated trusted application implements the secure copy&paste function and mediates the data transfer according to the information flow policy.
- Solaris TX [7] supports multi-level desktop sessions in a similar way to Trusted X but uses a single trusted desktop system instead of polyinstantiated X servers. 
- EWS [8] is a MANDATORY ACCESS CONTROL capable window system for the EROS operating system. It consists of a small display server that renders the output of client applications and supports window labeling to indicate the trusted path. It does not have an X server, instead all drawing logic is in the applications, and shared memory is used to define Content. Secure copy&paste is realized via special invisible windows which are dedicated trusted applications. 
- Nitpicker [9] is a small, framebuffer-based secure GUI server on top of the L4 microkernel. It controls the physical display and aggregates the virtual screens of clients, which can be native processes or virtual machines. The server also supports visible labels. 
- Green Hills’ INTEGRITY [10] is a microkernel-based operating systems that supports multi-level security. It displays the maximum-security level and the current input security level at the top and, respectively, bottom of the screen as a colored bar. Applications in the system are virtual machines (VMs) that run isolated from each other. The VMs can only access virtual devices and cannot draw on the reserved screen areas. 
- The SDH architecture [11] divides the screen in separate regions according to security levels. For each region, an untrusted processor draws the Content, and a hardware implemented Display Manager multiplexes the output on the screen. A software-implemented Input Manager functions also as SUI Control, i.e., it switches current security level and input focus simply by moving the mouse pointer from one region on the screen to another.

While the examples above are mainly MANDATORY ACCESS CONTROL systems or research prototypes, commodity operating systems would also benefit from applying the SECURE GUI SYSTEM pattern. However, this would require changing the access to graphics and input hardware, i.e., not allow every application to arbitrarily use these devices (e.g., preventing installation of “filter” drivers that can intercept keyboard input). Moreover, the GUI system of a commodity operating system needs to separate the graphical content that applications can draw from what is finally presented on the screen (i.e., providing window labeling and/or a reserved area).

## **See Also**
On an architectural view, the SECURE GUI SYSTEM pattern is a SINGLE ACCESS POINT, for users to the graphical interfaces of the system’s applications, but also for applications to the user input/output devices, especially keyboard/mouse and the graphical processing unit.

REFERENCE MONITOR intermediates access to resources defined by policy. This is similar to the SECURE GUI SYSTEM pattern since the SUI system controls the usage of the graphics output and the user input, i.e., which application receives the input. But the SECURE GUI SYSTEM has additionally to ensure authenticity of the end points. Actually, the SECURE GUI SYSTEM can use the REFERENCE MONITOR pattern to implement a policy-driven secure GUI.

The AUTHENTICATOR pattern can be applied to provide proofs of identity of users and applications, which are requested by the SECURE GUI SYSTEM. For example, digital certificates of program binaries can be used to reliably identify applications, which the operating system has to verify when a process is started.

The SUI has to be executed in an EXECUTION DOMAIN which restricts the usage of the user I/O devices solely to the SUI. Other processes must not be allowed to directly access the GPU, otherwise they could easily circumvent the SUI.

To protect the input sent to an application and the output sent to the graphics hardware, it must be possible to isolate process memory. Otherwise, one process could directly read or modify the memory of another process and, hence, intercept or manipulate the data entered by or presented to the user. Process isolation can be achieved with the CONTROLLED VIRTUAL ADDRESS SPACE pattern.

## **References**

[1] Spalka, A., Cremers, A. B., & Langweg, H. (2001, June). The fairy tale of’what you see is what you sign’-trojan horse attacks on software for digital signatures. In *Proceedings of the IFIP WG* (Vol. 9, No. 11.7, pp. 75-86).

[2] Epstein, J. J. (1990, December). A prototype for Trusted X labeling policies. In *[1990] Proceedings of the Sixth Annual Computer Security Applications Conference* (pp. 221-230). IEEE.

[3] Lagar-Cavilla, H. A., Tolia, N., Satyanarayanan, M., & De Lara, E. (2007, June). VMM-independent graphics acceleration. In *Proceedings of the 3rd international conference on Virtual execution environments* (pp. 33-43).

[4] Smowton, C. (2009, March). Secure 3D graphics for virtual machines. In *Proceedings of the Second European Workshop on System Security* (pp. 36-43).

[5] Epstein, J., McHugh, J., Orman, H., Pascale, R., Marmor-Squires, A., Danner, B., ... & Rothnie, D. (1993). A High Assurance Window System Prototype 1. *Journal of Computer Security*, *2*(2-3), 159-190.

[6] Epstein, J. (2006, December). Fifteen years after tx: A look back at high assurance multi-level secure windowing. In *2006 22nd Annual Computer Security Applications Conference (ACSAC'06)* (pp. 301-320). IEEE.

[7] Faden, G. (2006). Solaris trusted extensions. *Sun Microsystems Whitepaper, April*.

[8] Shapiro, J. S., Vanderburgh, J., Northup, E., & Chizmadia, D. (2004). Design of the {EROS} Trusted Window System. In *13th USENIX Security Symposium (USENIX Security 04)*.

[9] Feske, N., & Helmuth, C. (2005, December). A Nitpicker's guide to a minimal-complexity secure GUI. In *21st Annual Computer Security Applications Conference (ACSAC'05)* (pp. 85-94). IEEE.

[10] Green Hills Software Inc. (2008, November). INTEGRITY PC Technology. <http://www.ghs.com/products/rtos/integritypc.html> 

[11] Sherman, R., Dinolt, G., & Hubbard, F. (1991, December). Multilevel secure workstation. U.S. Patent 5,075,884.

## **Source**
Fischer, T., Sadeghi, A. R., & Winandy, M. (2009, August). A pattern for secure graphical user interface systems. In *2009 20th International Workshop on Database and Expert Systems Application* (pp. 186-190). IEEE.
