<!--ts-->
- [📡 Global Variables](#-global-variables)
- [📃 Localization](#-localization)
- [🧬 Database Model](#-database-model)
  - [📦 Сustom types](#-сustom-types)
  - [⛓ Relations between objects](#-relations-between-objects)
    - [🔌 Master-Detail](#-master-detail)
    - [🔌 Look Up](#-look-up)
    - [🔌 Many-to-Many](#-many-to-many)
    - [🔌 Self-relationship](#-self-relationship)
    - [🔌 External](#-external)
- [🥷🏻 Backend](#-backend)
- [🖥 Frontend](#-frontend)
- [🚊 Deployment CI/CD & DevOps](#-deployment-cicd--devops)
- [🔨 Development Process](#-development-process)
- [📦 Salesforce APIs](#-salesforce-apis)
<!--te-->

Salesforce development is very similar to Web Development, you have the same techniques, technologies, and tools. You can change backend, frontend, and even a database model. Of cause, there are some restrictions and limitations, will be covered later.

Salesforce is **completely cloud solution**, so you won't compile it on a local machine, but you can do it if necessary.

# 📡 Global Variables

To store the global variables salesforce uses **Custom Settings** tool or **Custom Metadata Types** that is widely used in package development.

The benefit of **Custom Settings** is the possibility to create unique hierarchical records (ex. user-specific profiles). On other hand, you can create relations between **Custom Metadata Types**. There are more differences between them.

# 📃 Localization

There is a tool as **Translation Workbench** for language versioning. During the development of custom components, you are recommended to use **Custom Labels** which support multi-language.

In Additional, there is a support of currency depending on the exchange rate for a specific date.

# 🧬 Database Model

The system contains a lot of [built-in objects](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_list.htm), you can create your own, but the amount of available custom classes is limited. **Ultimate Edition** allows create up to **2000** objects and **1000** to install as packages.

> An object that you can save in the database is called **SObject**, it's like a table in a relational database.

You can create objects through UI interface or **Metadata API** as well as it can be created automatically by digital tables.

There is a built-in privacy policy with [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulationhttps://en.wikipedia.org/wiki/General_Data_Protection_Regulation) regulation. For a text-encrypted field, **Classic Encryption** is used (AES 128bit). There is a non-free **Salesforce Shield** that allows encrypting not only text fields and uses AES 256 bit block encryption.

> If you have collected billions of records, you can use a special tool - **Salesforce Big Objects**.

## 📦 Сustom types

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

## ⛓ Relations between objects

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

# 🖥 Frontend

There are two versions of a user interface in Salesforce:
- Classic (old)
- Lightning 

The first tool for the creation of a custom UI was **SControl**, mostly there were HTML WebForms. Now, there is no possibility to use SControl but Salesforce supports products that were built earlier with SControl.\
The classic version has *JavaScript Buttons* that can do the simplest actions or call Apex code. As an analog of JavaScript Buttons at Lightning have become **Lightning Quick Actions**.

In both versions, all UI elements are grouped to **tabs** which are grouped to **App** as well. Usually, for one object, one tab exists. The tab can be a *separate component*, *App page*, *Record page* or *external web resource* is built-in with iFrame.

For now, there are two possibilities to create your own UI by using:
- **Visualforce** (VF) - the oldest one available. A page or a component is generated on the server-side but Apex code and page layout are made separately. It has a feature for page optimization. Anyway, several components of VF are hard to replace such as advanced email template, PDF generation, custom SObject List page.
- **Aura Framework** - comes with the Lightning engine. Client-side rendering allows transferring of a variable between parent- and built-in- components by value or reference (two-way relationship). There are application events (publish-subscribe) and component events for more difficult scenarios.
- **Lightning Web Components** (LWC) - the newest of available tools. Less flexible than Aura (no binding, dynamic components). However, it has standard possibilities of Web Components. You can create a component without HTML layout or otherwise, it can have several layouts. LWC is faster than others.
 
There is a possibility to call and subscribe for some system events. In addition, you can subscribe to Platform Events with EMP API. There are a lot of handlers for initialization and rendering points of components.

A big advantage is a *dynamic creation of components* and support of OOP, custom components can be extended, and some standard interfaces can be implemented as well.

Each Aura component (Aura bundle) can consist of the following parts:
- Component (HTML layout) **[required]**
- Controller (JS controller)
- Helper (Main part of a JS code)
- Style (CSS)
- Documentation
- Renderer (overrides of rendering flow)
- Design (attributes from a graphic builder)
- SVG (icon of the component).

> If you need a lib of JS functions, you can create a component with an empty tag or a separate JS file. The last one is loaded into *Static Resources* after that, you can include it in a component.

> There is a style library like Bootstrap - **SLDS** [Salesforce Lightning Design System], it includes by default for Aura components and LWC but in *Aura Apps* and *Visualforce SLDS* it has to be included manually. Read more about SLDS [here](https://www.lightningdesignsystem.com).

All the code, that is used by Aura and LWC, runs under *use strict* mode. In addition, Salesforce added a **Locker Service** that increases security for the frontend part. The data from the server is transferred as proxy objects, and a part of HTML layout and JS functions is not available. It increases the security level but makes the development of a custom UI more difficult.

> Read more about the **Locker Service** [here](https://developer.salesforce.com/docs/component-library/tools/locker-service-viewer).

There are more technologies, that can be mentioned here:
 - **Lightning Out** allows to built Aura components into Visualforce or an external resource.
 - **Lightning Message Service** allows transferring data between Auro, LWC, and Visualforce components. 
 - **Content Security Policy** [CSP] - default feature.
 - **Cross-Origin Resource Sharing** [CORS] - default feature.

> You can write **Unit Tests** for Aura and LWC as well.

Each technology has a set of standard components, some of them can be extended. [Lightning Component Library](https://developer.salesforce.com/docs/component-library/overview/components) for **Aura** and **LWC**, [Visualforce Component Library](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_compref.htm) for **Visualforce**.


<!-- grammarly -->

# 🚊 Deployment CI/CD & DevOps

**SFDX CLI** [Salesforce Developer Experience Command Line] was introduced at the end of 2017. It has been written on Node.js that provides easy integration with VCS (Version Control System) on Gitlab or Github. The additional feature of SFDX is the possibility to create Scratch Orgs in the fastest way, delete them, and deploy metadata on them.

There are several ways to deploy of Salesforce instance:
- **Change Sets** - sets of changes can be easily merged from scratch to production instances.
- **Ant** - deploy changes and destructive changes through the Ant.
- **IDE** - metadata are deployed through Metadata API.
- **SFDX CLI** - deployment of metadata describes in package.xml.
- **Workbench** - deployment of metadata by an archive, which contains changed metadata and a list of them in pakcage.xml.
- **External services** - such as GearSet, Flosum, etc.
- **Salesforce DevOps Center** - will be available soon.

# 🔨 Development Process

Usually, Agile (two-week sprints) is used for the development process with Salesforce, rarely Kanban.

The common flow of the development:
1. **Development** - a developer works on a feature in a separate sandbox or scratch org.
2. **Staging** [QA Testing] - the changes, which made by a developer, are transferred to QA Sandbox (staging). QA test and validate the changes.
3. **UAT** - one of the stakeholders (client) checks the changes in a separate sandbox that contains a part or full data from Production org.
4. **Deployment on Production**.

# 📦 Salesforce APIs

Salesforce has become an API-frist solution, but in older products, a part of functionality can be unavailable.

The most popular APIs:
- **REST API** - powerful, flexible, and simple mechanism for integration with mobile applications and web services.
- **SOAP API** - the same as above, optimized for real-time requests.
- **Bulk API** - REST-like endpoints for work with a big amount of data.
- **Metadata API** - used to getting, deploying, creating, deleting, or updating settings for an organization. Usually, it is used for merging changes from one org to another.
- **Tooling API** - a mix of REST and Metadata API, that allows creating of SOQL requests for metadata that are not available for the common SOQL. Usually, it is used by developers of IDEs.

> Salesforce uses **API Versioning** and provides a new version every season: Spring, Summer, Winter. It guarantees 3 years of minimum support for each API release.

The helpful packages and applications for salesforce development are [here](tools.md).


