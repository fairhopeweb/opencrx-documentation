# Introduction to the openCRX UML model - Part II #

### Package _org.opencrx.kernel.document1_ ###
_document1_ allows to capture documents. The class _Document_ is the holder for 
the non-content related attributes of a document, i.e. the document name, status, 
author, keywords, etc. The content is attached as _DocumentRevision_s. A revision 
has a version number. There are different types of document revisions:

* _ResourceIdentifier_: the document content is managed in a 3rd party document
  management system. The content location is specified by an _URL_.
* _MediaReference_: As explained in model package _generic1_ all _CrxObjects_ are
  media holders. Any media object can be referenced by a _MediaReference_.
* _MediaContent_: Allows to upload and attach a media.

The document references the current head revision. Applications and users should 
access the current revision using the reference _Document.headRevision_. Any application
that uploads a revision must update the version and update the head revision correspondingly.
A specific revision can be set as head revision by setting _headRevision_ correspondingly.

The following additional information can be attached to a document:

* _DocumentLink:_ If a document references documents managed in 3rd party document 
  management systems the corresponding URIs can be added as document links.
* _DocumentLock:_ In case users or systems want to mark a document as locked a 
  corresponding lock object can be attached to the document.
* _DocumentAttachment:_ If required, media attachments can be attached to a document. 
  Attachments are not part of the content.
* _DocumentFolder:_ Documents can be assigned to one or more document folders. 

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/document1/010_Main.png)


### Package _org.opencrx.kernel.forecast1_ ###
The use of _forecast1_ is to capture any kind of forecast-related data. Currently
only budgets are supported.

The period of a _Budget_ entry is specified with _startingFrom_ and _endingAt_.
The budget values are stored as property set. The target of a budget  is either 
an account or an organizational unit. Examples where budgets can be used are

* Business volumes assigned to sales teams or sales persons
* Expense constraints for teams or organizational units
* Expected business volume generated by a customer,
* etc.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/forecast1/999_Budget.png)


### Package _org.opencrx.kernel.generic_ ###
As the name suggests _generic1_ contains generic classes and patterns which are
used in the other model packages. The most important class of _generic1_ is 
_CrxObject_. Almost all classes of _openCRX_ inherit from _CrxObject_.   

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/generic/010_Main.png)

A _CrxObject_ allows to attach media, notes, documents, ratings, links to external 
resources. Moreover, _CrxObject_s can be assigned to document folders which allows
to structure and group _CrxObject_s of various types. E.g. a customer file can
be captured as document folder. Objects related to this file such as
accounts, contracts, activities, products or notes can be assigned to this 
document folder.

All _CrxObject_s are secure, have an audit trail and are indexable. Moreover 
_CrxObject_ has a set of reserved fields which can be used to customize _openCRX_
without having to extend the model. The _disabled_ flag allows to mark objects
as disabled. _externalLink_s allow to store identifiers of 3rd party systems. They
are mainly used when objects are imported and the original IDs must be stored (e.g.
_ICAL_ id, _MIME_ message number, ...).
  
![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/generic/020_CrxObject.png)

Another powerful option to attach customer-specific values to an object are properties.
A _CrxObject_ can hold any number of _PropertySet_s which itself can hold any number
of typed _Property_s. The standard GUI of _openCRX_ allows to display properties
as standard attributes using the data binding feature of _openMDX/Portal_. 
 
![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/generic/060_Properties.png)


### Package _org.opencrx.kernel.home1_ ###
Each user which is allowed to login to _openCRX_ has a user home. _UserHome_ is
the main class of _home1_. 

A _UserHome_ has the following features:

* _Alert:_ The method _sendAlert()_ sends an alert to the listed recipients. Sending
  an alert results in the creation of an _Alert_ object which is attached to the 
  _UserHome_ of the receiving user.
* _QuickAccess:_ allows to store links to objects which are often accessed by the user.
  The _QuickAccessDashlet_ of the standard GUI uses this feature. 
* _AccessHistory:_ Allows to store the history of object accesses. This feature 
  is currently not used by the standard GUI.
* _ObjectFinder:_ The _ObjectFinder_ is used in combination with indexable objects.
  An _ObjectFinder_ searches the index entries by the ObjectFinder's keywords. 
  The index entries are calculated by a database view and returned in the form of 
  index entries referencing the index object. The _Search_ control of the standard 
  GUI uses this feature. When a user enters a search string it creates a corresponding 
  _ObjectFinder_ object and then navigates to it.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/home1/010_Main.png)
  
