# **Actor-Based Access Rights**

## **Intent**
This pattern allows us to depict and classify all business actors that act on an information system. By classifying we get a better hold on the completeness of the set of actors and therefore we get a better hold on the access rights granted to these actors. As a result, we get a better hold on authorization.

## **Example**
In the case of an Internet banking environment (see Known Uses) the information system X consisted of four subsystems. The grouping of actors proved very useful in determining the set of actors and the accompanying access rights.

Within the group of consumers, the following actors were determined: 

- Visitor, an unknown person who just visits the site, retrieving public information. 
- Prospect, a known person who took an application form by filling in some general information, but whose application form is not yet accepted. 
- Customer, a known person who already became a customer by filling out an application form that was accepted by the banking organization.

Within the group of employees three groups were determined: 

- Regular business actors, a group that was further decomposed later in the project and consisted of 15 to 20 different business actors. 
- Governance organization actors, a group that was further decomposed later in the project and consisted of approximately 10 different actors, including actors from outsourcing companies. 
- Security organization actors, another group that was further decomposed; this group included security managers from outsourcing companies.

Within the group of the business partners the most important actors were the ones from the line of control and the ones to deliver reports to. 

Within the group of business partners, the most important actors were credit check organizations and credit card organizations.

![](./Images/actor-based_access_rights_example.png)

*Figure 70: Example of Actor Groups.*

## **Context**
This pattern can be applicable in heterogeneous environments where multiple types of users need access to systems or information. Typically, this could be a business application with multiple channels to their customers, consumers, business partners or others.

## **Problem**
In everyday life we do not have a complete overview of the set of actors that participate in an information system. As a result, it is difficult or impossible to get hold of the access rights granted. A classification of all actors can help us to improve the completeness of the set and to improve the control on their access rights.

Heavily stimulated by the almost endless possibilities of the Internet, the exchange of data between organizations and between organizations and their customers increased enormously in recent years. All kinds of new systems were introduced in the so-called “e-business”. At the other side of the coin a lot of new people got access to these new systems and, as a result of this, got access to information.

In the rush for new business and new customers organizations tend to forget about restricting the access rights. As a matter of fact, they often do not even know who have access to their systems! Besides that, the set of new business and new customers is very volatile, since new systems come and go within months.

In the meantime, the demand for more powerful systems with a high granularity in functions of the systems increases. So, authorization for different customers has to be granular as well. A lot of organizations are currently shifting to a “need to protect” security strategy.

So, there’s the problem. How do we control granular access rights when we do not have an overview of all people who, all with their own characteristics, have access to our systems?

The following forces apply:

- Organizations left the fortress mentality and open up for actors like customers or business partners. These actors demand and get access to information related to them. Organizations delegate responsibilities to the actors but remain responsible for granting the access rights.
- Granting the wrong access rights to the wrong actors might lead to unauthorized access with consequences in fraud, privacy violation or image damage.
- The population of actors is highly dynamic, due to new business partners, acquisitions and mergers, new customers and in or outsourcing partners.
- The changes in access rights are highly dynamic as well, due to continuously changing functionality of information systems due to changing business requirements.
- Standard available role-based access in information systems increases. However, these roles do not always match the roles within the organization. In order to control access rights, organizations have to be able to match the standard available roles.

## **Solution**
In order to control a highly dynamic environment we must look at the elements that remain stable. Stable elements that allow organizations to provide products or services. 

1. Business processes that have to be performed to provide products or services 
1. Information accompanying these processes

Anyone or anything that acts on the information within a business process is a business actor. Access rights for actors only depend on the actions they perform on information; they do not depend on things like the location of these actors. To be clear: the security controls that have to be in place to enforce these access rights of course do depend on location, or specific channel, etc.

To some extend the types of actors in an organization are independent of the business processes or information involved. We distinguish the following actors.

### **Group 1. Employees.**
In every business process employees of the organization will be actor. But not all employees will be actor. Therefore, we need higher granularity within this group. A first cut.

