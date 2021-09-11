<!--ts-->
- [📖 Terminology](#-terminology)
- [🗄 Salesforce's Services](#-salesforces-services)
  - [🧩 Sales Cloud (SaaS)](#-sales-cloud-saas)
      - [⚙️ Built-in Objects](#️-built-in-objects)
      - [⚙️ Built-in Logic](#️-built-in-logic)
  - [🧩 Service Cloud (SaaS)](#-service-cloud-saas)
      - [⚙️ Built-in Objects](#️-built-in-objects-1)
      - [⚙️ Built-in Logic](#️-built-in-logic-1)
  - [🧩 Experience Cloud (PaaS)](#-experience-cloud-paas)
  - [🧩 Marketing Cloud (SaaS)](#-marketing-cloud-saas)
  - [🧩 Pardot (SaaS)](#-pardot-saas)
  - [🧩 Configuration Price Quotes (SaaS)](#-configuration-price-quotes-saas)
- [🗃 Additional Services](#-additional-services)
- [⛳️ What is next?](#️-what-is-next)
<!--te-->

# 📖 Terminology

**Salesforce Org** - an instance of the salesforce for a client. Can be either _Production_, Sandbox, Developer, Scratch.

**Production Org** - a production instance for a client.

**Sandbox Org** - a sandbox for experiments, linked to a _Production org_.

**Developer Org** - for creation of packages for AppExchange. Can be used for experiments.

**Scratch Org** - temporary instance (up to 30 days) for dedicated tasks or Proof of Concept implementation. When you can't set up Sandbox for each developer. It is used for CI/CD as well.

**Package** - a custom application with additional functionality or integration with external systems. Usually, it spreads by app store (AppExchange), sometimes by a link or subscription.

**Independent Software Vendor** - a developer of packages for AppExchange.

**AppExchange** - an official store of applications for Salesforce.

**Application** - often it is called a set of grouped tabs, rarely autonomous Aura-app, very rarely as a package.

**SaaS** - Software as a Service model provides web application in front of Cloud Solution.

**PaaS** - Platform as a Service model allows to deploy and scale services quickly, without deep configurations. PaaS company takes a responsibility for deploy process.

# 🗄 Salesforce's Services

Salesforce provides a lot of different solutions for different industries, some of them are standalone cloud projects, others you can install into Salesforce Org.

## 🧩 Sales Cloud (SaaS)

 The **Core** of the CRM. It has [built-in objects](#built-in-objects-sales) for work with a client and [business logic](#built-in-logic-sales) that allows making a pipeline and has reach tools for reporting.

#### ⚙️ Built-in Objects

 - Lead
 - Contract
 - Account
 - Opportunity
 - Order
 - Quote
 - etc.

#### ⚙️ Built-in Logic

 - Lead Conversion
 - Lead Assignment
 - Forecasting
 - etc.
 
 **Salesforce Console UI** allows working with several records concurrently.

## 🧩 Service Cloud (SaaS)

 Allows process requests of clients. It has set of [built-in objects](#built-in-objects-service) and [business-logic](#built-in-logic-service).

#### ⚙️ Built-in Objects

 - Lead
 - Contract
 - Account
 - Opportunity
 - Order
 - Quote
 - etc.

#### ⚙️ Built-in Logic

 - Lead Conversion
 - Lead Assignment
 - Forecasting
 - etc.

 The service cloud has its view of Salesforce UI - **Service Console** that allows working with several records without switching tabs.

## 🧩 Experience Cloud (PaaS)

As a web builder, uses for increasing collaboration with clients, partners, or employees. Allows to set up automatic export data from CMS. It has a lot of internal components, which copy features of Salesforce itself. Allows setting up everything: from the login page to the domain of the portal.

## 🧩 Marketing Cloud (SaaS)

B2C marketing solution gives the features like:

1. Email sending and monitoring
2. Building of landing pages
3. Different campaigns.

## 🧩 Pardot (SaaS)

B2B marketing solution allows landing page creation, monitor activity, evaluate potential customers and campaigns. Real-time statistics.

## 🧩 Configuration Price Quotes (SaaS)

Reach tool for marketing (sales and pricing), allows setup subscriptions. Its functionality can be extended by plugins.

There are a huge amount of different solutions for different fields, such as **Government Cloud** for government orgs, **Healthcare Cloud** unites patients and providers of medical services. **Vaccine Cloud** is built on Experience Cloud and integrated with Government Cloud, helps find clinics for vaccines and make the process easy for the government's services.

# 🗃 Additional Services

🏷️ **Force.com sites** (PaaS) - makes development and deployment of cloud applications easily.\
🏷️ **Chatter** - corporate messenger, allows creating of chat-bots.\
🏷️ **Omni channel** - a central point that unites different channels for conversation with customers.\
🏷️ **Einstein AI** - artificial intelligence from Salesforce, responsible for data analysis from Salesforce Org.\
🏷️ Built-in integrations with **Google** products (Gmail, Calendar, Contacts, Drive), **Microsoft** (Outlook), **Amazon AWS** which can set up _private connect_ instead of HTTP/HTTPS.

# ⛳️ What is next?
- [Development Notes](content/development.md) - relations and types, hight-level overview.
- [Tools](content/tools.md) - helpful tools and packages for development
- [Apex Notes](content/apex.md) - basic introduction to Apex language.