* _ExportProfile:_ All _CrxObject_s are _Exporter_s. The operation _exportItem()_
  takes as parameter an export profile which defines which objects and attributes
  have to be exported starting from the object the operation is invoked on. The current 
  version of _openCRX_ allows to export in the formats _XML_ and _XLS_. Export profiles 
  defined by the segment administrator are available to all users. 
* _SyncProfile:_ _SyncProfile_s are used by adapters such as _CardDAV_ and
  _CalDAV_. The _CalendarProfile_ allows to define the calendar collections exposed by 
  the _CalDAV_ servlet. For more information of how to configure the adapters see the 
  _openCRX administration guide_.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/home1/999_SyncProfile.png)
  
* _ServiceAccount:_ _ServiceAccount_s are used by services communicating with 
  external resources. The _EMailAccount_ allows the mail notifier to determine
  the user's e-mail address and the mail service to be used. The _TwitterAccount_
  allows to configure an account which is used by the twitter notification service. 
  For more information of how to configure service accounts see the _openCRX administration guide_.
 
![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/home1/999_ServiceAccount.png)

A user can make subscriptions to _Topic_s (for more information about topics see 
the _workflow1_ model package). A subscription for a specific topic instructs the 
_SubscriptionHandler_ that the user is interested in events published on that topic
either by other users, programs or external systems. In case an event is published
and the user has a subscription, the user receives a notification. The actions
performed to notify a user are defined by the _Topic_. Sample actions are 
sending an alert, sending an e-mail or sending a twitter direct message. For each 
workflow to be executed, a corresponding _WfProcessInstance_ describing the 
workflow to be executed is created. They are processed by the _WorkflowHandler_.  
 
![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/home1/999_Workflow.png)


### Package _org.opencrx.kernel.model1_ ###
CURRENTLY NOT USED


### Package _org.opencrx.kernel.product1_ ###
_product1_ defines the business objects for products, prices and price levels. A 
product is mainly used in the context of sales contracts. A product has the following
properties:

* __Price holder:__ Most important, a product is a price holder. Items which
 have a price can normally be captured as products in _openCRX_. If a product 
 is structured (i.e. when it is composed of other products or parts) it can 
 also be captured as _openCRX_ product. In these cases the "complex" product 
 is constructed using _is composed of_-relationships. The so constructed 
 "complex" product may have its own price and it can be used in sales contracts 
 as any other "simple" product. If no price is assigned, then a sales contract 
 should list the "complex" product and all its parts separately in a contract 
 position. __NOTE:__ In the current version of _openCRX_ the operation 
 _AbstractContract::createPosition()_ does not support "complex" products. 
* __Configuration holder:__ In addition, a product is a configuration holder. 
 Products which can be configured (e.g. modems) have an initial default 
 configuration. The possible default configurations are captured as property
 sets and stored with the product. When a product is used in a sales contract,
 the default configuration is "cloned" to the contract position. At this stage
 the product is called an _installed product_ or a _configured product_. 
 The configuration properties can be adapted as needed. Samples for configuration 
 properties are _color_, _form factor_, etc. It is important to note that 
 configuration properties can not be price relevant. If e.g. a product in color 
 _blue_ is more expensive than the same product in color _black_ it must 
 be treated as a different product.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/product1/020_AbstractProduct.png)

A product can have many prices. The following criteria (and some others) are
the input for the pricing rule (see below) which selects a price for a
given sales contract.

* __Currency:__ Currency of the price.
* __Price:__ Price amount. In case there are two or more prices matching all 
  criteria then the default pricing rule _Lowest Price_ selects the price 
  with the lowest price amount.
* __Usage:__ Allows to specify the usage of the price: _Consumer_, 
  _Retailer_, _Reseller_, etc.
* __Uom:__ The price is per unit. 
* __Quantity from/to__: In cases where there are discounts for large quantities
  _quantityFrom_ and _quantityTo_ allow to define a quantity range.  
* __Price level:__ A price can be listed on one or more price lists (or
  price levels). Price levels are created for specific time periods and
  customer segments (see below).

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/product1/999_Prices.png)

