---
layout: post
title:  System Design of Restaurant Management System
categories: [System Design]
---

We will take a look at the key features a restaurant management system needs to offer, its high-level, low-level design, database design, and some of the features that turnkey software solutions offer.

**Table of contents:**

1. Restaurant Management System
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

## Restaurant Management System
A restaurant management system is a software that helps the restaurant industry streamline their food business operations. It provides a complete set of features including Point of Sales system, payment processing, table reservations, inventory management , accounting and employee management.

## System Requirements
The system we will be designing will provide the most important features that every restaurant management system must offer. However, do note that a restaurant management system can have other sub-systems for accounting, employee management, etc.

###### Functional Requirements
The restaurant management system should at least include the following features,
1. Allow to add and remove item to order
2. Allow to add and remove table reservation
3. Allow to add and remove menu items
4. Generate bills
5. Manage payment of bills
6. Generate kitchen order tickets

A better understanding of the functional requirements can be gained from the use-case diagram below.

![UML-fee-mgmt](/images/content/2022/08/UML-fee-mgmt.png)

###### Non - Functional Requirements

**Usability** <br>
The system should provide an interactive user-friendly interface that is easily understandable for all users.

**Availability** <br>
The System should be available at least during the restaurant operating hours and must be recovered within an hour if it fails. The system should respond to the requests within two seconds.

**Dependability** <br>
The system should provide consistent performance with easy tracking of records and updating of records.

**Maintainability** <br>
The software should be easily maintainable. Adding additional features and making changes to the software must be as simple as possible.

**Security** <br>
Only authorized users can access the system to view and change the data.

###### Software Requirements
1. A server running Windows Server/Linux operating system.
2. A backend language such as like Java, Python to process the orders.
3. Front-end frameworks like Angular/React/Vue for the user interface.
4. Relational database management system such as MySQL, PostgreSQL.

## System Design
With our understanding of the functional and non-functional requirements of our system, we will now look at the system architecture, subsystems decomposition and database design.

###### Architecture of the System
The restaurant management system follows a simple 3 tier client/server architecture. The client can use web browsers on a tablet to access the system (restaurant menu) through the local area network of the restaurant using Hypertext Transfer Protocol Secure (HTTPS).

![System-Architecture](/images/content/2022/08/System-Architecture.png)

The middle tier includes the server which presents the website to the user and controls the business logic. It controls the interactions between the application and the user. The server also send the user orders to both the Point of Sales system and Kitchen Order Tickets printers. Common web server technology used here can be Apache, Nginx, etc.

The data tier maintains the application's data such as order data, menu data, reservation data, etc. It stores these data in a relational database management system like PostgreSQL. The client tier interacts with the server to make requests and retrieve data from the database. It then displays to the user the data retrieved from the server.

###### Subsystem Decomposition
Decomposing the system into smaller units called subsystems will help reduce the complexity of the system. Subsystems are just packages holding related classes. Our restaurant management system is also decomposed into subsystems as follows. The major subsystems are 'Authentication', 'Menu', 'Reservation', 'Order', and 'Kitchen' systems.

![subsystems](/images/content/2022/08/subsystems.png)

The Authentication subsystem authenticates a user to grant access based on the role of the user.

The Menu subsystem generates the restaurant menu, which involves assigning a price to each item.

The Reservation system aid the customer in the booking of table and handle the payment of the booking fee in the event of a no-show.

The Order subsystem registers the user selection from the menu. It compile the order for the Point of Sales system for billing. It also send the data to the Kitchen subsystem for processing.

The Kitchen subsystem deals with printing of the Kitchen Order Tickets and adjusting the inventory.

###### Low-Level Design
The following code shows some of the classes involved in Restaurant Management System Software.

```python
class Person:
    def __init__(
        self, 
        first_name: str, 
        last_name: str, 
        gender: str, 
        phone: int
        ):

class Staff(Person):
    def __init__(
        self, 
        userid: str,
        email: str,
        role: str,
        password: str):
        
    def login(self, email: str, password: str):
        
    def logout(self):
        
class Customer(Person):
    def __init__(
        self, 
        email: str,
        table_id: int):
        
    def add_item_order(self, table_id: int, menu_id: int):
        
    def remove_item_order(self, table_id: int, menu_id: int):
                
    def add_reservation(self, email: str, date: str, time: str):

    def edit_reservation(self, email: str, date: str, time: str):

class Restaurant(Staff):
    def __init__(self):
       
    def add_item_order(self, table_id: int, menu_id: int):
        
    def remove_item_order(self, table_id: int, menu_id: int):
                
    def add_reservation(self, email: str, date: str, time: str):

    def edit_reservation(self, email: str, date: str, time: str):        
                
    def create_invoice(self, student_id: int, amount: int):

class Kitchen(Staff):
    def __init__(self):
       
    def manage_inventory(self, item_id: int, quantity: int):        
                
    def view_order(self, order_id: int):

class Menu:
    def __init__(
        self, 
        menu_id: int, 
        menu_name: str, 
        menu_price: int, 
        menu_description: str, 
        menu_category: str, 
        menu_image: str,
        menu_quantity: int
        ):

class Inventory:
    def __init__(
        self, 
        inventory_id: int, 
        inventory_name: str, 
        inventory_cost: int, 
        inventory_description: str, 
        inventory_category: str, 
        inventory_quantity:int
        ):
```
The `Person` class is the base class for all the employees of the restaurant. The `Staff` class adds the necessary information for authenticating a user. The `Restaurant` class inherits from the `Staff` class and has functions to add and remove items to the order, manage table reservations and create invoice to bill the customer.

The `Kitchen` class also inherit from the `Staff` class. The `Kitchen` class can view the customer order and manage the inventory of the ingredients they used.

The `Customer` can add item and remove item to the order before submitting. In addition they can create table reservation request and amend it.

The `Menu` class contains information about the item on the restaurant menu. The `Inventory` class contains information to keep track of the ingredients used by the restaurant to prepare the food.

###### Database Design
A restaurant management system software needs to store data about the Order, Menu, Reservations, etc. Therefore, we have identified the major tables that will be implemented on the selected relational database management system.

![database-design](/images/content/2022/08/database-design.png)

The above database diagram shows the schema for the restaurant management software database.

In a relational database system, relationships often come in three different types.  These are one-to-one, one-to-many, and many-to-many relationships. The system under consideration has one-to-many and many-to-many relationships.

The Reservation, Order and Menu tables store data of the reservations, orders and menu. Each Order is associated with one or many order items. Each order item has a food item and ingredients associated with it. The Menu table acts as relation between the Restaurant_Staff and Food_Item tables. The Payment table stores the details about a specific order.

## Turnkey Software Solutions
Below are some of the turnkey software solutions available:
* LimeTray
* LAVU
* Zoho

In general, the features provided by the turnkey software solutions include customer facing tools such a web base menu to take orders and table reservations. The software provide automatic processing and tranmit the data to the Point Of Sales system for billing. The system also can digitize kitchen order tickets by displaying orders on-screen and provide tools for inventory management.

This article was originally published at OpenGenus:

[link to my article at OpenGenus](https://iq.opengenus.org/system-design-of-restaurant-management-system/ "my article at OpenGenus")