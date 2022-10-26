# **Circle of Trust**

## **Intent**
The CIRCLE OF TRUST pattern allows the formation of trust relationships among service providers to allow their subjects to access an integrated and more secure environment.

## **Example**
Our university had different ways to access different services. For each service, one needed a different protocol and to remember a different password. This was cumbersome and prone to error; it was also insecure because people would write their multiple passwords on their office bulletin boards.

## **Context**
Service providers that provide services to consumers (subjects) over large systems such as the Internet.

## **Problem**
In such large open environments, subjects are typically registered (have accounts) with unrelated services. Subjects may have no relationships with many other services available in the open environment. It may be cumbersome for the subject to deal with multiple accounts, and it may not be secure to build new relationships with other services since identity theft or violation of privacy can be performed by rogue services. How can we take advantage of relationships between service providers to avoid the inconvenience of multiple logins and to select trusted services? 

The solution to this problem must resolve the following forces:

- Service providers are numerous on public networks. It can be cumbersome, indeed impossible, for each service provider to define relations with every other provider. 
- The service providers’ infrastructures for their subjects’ login may be implemented using different technologies.

## **Solution**
Each service provider establishes business relationships with a set of other service providers. These relationships are materialized by the existence of operational and possibly business agreements between services. Such relationships are trust relationships since each service provider expects the other to behave according to the operating agreements. Therefore, a circle of trust is a set of service providers that have business and operational relationships with each other – that is, that trust each other. Operational agreements could include information about whether they can exchange information about their subjects, for example, and how and what type of information can be exchanged. Business agreements could describe how to offer cross-promotions to their subjects. Service providers need to agree on operational processes before their users can benefit from the seamless environment.

## **Structure**
Figure 114 shows a UML class diagram with an OCL constraint describing the structure of the solution. A CircleOfTrust includes several ServiceProviders, who trust each other, as described by the OCL expression.

![](./Images/circle_of_trust_structure.png)

*Figure 114: Class diagram for the CIRCLE OF TRUST pattern*

## **Dynamics**

## **Implementation**
Operational agreements should include a means to concretely enable trust, such as the sharing of a secret key or the secure distribution of certificates. The providers have to exchange credentials through some kind of external channel to trust and recognize each other.

## **Example Resolved**
Our university can now trust the identity providers who are members of our federation, so we do not have to worry that they will misuse our information.

## **Consequences**
The CIRCLE OF TRUST pattern offers the following benefits: 

- The subjects can interact more securely with (potentially unknown) trusted services from the circle of trust. 
- The services can provide a seamless environment to the subjects and can exchange information about their users by using operating agreements that describe what common technologies to use. 

Liabilities include keeping the participants synchronized in the presence of changes.

## **Known Uses**
- Identity federation systems and models such as the LIBERTY ALLIANCE IDENTITY FEDERATION standard. 
- PayPal has a variety of partners that trust their payment system, including Verifone and Equinox [1].

## **See Also**
- The IDENTITY PROVIDER pattern and IDENTITY FEDERATION pattern. 
- The LIBERTY ALLIANCE IDENTITY FEDERATION pattern [2]. 
- [3] formally discusses the CIRCLE OF TRUST. 
- The CREDENTIAL pattern.

## **References**

[1] Paypal partners with Verifone and Equinox to accept mobile payments in store. (n.d.). VentureBeat. from <http://venturebeat.com/2012/05/24/paypal-partners-with-verifoneequinox-to-accept-mobilepayments-in-store/> 

[2] Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.

[3] Nizamani, H. A., & Tuosto, E. (2011). Patterns of Federated Identity Management Systems as Architectural Reconfigurations. *Electronic Communications of the EASST*, *31*.

## **Source**
Fernandez-Buglioni, E. (2013). *Security patterns in practice: designing secure architectures using software patterns*. John Wiley & Sons.
