# **Risk-Based Authenticator**

## **Intent**
The RISK-BASED AUTHENTICATOR (RBA) pattern uses information on a subject, so called Authentication Indicators, to verify their risk of not being authentic and, in case of a high risk, it takes further steps to raise the trust in the subject’s authenticity. A profile is generated for each subject by continuously and transparently gathering information on them during a session. A risk, that the subject is not authentic, is calculated for each request to the RBA by comparing the authentication indicators with the profile of the subject. If the risk exceeds a defined limit a step-up or reauthentication may be scheduled to reduce the risk or the access is denied.

## **Example**
We want to build an online shop for shoes that addresses consumers with a web application through the internet. Several other shops exist and, thus, usability is one of the key quality characteristics we want to address while still being secure. Users shall offer their personal data as simple as possible and authenticate with easy-to-use methods. We use a weak authentication based on social networks (social login) because it is easy to use. But, if a risky action is performed, e.g., if the user wants to initiate payment, we want to be sure they are authentic, and their account is not hijacked.

## **Context**
Web applications that offer services for consumers and that are accessible through the internet. The consumer needs to authenticate in order to use the services. Transactions of the web application need to be secured properly.

## **Problem**
In general, we are aware of vulnerabilities in our authentication, e.g., passwords can be stolen, fingerprints can be imitated, or the authentication protocol implementation may have a security flaw. Thus, hijacking user accounts is a problem in our web application. How can we prevent attackers from impersonating as a legitimate service consumer and, as a result, causing damage to the user and our enterprise? The following forces have to be tackled by the solution:

- Usability: Ease of use is a competitive advantage and users will switch to a competitor in case of bad usability. 
- Effort: The user should be authenticated with little effort. If the effort is too high, the user will not use the application. 
- Frequency: The user should interact as often as needed but not more. The frequency of user interaction to authenticate directly affects the usability and should be kept small. 
- Performance: The response time of requests in the web application as well as the web user interface’s response time should keep almost the same when using a transparent authentication. 
- Cost: The authentication should be cost-effective, e.g., the need for hardware raises costs and does not scale well. 
- Security level: The authentication should ensure a high security level, thus, the probability that the user is not authentic is small. 
- Security: The information used to authenticate the user must be securely stored to avoid unwanted access.

## **Solution**
You should continuously and transparently collect information on users and learn about their typical context. Based on the gathered information, calculate a risk for a concrete request context. If the risk is high, process a case handling for the request, e.g., schedule a step-up authentication, inform the account owner, or deny access. Perform a risk-based authentication for a request based on your requirements, e.g., if a valuable asset is accessed or on each request.

## **Structure**
In Figure 83 the structure of the pattern is depicted. The RbaAuthenticator realizes an Authenticator. Thus, it authenticates a User, and, in case of success, it creates a ProofOfIdentity. The RbaMediator that the user is not authentic, and for giving a recommendation on how to process their request. For this purpose, the RbaAuthenticator forwards each request to the RbaMediator.

![](./Images/risk-based_authenticator_structure.png)

*Figure 83:  Structure of the Risk-Based Authenticator based on the AUTHENTICATOR pattern [Schumacher et al. 2006]*

The RiskDataCollector collects AuthenticationIndicators and the subject’s history information from the http request and UserRepository. The RiskEngine takes the AuthenticationIndicators and calculates a risk for the current request. The CaseDecisionMaker decides how to handle the request based on the risk score. The RbaAuthenticator processes the request taking the recommendation in mind, e.g., it invokes an authenticator or destroys a previously created ProofOfIdentity. It may act against the decision of the CaseDecisionMaker under certain circumstances, e.g., if it shall reauthenticate the user, although this does not reduce the risk score.

## **Dynamics**
The flow of the RBA is separated into an abstract flow, that handles the authentication without knowing about risk, and the flow mediated by the RbaMediator. We separated the flows of the RBA into three use cases. The use case “Intercept a request” describes the abstract flow while the use cases “Register user” and “Risk-based authenticate user” describes the mediated flow.

The first use case “Intercept a request” focuses on handling further steps based on the risk score, Figure 84. The RbaAuthenticator processes the http request to the RbaMediator which returns a recommendation for handling the risk. Afterward, it handles the recommendation, authenticates the user, takes further steps with contacting other Authenticators, or rejects the request.

The other use cases describe how the RbaMediator handles the first request (use case “Register user”) and how a typical request (“Risk-based authenticate user”) of a user is processed using the RiskDataCollector, RiskEngine and CaseDecisionMaker, Figure 85.

![](./Images/risk-based_authenticator_dynamics_1.png)

*Figure 84: Sequence diagram for use case 1 “Intercept request and process according to risk score”*

![](./Images/risk-based_authenticator_dynamics_2.png)

*Figure 85: Sequence diagram for use case 2 “Register subject and learn basic context” and use case 3 “Process risk-based authentication request”*