A price level lists product prices for a set of products, for a given time 
period and for a specific customer segment. E.g. a company changes their product
prices quarterly. Then one would create the price levels _Prices 1Q2010_,
_Prices 2Q2010_, _Prices 3Q2010_, etc. _validFrom_ and _validTo_ are
set correspondingly. If the company in addition needs to have different prices
for standard customers and good customers, this would result in price levels 
such as 

```
  Standard Prices 1Q2010
  Cat I Prices 1Q2010 
  Standard Prices 2Q2010
  Cat I Prices 2Q2010
  etc. 
```

The set of "Cat I customers" can be defined by correspondingly setting the 
account filter. As a next step the company maybe needs product prices in 
different currencies. In this case, additional price levels must be 
created with the _priceCurrency_ set correspondingly. This will result 
in additional price levels such as 

```
  EUR Standard 1Q2010
  EUR Cat I 1Q2010  
  EUR Standard 2Q2010
  EUR Cat I 2Q2010
  USD Standard 1Q2010
  USD Cat I 1Q2010  
  USD Standard 2Q2010
  USD Cat I 2Q2010
```

Finally, the company needs to maintain prices for the usages _Consumer_ and
_Retailer_. Again this results in additional price levels where the _usage_ must
be set:

```
  Name                                Currency     Usage       Valid from     Valid to
  EUR Standard 1Q2010 - Consumer      EUR          Consumer    01-01-2010     31-03-2020
  EUR Cat I 1Q2010 - Consumer         EUR          Consumer    01-01-2010     31-03-2020
  EUR Standard 2Q2010 - Consumer      EUR          Consumer    01-04-2010     30-06-2010
  EUR Cat I 2Q2010 - Consumer         EUR          Consumer    01-04-2010     30-06-2010
  USD Standard 1Q2010 - Consumer      USD          Consumer    01-01-2010     31-03-2020
  USD Cat I 1Q2010 - Consumer         USD          Consumer    01-01-2010     31-03-2020
  USD Standard 2Q2010 - Consumer      USD          Consumer    01-04-2010     30-06-2010
  USD Cat I 2Q2010 - Consumer         USD          Consumer    01-04-2010     30-06-2010
  EUR Standard 1Q2010 - Retailer      USD          Retailer    01-01-2010     31-03-2020
  EUR Cat I 1Q2010 - Retailer         EUR          Retailer    01-01-2010     31-03-2020
  EUR Standard 2Q2010 - Retailer      EUR          Retailer    01-04-2010     30-06-2010
  EUR Cat I 2Q2010 - Retailer         EUR          Retailer    01-04-2010     30-06-2010
  USD Standard 1Q2010 - Retailer      USD          Retailer    01-01-2010     31-03-2020
  USD Cat I 1Q2010 - Retailer         USD          Retailer    01-01-2010     31-03-2020
  USD Standard 2Q2010 - Retailer      USD          Retailer    01-04-2010     30-06-2010
  USD Cat I 2Q2010 - Retailer         USD          Retailer    01-04-2010     30-06-2010
```

A product should now have a price for each of the price levels, e.g.

```
Price Level                       Usage      Currency    Price      Uom
EUR Standard 1Q2010 - Consumer    Consumer   EUR         2.00       Piece(s)
EUR Cat I 1Q2010 - Consumer       Consumer   EUR         1.80       Piece(s)
EUR Standard 2Q2010 - Consumer    Consumer   EUR         2.10       Piece(s)
EUR Cat I 2Q2010 - Consumer       Consumer   EUR         1.90       Piece(s)
USD Standard 1Q2010 - Consumer    Consumer   USD         3.10       Piece(s)
USD Cat I 1Q2010 - Consumer       Consumer   USD         2.80       Piece(s)
USD Standard 2Q2010 - Consumer    Consumer   USD         3.10       Piece(s)
USD Cat I 2Q2010 - Consumer       Consumer   USD         2.90       Piece(s)
EUR Standard 1Q2010 - Retailer    Retailer   EUR         1.50       Piece(s)
EUR Cat I 1Q2010 - Retailer       Retailer   EUR         1.30       Piece(s)
EUR Standard 2Q2010 - Retailer    Retailer   EUR         1.60       Piece(s)
EUR Cat I 2Q2010 - Retailer       Retailer   EUR         1.40       Piece(s)
USD Standard 1Q2010 - Retailer    Retailer   USD         2.50       Piece(s)
USD Cat I 1Q2010 - Retailer       Retailer   USD         2.30       Piece(s)
USD Standard 2Q2010 - Retailer    Retailer   USD         2.60       Piece(s)
USD Cat I 2Q2010 - Retailer       Retailer   USD         2.40       Piece(s)
```
 
