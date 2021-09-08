<!--ts-->
- [🧩 Apex Developer's Notes](#-apex-developers-notes)
  - [🧰 Data Manipulation](#-data-manipulation)
    - [🔩 DML - Data Manipulation Language](#-dml---data-manipulation-language)
    - [🔩 SOQL - Salesforce Object Query Language](#-soql---salesforce-object-query-language)
    - [🔩 SOSL - Salesforce Object Search Language](#-sosl---salesforce-object-search-language)
  - [🚧 Governor Limits](#-governor-limits)
  - [🚀 Asynchronous Apex](#-asynchronous-apex)
  - [🛎 Event-Based Architecture](#-event-based-architecture)
  - [🛡 Security & Access Control](#-security--access-control)
  - [✉️ Web & Email services](#️-web--email-services)
  - [🩺 Unit Testing](#-unit-testing)
  - [🛠 Debugging](#-debugging)
  - [💣 Triggers](#-triggers)
<!--te-->

# 🧩 Apex Developer's Notes

It is an appropriate Java-like object-oriented language, is developed for working with Salesforce CRM. The language is not case sensitive but stick to the register is a good practice.

There are two main approaches at the core of Salesforce's architecture:
- **transaction-based**
- **event-based**

But you still run a code on-demand or with a specific schedule. 

> ***static*** variables will be stored in a context of a single transaction (ex. on creation of a record and all synchronous and asynchronous actions called by this record).

There are several data types in Apex language:
- **Primitive** - Blob, Boolean, Date, Datetime, Decima, Double, Id, Integer, Long, Object, String, Time.
- **SObject** - standard and custom types that are stored in the database.
- **Collections** - List, Set, Map.
- Typed list of values (enum).
- User's Apex classes.
- System's Apex classes.
- Null.

> Standard and Custom types inherit from **SObject**, others are inherited from **Object**.

> You can implement your own types of lists and arrays, there is **Iterator** interface in salesforce. A **multi-map** is an often thing for implementation.

## 🧰 Data Manipulation

There are several tools for data manipulation in Apex:
 - DML - Data Manipulation Language
 - SOQL - Salesforce Object Query Language
 - SOSL - Salesforce Object Search Language

For manipulation among records, you can do not create a connection with the database and even create DTO classes. It is a big **advantage** of working with a database in Salesforce.

>You can generate documentation with well-written code by **ApexDoc**. For static analysis, you can use **PMD** or any service or plugin from the market.

### 🔩 DML - Data Manipulation Language

Supports the following operations: *insert, update, upsert, delete, undelete, merge*. It works with a single record or list of SObjects that was created by a user or was received by SOQL.

### 🔩 SOQL - Salesforce Object Query Language

SQL-like language, the main difference is the lack of *Join* support. Almost all features are the same but have some limitations.

The example of a SOQL request, which gets account's data and linked contacts:

~~~~ sql
SELECT Id, Name, (SELECT Id, Name FROM Contacts) FROM Account
~~~~

Contacts are linked with an account by **look-up** field **AccountId**.

SOQL returns the following:
- Single record (if *LIMIT 1* or *WHERE* has an exact value).
- An empty list of *SObject*, *Custom Settings* or *Custom Metadata Types* (when no records match the query).
- List of *SObject*, *Custom Settings* or *Custom Metadata Types*.
- Aggregated results or a single value (ex. *COUNT*).

For optimization you can use an index for a field, some fields are automatically indexed.

### 🔩 SOSL - Salesforce Object Search Language

The language is just for searching, it is used rarely than SOQL. It is quite useful in the implementation of global searching.

~~~~ sosl
FIND {"Joe Smith" OR "Joe Smythe”} IN Name Fields RETURNING lead(name, phone), contact(name, phone)
~~~~

## 🚧 Governor Limits

The execution of Apex code is limited to excluding the monopoly of sharing resources. There are limitations for everything in a transaction context: 
- Quantity of SOQL requests
- Quantity of SOSL requests
- Quantity of DML operations
- Heap size
- CPU time

For an asynchronous Apex, the restrictions are not so strong.
> You can read more about Governor Limits [here](https://developer.salesforce.com/docs/atlas.en-us.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_apexgov.htm).

> To follow the limitations you can read [Apex Design Best Practice](https://developer.salesforce.com/wiki/apex_code_best_practices).

## 🚀 Asynchronous Apex

There are several types of asynchronious Apex:
- **Future** - used to short callouts (up to 60 seconds).
- **Continuation** - used to long callouts, available for *VisualForce*, *Aura* and *LWC*.
- **Batch** - allows to process a lot of data asynchronously, helps to avoid Governor Limits.
- **Queueable** - like a Batch, used to process a lot of data, the main advantage over Batch is the possibility to call another Queueable (chain of calls).
- **Schedulable** - like a job, allows doing something by schedule.

## 🛎 Event-Based Architecture

To implementation of event-based architecture, **Platform Events** are used as well as **Change Data Capture Events**, common and async **triggers**.

**Platform Event** is a publish-subscribe model for the integration with external applications and isolation of hight-load functionality.

**Change Data Capture Event** is used for processing above changing a record's data.

## 🛡 Security & Access Control

You can take away or give access to an Apex class in the user's profile.

Apex has the following access modifiers: *private*, *protected*, *public*, and *global* that often is used for the development of external packages, less often for implementation of some standard interfaces.

On declaration of a class, you can provide keywords: 
- **with sharing** - all sharing rules, CRUD, FLS will be followed.
- **without sharing** - code will have an access to all records and full access to CRUD and FLS.
- **inherited** - sharing mode of the class depends on a class which has called it.

Example of a class declaration:
~~~~csharp
public inherited class AccountService {}
~~~~

## ✉️ Web & Email services

Apex allows developing full SOAP-, REST-, Web- and Email-services. For example, Apex REST supports the following returning options: 
- Apex Primitives (excluding Object and Blob)
- sObjects
- Lists and Maps of primitives of Apex or sObjects (only string-key maps)
- User's defined types, which contain fields - variables of types above.

You can use Email services for processing of internals (data of an email).
> You can create an email service, that will create a list of contacts from contacts information in email messages.

## 🩺 Unit Testing

You are not able to change sources directly on a production org but you can change code at sandbox then deploy a new version on a production.

> Code has to be covered not less than 75% and successfully deployed. But you can deploy *Sandbox to Sandbox* without these restrictions.

## 🛠 Debugging

There are **Debug Logs** with different levels of logging. There are **traced flags** for:
1. Automatic process
2. Platform integration
3. Specific user
4. Apex class
5. Apex trigger

Also, you can use breakpoints for debugging as well.

## 💣 Triggers

You are able to create ***before-*** and ***after-*** triggers for the following DML operations: *insert*, *update*, *delete*, *undelete* (after only).

Depending on the operation you can use the following context variables:
- **Trigger.new** - list with changed values
- **Trigger.old** - list with unchanged values
- **Trigger.newMap** - the same but as map view
- **Trigger.oldMap** - the same as above
- etc.

**Upsert** DML operation is a combination between *Insert* and *Update*.\
**Merge** is combination of *Update* and *Delete*.

> As a common practice, all triggers code is placed in **handlers**, reusable code is placed in **service** classes. However, a big amount of patterns and trigger-frameworks exist.

For optimization of triggers, asynchronous triggers are used, they will be called when a transaction is completed. To deploy a trigger on production org, you have to cover with unit tests at least one line of a trigger's code.

> Each Salesforce developer has to remember **Order of Execution**. During of DML-operation, not only a trigger will be called but a set of different declared functionality, that can cause recursion and as a result - Governor Limits will be exceded. You can read more about the Order of Execution [here](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm).