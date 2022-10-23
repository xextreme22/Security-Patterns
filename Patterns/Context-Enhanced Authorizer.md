# **Context-Enhanced Authorizer**

## **Intent**
When an authorized subject requests data about an object, provide some contextual information to make this data more explicit and meaningful.

## **Example**
Ernest was wrongly arrested, and he was acquitted immediately after the error was discovered. He applied for a job several years later and he was turned down because his potential employer got a record from one of the several background databases available in the US and other countries that only said he was arrested.

## **Context**
Many institutions that provide data about individuals have a large amount of information, but the requestor needs to know what to ask. To decide or to understand better a problem, we need information which we might not even know exists and therefore we do not ask for it. This information is sensitive, and its access is usually controlled. We assume users are authenticated and authorized before they can get any data from the system. Because the additional information is provided for specific records, the CONTENT-DEPENDENT AUTHORIZER must first be applied.

## **Problem**
In many cases some data is not meaningful unless it has a context. Without the context the data requestor may come to erroneous conclusions. How can we provide the requestor with the necessary information to make a proper interpretation? 

The solution is affected by the following forces: 

- Relevancy—the additional information we return to the user must be relevant and meaningful; otherwise, we may confuse the requestor. 
- Overhead—the time to return the answer to a request should not be too long. 
- Policy—the owner of the data should have policies about what data to include in each context. 
- Accountability—we need to record the metadata associated to each query so we can audit it later in case of legal problems. 
- Transparency—the requestor should not have to indicate what specific data he wants as context. 
- Protection of the security information—the definition of contexts should be protected of tampering which could disclose sensitive information.

## **Solution**
Append to each predicate used to select objects a list of data fields that define a selected context for the request. The specific context is selected based on policies of the institution according to the application.

## **Structure**
Figure 22 shows the class diagram of this pattern. Add a context predicate to class Right that adds a list of values corresponding to the context to each retrieved record. Since the context is a further refinement of content access, the content predicate should be applied first. In the context of access control models, the context is what is called an obligation.

![](./Images/context-enhanced_authorizer_structure.png)

*Figure 22: Class diagram of the CONTEXT-ENHANCED AUTHORIZER pattern.*

## **Dynamics**
This solution is an extension of the Content-dependent access control pattern. The Context Data Manager evaluates the context predicate and appends to the original request the information indicated by the predicate. This information is produced by the Generate Context process of the Context Data Manager. Figure 23 shows the UC “Request information”.

![](./Images/context-enhanced_authorizer_dynamics.png)

*Figure 23: Use case: “Request information”.*

## **Implementation**
The institution must define policies about what data to use in each context, depending on the type of application. The predicate in the authorization rule must also have an “obligation”, an extension to the content-dependent authorization rules. 

The records that satisfy a specific condition must be retrieved first; the corresponding contexts are then appended to these records’.

## **Example Resolved**
The employer who turned down Ernest realized that they needed more information about potential employees and started using a context-dependent DBMS. Now any suspicious events about candidates can be better interpreted.

## **Consequences**
This pattern presents the following advantages: 

- Relevancy—we can define policies to return to the user only relevant and meaningful information. 
- Overhead—content-dependent access control must happen first. The extra overhead of adding a context is small for a reasonable amount of context information. 
- Policy—the owner of the data can have contexts to add to each type of request.
- Accountability—we can log the metadata associated to each query and we can audit it later. 
- Transparency—the requestor does not need to indicate what specific data he wants as context. 
- Protection of the security information—the metadata that describes authorization rules can include the contexts. 

Liabilities include: 

- Possible overhead in retrieving the data for the context. 
- It may not be clear for some applications what to put in the context.

## **Known Uses**
- This model comes from [1]. 
- CaseMap it is a tool from Lexis Nexis for legal teams during a trial that can be used to obtain relevant information for a case [2]. 
- Lexis Nexis uses the ECL language to correlate data from several databases and presents all what is known about a specific individual [3].

## **See Also**
- AUTHORIZATION: A.K.A.: Access Matrix. Describe who is authorized to access specific resources in a system, in an environment in which we have resources whose access needs to be controlled. It indicates for each active entity, which resources it can access, and what it can do with them. 
- CONTEXT-DEPENDENT ACCESS CONTROL: The access decision is based on the context of a collection of information rather than content within an object. A firewall makes a context-based access decisions when it considers state information on a packet before allowing it into the network; a cell phone has different rights according to a context including location of the user, subscribed services, etc. [4,5].

## **References**

[1] Fernández, E. B., Summers, R. C., & Coleman, C. D. (1975, May). An authorization model for a shared data base. In *Proceedings of the 1975 ACM SIGMOD international conference on Management of data* (pp. 23-31).

[2] LexisNexis. (n.d.). Case Map. http://www.lexisnexis.com/en-us/litigations/products/casemap.page

[3] Wikipedia contributors. (n.d.). ECL (data-centric programming language). Wikipedia. https://en.wikipedia.org/wiki/ECL\_%28data-centric\_programming\_language%29

[4] Kirkpatrick, M., & Bertino, E. (2009, April). Context-dependent authentication and access control. In *International Workshop on Open Problems in Network Security* (pp. 63-75). Springer, Berlin, Heidelberg.

[5] Nitsche, U., Holbein, R., Morger, O., & Teufel, S. (1998). *Realization of a context-dependent access control mechanism on a commercial platform* (pp. 160-170).

## **Sources**
Fernández, E. B., Monge, A. R., Carvajal, R., Encina, O., Hernández, J., & Silva, P. (2014, July). Patterns for content-dependent and context-enhanced authorization. In *Proceedings of the 19th European Conference on Pattern Languages of Programs* (pp. 1-10).
 