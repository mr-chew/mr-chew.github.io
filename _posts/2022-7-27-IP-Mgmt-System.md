---
layout: post
title:  System Design of IP/ Patent Management System
categories: [System Design]
---

We will take a look at the key features a IP/ Patent management system (IPMS) needs to offer, its high-level, low-level design, database design, and some of the features that turnkey software solutions offer.

**Table of contents:**

1. IP/ Patent Management System (IPMS)
    * Types of Intellectual Property (IP)
    * Patent Docketing
2. System Requirements
    * Functional Requirements
    * Non - Functional Requirements
    * Software Requirements
3. System Design
    * Architecture of the System
    * Subsystem Decomposition
    * Low-Level Design
    * Database Design
4. Turnkey Software Solutions

## IP/ Patent Management System (IPMS)
The IPMS is a management and policy encompassing tool that aids in the accumulation and enhancement of the values associated with a good intellectual property (IP) portfolio. When it comes to IP, strategy implementation involves input from several organizational subsystems. As a result, this aids in the maintenance of the framework as well as the portfolio. This process comprises of maintaining the patent portfolio, which includes increasing the flow of potential patents for the decision process. Valuation, cost management and determining the best conversion parameters to extract value from the patent are part of the process.

###### Types of Intellectual Property (IP)
Generally speaking, Intellectual Property (IP) include the following types: Copyrights, Patents, Trademarks and Trade Secrets.

Copyright refers to the legal rights that authors and artists have over their literary and artistic creations. Books, music, paintings, sculpture, and films are all covered by copyright, as are computer programs, databases, ads, maps, and technical drawings.

An invention is protected by a patent, which is an exclusive right awarded to the inventor. In general, a patent gives the patent owner the right to decide how - or whether - others can use his or her creation. The patent owner gives up this privilege in exchange for making technical information about the invention publicly available in the published patent document.

A trademark is a symbol that distinguishes one company's goods or services from those of other companies. Trademarks date back to ancient times, when artists used to sign their wares with their signature or "mark."

IP rights on proprietary knowledge that may be sold or licensed are known as trade secrets. Unauthorized acquisition, use, or disclosure of such secret information by others in a manner that is inconsistent with honest business practices is considered an unfair practice and a breach of trade secret protection.

###### Patent Docketing
A method or system for handling the patent application process is called patent docketing. The patent application procedure entails a significant amount of documentation. A new portfolio should be uploaded into the docketing system whenever the patent application is drafted. The docketer should make sure the information is specific and thorough. The portfolio contains the inventor's name, contact information, invention, industry served, and other related papers.

## System Requirements
The IP/ Patent management system should at least include the following features,

1. IP portfolio management such as
    * monitoring of patent application progress (such as submission of additional information)
    * monitoring of deadline of patent application
    * recording of data and documents to support patent application
    * generate report of patent application for submission to patent registration office    
2. IP search function of similar registered patent such as
    * searching within internal database of patents held by the organization
    * searching external database of patent registration office

A better understanding of the functional requirements can be gained from the use-case diagram below.
![UML-ip-mgmt](/images/content/2022/07/UML-ip-mgmt.png)

###### Non - Functional Requirements

**Usability**

The system should provide an interactive user-friendly interface that is easily understandable for all users.

**Availability**

The System should be available at least during the Patent Registration office operating hours and must be recovered within an hour if it fails. The system should respond to the requests within two seconds.

**Dependability**

The system should provide consistent performance with easy tracking of records and updating of records.

**Maintainability**

The software should be easily maintainable and adding new features and making changes to the software must be as simple as possible.

**Scalability**

For a large organization like a university, the expected user load for the system will be significant. University are a research center with a massive database of information. The ability to scale is critical to the system's adoption success.

Applying technology such Hadoop's MapReduce will provide the ability to scale the system. Hadoop is a highly scalable platform, due to its ability to store and distribute enormous data sets across a large number of machines. The servers used here are relatively inexpensive and can run in parallel. The system's processing power can be enhanced by adding extra servers. Traditional relational database management systems(RDBMS) couldn't handle big data sets at scale.

One such application of MapReduce is Distributed Grep. In plain-text data collections, Distributed Grep looks for lines that match a regular expression. It's used to look for a specific pattern in a large number of files. This will come in handy if the patent application reviewer has to look for related patents in a huge database.

**Security**

A user login mechanism will be established to prevent unauthorized employees from accessing the system. Any member who has access to the system will be requested to create a username and password that will allow them to use the system's capabilities.

To prevent unauthorized parties from accessing, viewing, or changing parts and information within the system, the system is expected to have several access levels depending on which staff member is accessing the system, such as Inventor, Reviewer or Approver. This means that certain aspects of the system will be restricted to a small group of people based on their position or authority within the organization.

On top of all the other security measures mentioned above, system authentication is by far one of the most significant forms of security that should be included into this system. For certain access levels, an authentication mechanism such as ID scanning may be used to validate specific staff members and grant them access to specific system items.

