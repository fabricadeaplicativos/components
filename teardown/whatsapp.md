Whatsapp
========
Whatsapp application components.
> **Tags:** *chat*, *group-chat*, *media*, *notification*, *contacts*.

###Conventions
* **Maker** is the platform end user, an app maker.
* **User** is the app end user.
* **UIComponents** or **UIC** is here referred to classify all visual and non-visual components deployed on the target devices of the platform.
* **DomainEntities** or **DE** is the set of entities that can be either from the platform domain, the maker domain or a subset. Must be written in CamelCase.
* **Operations** are border components that provide all the interactions between the UIC and DE. Must be written in lowercase and be a **verb**.
```sequence
UIComponent->DomainEntity: operation
Note right of DomainEntity: operation is processed
DomainEntity-->UIComponent: result
```
* **Schema.org**[^schema] is used as standard for DE modelling convention.
* The [**Action**](http://schema.org/Action) schema is used to envelop all operations, being a suffix for all specialized actions.

###DomainEntities
 * [CommunicateAction](http://schema.org/CommunicateAction) is the entity that encapsulates the message that is sent between users through the app.
**Domain restrictions:**
* In this domain the `agent` and `recipient` properties are mandatory and **do not** accept the Organization type.
* The `actionStatus` property is used to register the message  status as: *sent*, *received* and *read*.
 * [EmailMessage](http://schema.org/EmailMessage) contains all message data.
 **Domain restrictions:**
* Is extendable by the Maker.
* Must be encapsulated by CommunucateAction.
 * [Person](http://schema.org/Person) gather the contacts information.
 * [PeopleAudience](http://schema.org/PeopleAudience) gather contacts group data.

###Operations
`create()`
> Fetch the Maker's custom data model. 
> **result:**  [EmailMessage](http://schema.org/EmailMessage)

`send(commAction)`
> Send user message to recipient.
> **param:** [CommunicateAction](http://schema.org/CommunicateAction)
> **result:** [ActionStatusType](http://schema.org/ActionStatusType)

`get()`
> Get messages sent to user.
> **result:** [CommunicateAction](http://schema.org/CommunicateAction)

`notify(commAction)`
> Notify message agent that current message was read.
> **param:** [CommunicateAction](http://schema.org/CommunicateAction)

###UIComponents
* Login Form
* Phone Number Input
* Alphanumeric Input
* Chat List View
* Chat Form
* Audio Button
* Input Combo -- *input+button*
* UISelector
 * type
* List View
 * collection
* ContactUI
 * layout 
* Action
 * src


[^schema]: [Schema.org](http://schema.org/docs/full.html) is a collaborative, community activity with a mission to create, maintain, and promote schemas for structured data on the Internet, on web pages, in email messages, and beyond.

