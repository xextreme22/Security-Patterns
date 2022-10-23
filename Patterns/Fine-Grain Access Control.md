# **Fine-Grain Access Control**

## **Intent**
A holistic access control to business objects is not sufficient as business objects are often comprised of multiple parts (fields) and may contain references to other business objects. Objects need to be designed with built-in support for fine-grained access control to their elements. Consequently, we need a model which provides a fine-grain access-controlled business objects. In this pattern we present an approach for access control to an object and to its individual attributes and operations.

## **Example**
Consider a simple customer business object, which consists of customer name, address, telephone, email, credit card info, and ordering history.

The customer data should only be available to individuals who need it and have the right (permission) to access it. Most of the time, only parts of the customer's data are needed by those who access it. For example, a marketing department might need to see the customer's order history and contact info, but they should not be able to see the customer's credit card info. On the other hand, the ordering department should have access to the customer's credit card info in order to be able to validate it but might not have a valid business reason to see the order history.

Access to the customer object and its fields could be programmed into the object itself. This approach however is not desirable as it does not consider constantly changing business conditions and processes. Any changes to the way customer object are accessed and controlled would require additional programming, which is often expensive and time consuming.

An alternative is to associate permissions with the customer object. Thus, for example, a Marketing Department permission can be associated with the customer business object, and its contact info and ordering history details. At the same time, an Ordering Department permission can be associated with the customer business object's credit card details. Both permissions are associated with the customer object, but each propagates to a different set of its fields, thus each permission provides a FINE-GRAIN ACCESS CONTROL over the customer object.

## **Context**
You are designing a new business object and need a way to provide FINE-GRAIN ACCESS CONTROL to the object and its fields when your business object is deployed in your business application. The FINE-GRAIN ACCESS CONTROL should be configurable and verifiable. Finally, the fine-grain access to the business object should also provide control over the business object's fields visibility and editability

## **Problem**
Determining business object's access control and fine-grain accessibility of its fields at design time or programmatically is not always feasible or possible. How can I design a business object's fine-grained access control which can be parameterized, and thus configurable at run time or at the time of deployment?

## **Solution**
The UML class diagram for the pattern's design is shown in Figure 26. The business object metamodel is suitable for different runtime environments (such as web client, web backend, web services, Java virtual machine). It includes the following concepts: 

- BusinessDataObject class: for business objects. Such an object is uniquely identified and all objects from the same class have similar states and behavior, as per the object-oriented paradigm. A class state is defined by its attributes (i.e., properties) and an object is capable to execute a set of operations. 
- Attribute class: an object of this class describes the name and type of a business object's attribute. The runtime value of an attribute is not addressed in this pattern. A BO may have zero or more Attribute objects, one for each of its attribute. 
- Operation class: each operation supported by a BO is described by an Operation object. Its attributes may include name, return value, parameter list.

Access rights and permissions are represented in this design by corresponding classes for BOs, their attributes, and operations. A BO instance has an optional DataAccessRightsDescriptor object that represents the rights assigned to the BO in an abstract and opaque way. An Attribute object is also associated with an optional DataAccessRightsDescriptor instance, describing the access rights endowed to this particular attribute. The OperationRightsDescriptor class describes the rights assigned for a particular operation for a BO, again in an abstract way.

The Subject class represents the entity that access a BO's attributes and executes its operations. A Subject's access permissions are described by a PermissionsDescriptor object.

The Runtime class abstracts the runtime system (e.g., web application server, JVM, browser JavaScript engine) and aggregates subjects and BO instances.

The PermissionAuthority object is responsible for authorizing access by Subjects to BOs, attributes, and operations depending on the access rights descriptors associated for a particular access attempt by a Subject. The PermissionAuthority checkDataAccess() and checkOperationAccess() methods implement the validation for access to a BO's attribute and operation, respectively, and encapsulate policies for dealing with defaults and conflicts.

![](./Images/fine-grain_access_control_solution.png)

*Figure 26:  The design of the Fine Grain Access Control for Business Objects pattern.*

## **Structure**


## **Dynamics**


## **Implementation**


## **Example Resolved**


## **Consequences**


## **Known Uses**
Most, if not all major Commercial Off The Shelf (COTS) Enterprise Resource Planning (ERP) developers such as Oracle and SAP to name the two biggest, employ some sort of permission-based access to their data forms and data objects. They too apply permission-based access at both the full object level, as well individual object fields. Ariba for example uses permission objects which can be assigned to role objects, which in turn are then assigned to user objects. Developers can then assign permissions to actions such as editability and visibility of the fields within a given object. This allows Ariba business objects to be accessed via permissions, without the need for further coding

## **See Also**
The DISCRETIONARY ACCESS CONTROL (DAC) pattern enforces access control based on user identities and the ownership of objects. The owner of an object may grant permission to another user to access the object, and the granted user may further delegate the permission to a third person.

The MANDATORY ACCESS CONTROL (MAC) pattern governs access based on the security level of subjects (e.g., users) and objects (e.g., data). Access to an object is granted only if the security levels of the subject and the object satisfy certain constraints. The MAC pattern is also known as MULTILEVEL SECURITY model and lattice-based access control.

The Role Based Access Control (RBAC) pattern enforces access control based on roles. A role is given a set of permissions, and the users assigned to the role acquires the permissions given to the role. Since the RBAC pattern is based on roles which are in general fewer than the number of users, it is useful for managing a large number of users.

Attribute-based Access Control (ABAC) defines an access control architecture whereby access rights are granted to users through the use of policies which combine attributes together. The policies can then use any type of attributes (user attributes, resource attribute, etc...). Attributes can be compared to static values or to one another thus enabling relation-based access control. [1] 

The Semantic Access control (SAC) model was created in 2002. The fundamentals of this semantics-based access control model are the definition of several metadata models at different layers of the Semantic Web. Each component of SAC represents the semantic model of a component of the access control system. The semantic properties contained in the different metadata models are used for the specification of access control criteria, dynamic policy allocation, parameter instantiation and policy validation processes. [2]

CONTENT-DEPENDENT AUTHORIZER restricts subjects (users, systems, parties) to access (read, write) only data with specific values. 

## **References**

[1] Yuan, E., & Tong, J. (2005, July). Attributed based access control (ABAC) for web services. In *IEEE International Conference on Web Services (ICWS'05)*. IEEE.

[2] A Semantics-based Access Control Model for Open and Distributed Environments. (n.d.). Semantic Access Control. http://www.lcc.uma.es/%7Eyague/Semantics-basedAccessControl.html

## **Sources**
RUBIS, R., & CARDEI, D. I. (2014). Patterns for fine-grain access-controlled business objects. PLoP.