## **Implementation**
There are three areas to focus on when implementing the RBA. First, the AuthenticationIndicators, that shall be used to calculate the risk score, have to be determined. Second, all information needed to authenticate the subject based on the AuthenticationIndicators as well as methods to gather this information have to be defined. Third, for each possible risk score the recommendation of the CaseDecisionMaker has to be specified.

The three areas are discussed using a simplified implementation of a campus web application. The application offers services for students. They can see an overview of their grades by authenticating with their matriculation number and a password. The grades our web application offers have a high demand for confidentiality.

### Authentication Indicators need to be chosen.

We have to choose authentication indicators (hereafter only “indicator”) according to the requirements of our web application. The demand for confidentiality is high, thus, we need to choose indicators that assure a high trust in the user’s authenticity. We decide to take a browser fingerprint, because studies show that these fingerprints identify users with an accuracy above 90% Eckersley [1]. Additionally, we take the geo location of the request into consideration and count the failed authentication requests. In our simplified scenario, a change of the fingerprint shall result in a high-risk score. Requests from foreign countries shall raise the risk score and reduce the count of allowed authentication attempts.

We took the design decision, that the risk score is a ratio scale ranging from 0 to 100 [percent]. According to the previous considerations, we define the following risk calculation, that our RiskEngine implements.

The RiskEngine calculates a sub risk score for each authentication indicator and sums these sub risk scores up, while the maximum is 100. - The sub risk score of the browser fingerprint indicator is 100, if the browser fingerprint changes, and otherwise 0. - The sub risk score of the authentication attempts indicator is 20 ∗ |failedAttempts|. - The sub risk score of the geo location indicator is 60, if the user tries to access from a foreign country, and otherwise 0.

### The RiskDataCollector must collect the relevant information to calculate the risk.

In our scenario, we have three indicators to collect - browser fingerprint, authentication attempts and geo location.

