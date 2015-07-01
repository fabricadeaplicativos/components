99 Taxis
=======
99 Taxis application components.
> **Tags:** *route*, *location*, *notification*, *chat*.

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
* [**Schema.org**](http://schema.org/) is used as standard for DE modelling convention.
* The [**Action**](http://schema.org/Action) schema is used to envelop all operations, being a suffix for all specialized actions.

***
### Domain Entities

* [**Enumeration**](http://schema.org/Enumeration) is a list of objects of any type.
* [**Person**](http://schema.org/Person) is an entity able to encapsulate a system user with its properties like name, email, and so on. Both passenger and cab driver are represented through this entity.
* [**DateTime**](http://schema.org/DateTime) is a combination of date and time.
* [**Place**](http://schema.org/Place) is a location's representation.
* [**ReservationStatusType**](http://schema.org/ReservationStatusType), a set of values able to identify each status possibility. In order to fit the ride request status:
	*  [*pending*, *accepted*, *denied*, *confirmed*, *canceled*, *closed*]
* [**ItemAvailability**](http://schema.org/ItemAvailability) the driver's availability which might be: 
	* [*available*, *unavailable*, *onaRide*]
* [**TaxiReservation**](http://schema.org/TaxiReservation) is the entity that encapsulates the ride request. It is composed by:
	* provider:**Person** (the cab driver);
	* underName:**Person** (the passenger);
	* pickupTime:**DateTime**;
	* pickupLocation:**Place**;
	* reservationStatus:**ReservationStatusType**;

### Domain Operations
####Passenger claims for a ride
1. `getLocation():Place`
> Get the passenger's location based on GPS or passenger's input.
> **return**: a Place object corresponding to the passenger's location.

2. `findNearTaxi(Place):Person`
> It uses the passenger's location to find the nearest cab driver.
> **return**: a Person object corresponding to the nearest cab driver. 

3. `createRequest(Place, DateTime, Person, Person):TaxiReservation`
> It creates a request based on the passenger's location, passenger's info, pick up's location and time, and cab driver's info.
> **return**: a pending TaxiReservation instance.

4. `callTaxi(TaxiReservation)`
> It communicates the cab driver about the passenger's request.
> **return**: whether the cab driver accept the ride or not, also it changes the TaxiReservation's reservationStatus to accepted or denied.

5. `estimateTime(TaxiReservation):DateTime`
> Based on passenger's and driver's locations, it estimates the time to arrival after payment.
> **return**: a time in the future estimating the driver's arrival time.

#### Cab driver seeks passenger

1. `getLocaion():Place`
> It asks the driver's location for the location service.
> **return**: a Place instance corresponding to the current driver's location.

2. `setDriverState(ItemAvailability, Place)`
> It updates the driver's availability and the driver's current location.

3. `notifyRideRequest(TaxiReservation, ItemList:Place, DateTime)`
> This action is performed by the driver service towards the DriverMainUI. The ItemList holds many placed, which means the discrete points of a route.

4. `accept(TaxiReservation)`
> The driver might accept or deny the ride request.

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