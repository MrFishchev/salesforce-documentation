<!--ts-->
- [Global Variables](#global-variables)
- [Localization](#localization)
- [Database Model](#database-model)
  - [Сustom types](#сustom-types)
  - [Relations between objects](#relations-between-objects)
    - [🔌 Master-Detail](#-master-detail)
    - [🔌 Look Up](#-look-up)
    - [🔌 Many-to-Many](#-many-to-many)
    - [🔌 Self-relationship](#-self-relationship)
    - [🔌 External](#-external)
- [🥷🏻 Backend](#-backend)
<!--te-->

Salesforce development is very similar to Web Development, you have the same techniques, technologies, and tools. You can change backend, frontend, and even a database model. Of cause, there are some restrictions and limitations, will be covered later.

Salesforce is **completely cloud solution**, so you won't compile it on a local machine, but you can do it if necessary.

# Global Variables

To store the global variables salesforce uses **Custom Settings** tool or **Custom Metadata Types** that is widely used in package development.

The benefit of **Custom Settings** is the possibility to create unique hierarchical records (ex. user-specific profiles). On other hand, you can create relations between **Custom Metadata Types**. There are more differences between them.

# Localization

There is a tool as **Translation Workbench** for language versioning. During the development of custom components, you are recommended to use **Custom Labels** which support multi-language.

In Additional, there is a support of currency depending on the exchange rate for a specific date.

# Database Model

The system contains a lot of [built-in objects](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_list.htm), you can create your own, but the amount of available custom classes is limited. **Ultimate Edition** allows create up to **2000** objects and **1000** to install as packages.

> An object that you can save in the database is called **SObject**, it's like a table in a relational database.

You can create objects through UI interface or **Metadata API** as well as it can be created automatically by digital tables.

There is a built-in privacy policy with [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulationhttps://en.wikipedia.org/wiki/General_Data_Protection_Regulation) regulation. For a text-encrypted field, **Classic Encryption** is used (AES 128bit). There is a non-free **Salesforce Shield** that allows encrypting not only text fields and uses AES 256 bit block encryption.

> If you have collected billions of records, you can use a special tool - **Salesforce Big Objects**.

## Сustom types

Salesforce uses fields as a kind of tables column. There are several types of data for custom objects:
- **Auto Number** - increments record automatically, allows to create of a template.
- **Checkbox** - can be **true** or **false**.
- **Currency** - a currency type in ISO standard.
- **Date** - stores in GMT but shows with an offset, the offset depends on the time-zone of a user.
- **Date/Time** - like the previous one but allows keeping a time as well.
- **Email** - for storing Email addresses.
- **External Lookup Relationship** - a special type of relationship, that uses for integration with external systems.
- **Formula** - the value isn't stored in the database, it calculates on accessing it. There are several types for Formula type:
 - Checkbox
 - Currency
 - Date
 - Date/Time
 - Number
 - Percent
 - Text
 - Time
- **Geolocation** - allows to create coordinates.
- **Hierarchical Relationship** - a special type of relationship, which is available for the **User Object**.
- **Indirect Lookup Direction** - a special type of relationship between external objects.
- **Lookup Relaionship** - weak relationship.
- **Master-Detail Relationship** - the strong type of relationship, when a child(**detail**) inherits from a parent (**master**) an owner of sharing settings. When the master is deleted, the detail will be deleted automatically.
- **Number** - allows to store up to 18 numbers (ex. 18 numbers of an integer value, or 16 numbers of integer part and 2 of a fractional part).
- **Percent** - to storing of percentage, the view differs on UI, formulas, and Apex sources.
- **Phone** - fields for phone numbers, looks like as a text.
- **Picklist** - a dropdown in Salesforce. However, the database here is normalized, it still includes these fields, that are mismatched with normalization requirements.
- **Picklist (Multi-select)** - as the previous one, but values are stored as a split string with a semicolon.
- **Roll-Up Summary** - a special type is available only for **master** objects, makes calculations with aggregation functions (*COUNT*, *SUM*, *MAX*, *MIN*). The values of deterministic functions are stored in the database, other re-calculate if dependent **detail** records have changed.
- **Text** - a text field, allows to store up to **255** characters.
- **Text (Encrypted)** - a text field that is encoded with 128bit AES key, max length is **175** characters.
- **Text Area** - stores a text with a tab symbols, up to **131 072** characters.
- **Text Area (Rich)** - HTML text up to **131 072** with a limited set of possible tags.
- **Time** - the newest type that allows storing a time.
- **URL** - allows keeping hyperlinks.
- **Address - has to come soon**.

> **Email**, **Number**, and **Text** fields can be used as an *External ID*, it is convenient for both migration and integration processes.

Additional information for the custom fields can be found [here](https://help.salesforce.com/s/articleView?id=sf.custom_field_types.htm&type=5).

## Relations between objects

Many-to-many relation is created by **Junction Objects** which have two ***Master-Detail Relationship*** fields. Two fields are a maximum for this type of single object.

### 🔌 Master-Detail

One object acts as a master and the other acts as a detail object. Looks like a parent-child relationship. It is used when you want to bound two objects tightly or closely dependent on each other.

**The behavior:**
1. When a master record is deleted, the child (detail) object will be deleted automatically.
2. Both **master** and **detail** objects are strongly coupled to each other.
3. **Sharing** and **Security** settings of the detail record are inherited from the master record.
4. Can be defined between **Custom** and **Standard**, which the latest one has to be a **master** object.
5. A master object can have summary fields that help to calculate values from a child object by using aggregate functions such as Count, Sum, Min, Max.
6. A master-detail field is **required** on the detail record's page layout.
7. There is the only possibility to create a maximum of **2 master-detail** relationships per object.

> If we delete a child record and then restore it from the bin, the master-detail relationship gets lost at the time of deletion and does not get restored after the child (detail) getting restored.

### 🔌 Look Up

Both objects are loosely coupled, which means that if one gets deleted then the related object will not get deleted.

**The behavior:**
1. Objects are not strongly coupled.
2. When a parent's record is deleted, a child remains in existence.
3. **Not possible** to create summary fields.
4. Parent and child records have their own sharing and security settings.
5. Allowed to have a maximum **40** lookups per object.

> To convert a master-detail relationship to look-up we have to check that there are no roll-up summary fields available (**p.3**).

> To convert a look-up to master-detail we have to be sure that the **lookup field** in all records contains a value.

### 🔌 Many-to-Many

Records of particular objects are linked to multiple records of different objects and vice versa. There is no such field as a many-to-many relationship in Salesforce, we **can create a many-to-many** relationship by creating two master-detail relationships with a common object. This common object can also be specified as the junction (joining) object.

### 🔌 Self-relationship

Simply means creating a relationship with itself. In this, we can relate an object with itself by **lookup**. For example, the Account object has a field called **Parent Account** which shows the self-relationship in Account.

### 🔌 External

This is a new field type that has been introduced with **Salesforce Connect**. To link an external object to another external object, you can use the external relationship field. It supports a standard **lookup** relationship that uses **18 characters** Salesforce Id for the association.

# 🥷🏻 Backend

Salesforce's backend usually consists of **Apex** classes and triggers, a bit rarely Heroku functionality or integration with **MuleSoft** and **DellBoomi**, very rarely a custom **ETL**(*extract, transform, load*) solutions.

There is a new feature called **Salesforce Functions**. It is **FaaS**-tool, that allows implementing custom solutions with not only Apex but on any **other languages** then integrate it with a salesforce instance.

> You can read more about **Apex** [here](apex.md).