We use an open-source framework (<https://github.com/Valve/fingerprintjs2>) to calculate a browser fingerprint and add it to each request in the HTTP header. The fingerprint is recorded on the user’s first login, where they have to use an initial password, they received by encrypted mail. Implementing the authentication attempts indicator is quite simple. The value is initiated to 0 and reset on successful authentication. For each unsuccessful attempt, the value is increased by 1. The geo location is resolved using a free web service (http://ip-api.com/). The IP address of the user’s HTTP request is sent to this service. The answer of the service includes the country (geo location) of the IP address.

### The CaseDecisionManager has to be configured to handle the risk score.

Now, we have to define, which risk scores lead to which re- or step-up authentication method and which risk score is too high and results in the denial of the authentication. The risk score has to be harmonized; CaseDecisionMaker and RiskEngine must have the same understanding.

In our example, this is achieved by defining a risk score range. In addition, the risk calculation should be taken under consideration, when defining the risk score handling. As our application has a high demand for confidentiality, we use a strict strategy. Someone accessing our application from a foreign country shall have just one authentication attempt. Requests using an unknown browser shall be declined directly. But the user can request an initial password at will (send to them encrypted) and add further browsers.

Thus, the CaseDecisionManager is configured to reject requests with an overall risk score higher than 70 and no step-up or re-authentication is scheduled. One failed authentication from a foreign country (60+20∗1 > 70) and four from the country of our university (20 ∗4 > 70) are allowed. Changing the fingerprint always results in a denied request (100 > 70).

## **Example Resolved**

## **Consequences**
The RBA pattern offers the following benefits:

- Effort: Strong authentication mechanisms often go with a high user effort, e.g., when an extra device generating one-time passwords is needed and must be at hand. The RBA lowers this effort by enforcing a quite strong authentication based on authentication indicators. Additionally, if needed, strong authentication is performed only in case of high risk. 
- Frequency: The user’s authenticity is verified continuously, but transparent to the user. The user does not have to interact with the system. 
- Performance: The RBA can authenticate the user asynchronously. Thus, an authentication request can be sent to the RbaAuthenticator while the user can go on interacting with the system. This is helpful when the authentication takes a long time. 
- Cost: The RBA offers itself a quite strong authentication based on authentication indicators. Thus, it can be used to strengthen weak authentication, such as a password-based, without the need of new hardware. 
- Security level: By applying the RBA, The Password Anti-Pattern [2] (identity delegation by revealing username and password to a foreign service provider) as well as stolen passwords are detected and can be treated.
- Separation of Concerns: The RBA separates concerns, thus, data collection, risk calculation and handling of risk is divided into different components. 
- Extensibility: Existing Authenticators do not need adaptation. They are treated as resource for the RBA, and they are used for step-up or re-authentication.

The pattern has the following potential liabilities: 

- Performance: If the authentication is performed synchronously, the RBA can lead to performance issues. This is typically the case for the first request of a session. Additionally, some authentication indicators may need to collect a lot of data to statistically identify the subject. This may slow down the web application or raise requirements to the subject’s client. 
- Privacy: Sensitive information on the user is collected. This may be communicated to the user, e.g., depending on the law enforced to the web application. 
- Complexity: The RBA is an AUTHENTICATOR on top of existing authenticators; thus, additional components are introduced by the RBA making the application more complex. 
- Security: Securing the information used to authenticate the user, including history data, is not part of the RBA. The data store must be secured properly. 
- Security: The information used to authenticate the user must be securely stored to avoid unwanted access. Additionally, manipulating this data may lead to account hijacking. 
- Storage: The history data of subjects raises the need for a data store.

## **Known Uses**
The RBA is implemented in several IAM products, such as the following: 

- ForgeRock OpenAM [3], an open-source solution offering several authentication (single-sign on, adaptive authentication) and access control (web services security, entitlements) features. OpenAM offers a web user interface to configure the RBA. The administrator can define how the risk is calculated using given authentication indicators and they can define for different URLs, which risk score is acceptable and how high risk shall be handled. 
- CA Risk Analytics [4], a commercial product offering authentication based on risk calculation for web and mobile platforms. The system calculates a fingerprint for devices in order to identify subjects. The risk calculation is based on neural networks, learning throughout the subject’s session. The software focuses on e-commerce web applications. 
- SafeNet Context-Based Authentication [5], offers an RBA authentication based on configurable context information, e.g., Ip-address, daytime, and device identifiers. Their “Context Engine” calculates a context assurance level, which requires a specified authentication level. Each authentication mechanisms are associated with an authentication level. Depending on this information, a step-up authentication may be initiated. 
- PortalGuard [6], uses multiple factors to identify the subject. Here, the parameters for allowing access are strictly specified, e.g., access shall only be allowed during office hours. If a subject falls outside this parameters, access is denied. These parameters can be defined by the administrator.

Google, as mentioned in [7], Facebook, the video game developer Blizzard Entertainment and other companies implement the RBA, too. They reject suspicious requests, force to register new devices or schedule further authentication steps in order to strengthen trust.

## **See Also**
- The RBA is a specialization of the AUTHENTICATOR pattern using Authentication Indicators. 
- The pattern needs at least one concrete AUTHENTICATOR, such as Password, Biometric or X.509, that proofs the identity of a subject. Additionally, these Authenticators can be used by the RBA for step-up authentication. 
- When using a SECURITY SESSION pattern, the RBA can be used to verify the authenticity of the user. For example, a change of the user’s location in a limited extend is acceptable, whereas a complete change of the location indicates a high risk and may invalidate the session. 
- An Intercepting Web Agent [8] may be used to transparently authenticate requests using the RBA without affecting the web application. 
- Access Control taking Risk into consideration, e.g. [9; 10], is comparable to the RBA. But, in contrast to the RBA, these access control models (being quite similar to patterns) also consider the accessed resource for risk calculation. 
- The INFORMATION OBSCURITY pattern may be used to secure the stored information.

## **References**

[1] Eckersley, P. (2010, July). How unique is your web browser?. In *International Symposium on Privacy Enhancing Technologies Symposium* (pp. 1-18). Springer, Berlin, Heidelberg.

[2] Crumlish, C., & Malone, E. (2009). *Designing social interfaces: Principles, patterns, and practices for improving the user experience*. " O'Reilly Media, Inc.".

[3] ForgeRock Community. (2016). OpenAM Project. Retrieved November 14, 2016, from <http://openam.forgerock.org>

[4] CA Technologies. (2016). CA Risk Authentication. Retrieved November 14, 2016, from <http://www.ca.com/us/products/ca-risk-authentication.html> 

[5] SafeNet Inc. (2016). Context-Based & Step-Up Authentication Solutions. Retrieved November 14, 2016, from <http://www.safenet-inc.com/multi-factor-authentication/context-based-authentication> 

[6] PistolStar Inc. (2016). Contextual Authentication. Retrieved November 14, 2016, from <http://www.portalguard.com/contextual-authentication.html>

[7] Grosse, E., & Upadhyay, M. (2012). Authentication at scale. *IEEE Security & Privacy*, *11*(1), 15-22.

[8] Steel, C., & Nagappan, R. (2006). *Core Security Patterns: Best Practices and Strategies for J2EE", Web Services, and Identity Management*. Pearson Education India.

[9] Chen, L., & Crampton, J. (2012). Security and Trust Management: 7th International Workshop, STM 2011, Copenhagen, Denmark, June 27-28, 2011, Revised Selected Papers. Springer Berlin Heidelberg, Berlin, Heidelberg, Chapter Risk-Aware Role-Based Access Control, 140–156.

[10] Ni, Q., Bertino, E., & Lobo, J. (2010, April). Risk-based access control systems built on fuzzy inferences. In *Proceedings of the 5th ACM Symposium on Information, Computer and Communications Security* (pp. 250-260).

## **Source**
Steinegger, R. H., Deckers, D., Giessler, P., & Abeck, S. (2016, July). Risk-based authenticator for web applications. In *Proceedings of the 21st European Conference on Pattern Languages of Programs* (pp. 1-11).