###### Software Requirements

1. A server running Windows Server/Linux OS.
2. A backend language such as like Java, Python to process the patent applications.
3. Front-end frameworks like Angular/React/Vue for the user interface.
4. Relational DBMS such as MySQL, PostgreSQL.

## System Design
With our understanding of the functional and non-functional requirements of our system, we will now look at the system architecture, subsystem decomposition, and database design.

###### Architecture of the System
The IP/ Patent management system follows a simple 3 tier client/server architecture. The client can use web browsers to access the system through the local area network of the organization using the Hypertext transfer protocol secure (HTTPS) protocol.

![System-Architecture](/images/content/2022/07/System-Architecture.png)

The middle tier which includes the server presents the website to the user and controls the business logic. It controls the interactions between the application and the user. The server also send notification to the user through the email server using the Simple Mail Transfer Protocol (SMTP). Common web server technology used here can be Apache, Nginx, etc.

The data tier maintains the application's data such as patent application data, supporting documents data, existing patent data etc. It stores these data in a relational database management system (RDBMS) like PostgreSQL. The client tier interacts with the server to make requests and retrieve data from the database. It then displays to the user the data retrieved from the server.

###### Subsystem Decomposition
Decomposing the system into smaller units called subsystems will help reduce the complexity of the system. Subsystems are just packages holding related classes. Our IP/ Patent management system is also decomposed into subsystems as follows. The major subsystems are 'Authentication', 'Documentation', 'Report', 'Monitor', and 'Research' systems.

![subsystems](/images/content/2022/07/subsystems.png)

The `Authentication` subsystem authenticates a user to grant access based on the role of the user.

The `Documentation` subsystem facilitate the creation, review and approval of a patent application.

The `Report` subsystem generate the patent application report for submission to the patent registration office.

The `Monitor` subsystem monitor the progress of the patent application. In addition, it send reminder/notification of any upcoming patent application deadline.

The `Research` subsystem deals with retrieval of similar patent that is registered or held by the organization.

###### Low-Level Design
The following code shows some of the classes involved in IP/ Patent Management System Software.

```python
class Staff:
    def __init__(
        self, 
        first_name: str, 
        last_name: str, 
        gender: str, 
        phone: int
        userid: str,
        email: str,
        role: str,
        password: str
        ):
        
    def login(self, email: str, password: str):
        
    def logout(self):
        
class Inventor(Staff):
    def __init__(self):
       
    def add_patent_application(self, patent_id: int, patent_details: str, doc_filename: str):  
    
    def edit_patent_application(self, patent_id: int, patent_details: str, doc_filename: str):  

class Reviewer(Staff):
    def __init__(self):
       
    def view_patent_application(self, patent_id: int, review_comments: str):  
    
class Approver(Staff):
    def __init__(self): 
        
    def approve_patent_application(self, patent_id: int, review_comments: str, approval_status: str):
      

class Patent:
    def __init__(
        self, 
        patent_id: int, 
        patent_name: str, 
        patent_details: str, 
        doc_filename: str, 
        review_comments: str, 
        approval_status: str
        ):
```
The `Staff` class is the base class for all the employees of the organization which include the necessary information for authenticating a user. The `Inventor` class inherits from the `Staff` class and has functions to add and edit the patent application.

The `Reviewer` class also inherit from the `Staff` class. The `Reviewer` class can view the patent application and add comments to request more information about the patent application.

The `Approver` class also inherit from the `Staff` class. The `Approver` class can view the patent application and approve/reject the submission of the patent application.

The `Patent` class contains information about the patent application.

###### Database Design
A IP/ Patent management system software needs to store data about the Patent application, existing patent, etc. Therefore, we have identified the major tables that will be implemented on the selected RDBMS.

![database-design](/images/content/2022/07/database-design.png)

The above database diagram shows the schema for the IP/ Patent management system software database.

Generally, there are three types of relationships in a relational database system. These are one-to-one, one-to-many, and many-to-many relationships. The system under consideration has one-to-many and many-to-many relationships.

The `Patent_Application` tables store data of the patent application. The `Patent_Application` table acts as relation between the `Inventor_Staff`, `Reviewer_Staff` and `Approver_Staff`. The `Approved_Patent` table stores the details about approved patents held by the organization.

## Turnkey Software Solutions
Below are some of the turnkey software solutions available:
* IPzen Professional
* Inteum
* FoundationIP
* Symphony

In general, the turnkey software solutions include customer facing tools such a web base interface to provide the following features on IP/ Patent management:

* Information Disclosure Management
* IP Portfolio Management
* Trademark Tracking
* Deadline Management
* Docket Management
* Document Management
* Patent Tracking
* Renewal Management
* Spend Management

This article was originally published at OpenGenus:

[link to my article at OpenGenus](https://iq.opengenus.org/system-design-of-ip-patent-management-system/ "my article at OpenGenus")