- Regular Business Employees. These actors can perform their actions in front or back office. In most situations we will divide this group even further. 
- Governance actors. Every system has to be governed and these actors do influence the way a business process operates. In most situations we will divide this group even further. 
- Security actors. Within each organization these actors influence the way a business process operates by granting access rights, etc. In most situations we will divide this group even further.

### **Group 2. Consumers.**
This group is optional, since not all organizations will allow their consumers to act direct on the business processes and information. For instance, if consumers do act via (snail) mail or call center, they act indirect not direct on information.

Usually, we need higher granularity within this group. A first cut.

- Visitor. Typically, this would be a visitor to a web site who browses through the site and leaves without ordering anything or without requesting information.
- Prospect. Typically, this would be someone who request information and therefore has to leave some personal information.
- Consumer. Typically, this would be someone who orders products or services once.
- Customers. Typically, this would be someone with some order history.

### **Group 3. Business partners.**
This group is optional, although that’s not likely. Most organizations do have automated interaction with business partners. Other examples might be regulators and governmental agencies. The trend is that organizations will interact with their “volatile eco-systems” more intense than in the past. A typical first cut is based on actors in front office, mid office, or back office.

- Front office, e.g., content providers, information and brokerage portals 
- Mid office. Typically, these business partners provide services like credit check, criminal record check, etc. 
- Back office. Typically, these business partners provide financial services and transactions.

### **Group 4. Organizational lines of control.**
This group is optional. Depending on the scope of the set of business processes, these organizational lines of control could be within the organization or to a mother or daughter organization. A typical first cut.

- Controlling actors. 
- Reporting actors.

![](./Images/actor-based_access_rights_solution.png)

*Figure 71: Groups of Actors*

## **Structure**

## **Dynamics**

## **Implementation**

## **Example Resolved**

## **Consequences**
Consequences in implementing the pattern might be the following.

Some benefits: 

- A proven decomposition of actors can save time in determining the right access rights for the right actors. 
- The grouping of actors will provide overview in an environment with a lot of very different actors, such as in web-enabled systems. 
- The structured decomposition of the complete set of actors guarantees that easy to forget actors, like security administrators, will not be overlooked. 
- A quick actor overview helps to scope projects and identify window policies, contracts, and service level agreements (SLA’s) 
- The need for security administration procedures / tools can be quickly identified. 
- The pattern can be used as reference for security measures or to scope risk analysis studies 
- The pattern is easy to visualize and can be easily used to support further analysis and design 

Some pitfalls: 

- The decomposition of the actors in groups can be an enormous job in a large environment. Remember that having a complete decomposition is not the goal in itself, but it is merely a means to control access rights. 
- It is tempting to put all actors together and to call them just “users”. This might save time and a lot of discussion with businesspeople or business units, but it will not lead to a sound role-based access scheme, where the access rights can be explained in business terminology. 
- Not all classes are applicable in all situations. 
- Especially the class of business partners cannot be enumerated; there is an endless number of these business partners. A sub classification can be applied.

## **Known Uses**
The authors used this pattern to establish access rights in an Internet bank. Another use of this pattern is for an extranet security architecture for a high-tech company.

## **See Also**
In general, all patterns regarding role-based access are related, especially: 

- The Actor and role life-cycle pattern. 
- This pattern provides input for the Role pattern [1]. 
- Determining the variety of users helps to assign rights to users according to their roles in an institution [2]. 
- This pattern helps to identify user responsibilities within the organization in order to establish Role Based Access Control.

## **References**

[1] Yoder, J., & Barcalow, J. (1997, September). Architectural patterns for enabling application security. In *Proceedings of the 4th Conference on Patterns Language of Programming (PLoP’97)* (Vol. 2, p. 30).

[2] Fernandez, E. B., & Pan, R. (2001, September). A pattern language for security models. In *In Proc. of PLoP* (Vol. 1).

## **Source**
Hofman, A., & Elsinga, B. (2002). Control the Actor-Based Access Rights. In *EuroPLoP* (pp. 233-244).