The prices can either be created manually are calculated automatically in case
the prices can be derived from the prices of an existing price level. A price level 
can be derived from an existing price level by setting _basedPriceLevel_ to
an existing price level and adding _PriceModifier_s as needed. The operation 
_calculatePrices()_ takes the prices from the underlying price level, 
applies the price modifiers and stores the resulting prices as new prices.
The prices of final price levels can not be updated automatically.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/product1/999_PriceLevel.png)

It is the task of the pricing rule to select a price level for a given sales
contract position. The pricing rule is programmed in _Java_ and it has access
to all information it needs to determine the price level. Typically a pricing rule 
determines the price level in several steps:

* In a first step it selects all price levels which match the currency, usage, uom, 
  validity date and quantity. The work is done if the product has exactly one price 
  matching these criteria. 
* In a second step the contract customer is checked whether it matches the 
  account filter. If the product has exactly one matching price the work is done. 
* In a third step it is up to the pricing rule to select one of the remaining prices. 
  In case of the _Lowest Price_ pricing rule the lowest price is selected. Other 
  user-defined pricing rules can check the criteria such as _paymentMethod_ 
  or _shippingMethod_.
 
![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/product1/999_PricingRule.png)

A product is a holder for product configurations (more information see below) 
and product descriptions (_DescriptionHolder_). If the standard _description_ field
is not sufficient to describe a product, detailed descriptions in different languages 
can be added as _Description_s. 

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/product1/010_Main.png)

Technically speaking, a product configuration is a property set. Products can have 
multiple configurations which are attached as _ProductConfigurationSet_s. 
In cases when several products share the same configuration properties,
a _ConfigurationType_ can be created. A _ConfigurationType_ can then be 
applied to individual products using the operation _setConfigurationType()_. 
It is important to note that product configurations do not have impact on the 
product price, i.e. product prices and product configurations must be considered 
as completely orthogonal. In cases where a specific value of a configuration 
property has impact on the price, a new product having its own prices must be 
created. E.g. if a _red_ modem is more expensive than a _black_ modem two 
individual products must be created.
 
![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/product1/080_ProductConfiguration.png)


### Package _org.opencrx.kernel.ras1_ ###
CURRENTLY NOT USED


### Package _org.opencrx.kernel.reservation1_ ###
CURRENTLY NOT USED


### Package _org.opencrx.kernel.uom1_ ###
Units of measurements (Uoms) are modeled as classes and not - as in many other
systems - mapped to integer values. This results in more flexibility in customizing 
_openCRX_. User-defined uoms and uom schedules can be added as needed and used in 
product definitions and sales contracts. Uoms can be structured, e.g. _1 ton_ can 
be captured as uom which is based on _kg_ with a factor of _1000_. E.g. this
structuring allows components such as _contract1_ to automatically 
dereference a complex uom and handle _1 ton_ as _1000 kg_.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/uom1/010_Main.png)


### Package _org.opencrx.kernel.workflow1_ ###
A workflow is a predefined, programmed task. In the current version of _openCRX_ 
a programmed task is a _Java_ class implementing either the interface _SyncWorkflow_ 
in case of a synchronous task or _AsyncWorkflow_ in case of an asynchronous task. 
Workflows are currently used in

* __activity1__: An _ActivityProcessTransition_ can define one or more actions
  which are executed when performing the transition. A _WfAction_ allows to
  execute a worfklow.
* __home1__: In case a user has made a _Subscription_ for a specific topic
  and an event is published for that topic, then the user is notified. 
  The "notification actions" which are performed are defined by the _Topic_
  by means of the listed workflows to be executed.

_openCRX_ comes with a set of standard workflows (also see the 
_openCRX administration guide_). Additional workflows can be added by programmers 
and registering them as _WfProcess_.

Users make _Subscriptions_ to _Topics_. A topic has a name and an object
identity pattern. Supported event types are _object modification_, _object creation_
and _object removal_. Whenever an object is created, modified are removed and 
the object's identity matches the topic's identity pattern all subscribers are 
notified. Notification takes place by the execution of the configured topic actions.

![](http://www.opencrx.org/opencrx/5.2/uml/opencrx-core/workflow1/010_Main.png)

--- End of Part II ---
