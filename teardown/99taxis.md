99 Taxis components
===================

Conventions
-------------
* These components are built following [schema.org](http://schema.org/docs/full.html) conventions.
* **UIComponents** are the final HTML/CSS/JS components delivered to the browser.
* **Domain Entities** are the abstract representations of real world entities treated by the system.
* **Domain operations** the operations performed by the system, creating, changing and deleting entities.

***
### Passenger claims for a ride

#### UIComponents

#### Domain Entities

* **TaxiReservation** a ride request, which is composed by:
	* provider:**Person** - the taxi driver.
	* underName:**Person** - the passenger.
	* pickupTime:**DateTime** - the time when the passenger is picked up.
	* pickupLocation:**Place** - the place where the passenger is picked up.
	* reservationStatus:**Enumeration** - a range of values able to identify each status possibility (pending, accepted, denied, confirmed, canceled, closed).

#### Domain Operations

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

***
### Cab driver seeks passenger

#### UIComponents

#### Domain Entities

* **TaxiReservation** a ride request.
* **Place** a location.
* **Person** meaning the passenger or the driver.
* **DateTime** a time spot.
* **ItemAvailability** the driver's availability which might be: available, unavailable, onaRide.

#### Domain Operations

1. `getLocaion():Place`
> It asks the driver's location for the location service.
> **return**: a Place instance corresponding to the current driver's location.

2. `setDriverState(ItemAvailability, Place)`
> It updates the driver's availability and the driver's current location.

3. `notifyRideRequest(TaxiReservation, ItemList:Place, DateTime)`
> This action is performed by the driver service towards the DriverMainUI. The ItemList holds many placed, which means the discrete points of a route.

4. `accept(TaxiReservation)`
> The driver might accept or deny the ride request.

***