---
title: Receipt v4 - Schemas of Supported Receipt Types
layout: reference
---

See the schema documentation below for the specifications of each type, plus the various schemas that are shared components of each receipt schema.

* [Air](#receipt-air)
  * [tickets](#receipt-air-tickets)
  * [coupons](#receipt-air-coupons)
  * [Definitions](#receipt-air-definitions)
* [Car Rental](#receipt-car-rental)
  * [vehicle](#receipt-car-rental-vehicle)
  * [distance](#receipt-car-rental-distance)
  * [Definitions](#receipt-car-rental-definitions)
* [General](#receipt-general)
* [Ground Transport](#receipt-ground-transport)
* [Hotel](#receipt-hotel)
  * [guests](#receipt-hotel-guests)
  * [property](#receipt-hotel-property)
  * [room](#receipt-hotel-room)
* [Rail](#receipt-rail)
  * [railTickets](#receipt-rail-tickets)
  * [segments](#receipt-rail-segments)
* [Japan Public Transportation (JPT) IC Card](#receipt-jpt-ic)
  * [segments (JPT)](#receipt-jpt-ic-segments)
* [Core](#receipt-core)
* [Shared](#receipt-shared)
  * [Line Item](#receipt-shared-line-item)
  * [Location](#receipt-shared-location)
  * [Merchant](#receipt-shared-merchant)
  * [Payments](#receipt-shared-payments)
    * [cash](#receipt-shared-payments-cash)
    * [creditCard](#receipt-shared-payments-credit-card)
    * [companyPaid](#receipt-shared-payments-company-paid)
    * [digitalWallet](#receipt-shared-payments-digital-wallet)
    * [unusedTicket](#receipt-shared-payments-unused-ticket)
    * [cardDetail](#receipt-shared-payments-card-detail)
  * [Address](#receipt-shared-address)
  * [Address-Original (JPT Only)](#receipt-shared-address-original)
  * [broker](#receipt-shared-broker)
  * [seller](#receipt-shared-seller)
  * [taxes](#receipt-shared-taxes)
  * [authority](#receipt-shared-authority)
  * [Discount](#receipt-shared-discount)
* [Shared Definitions](#receipt-shared-definitions)

## <a name="receipt-air"></a>Air

Schema for airline receipts.

* Includes all of [Receipt Core](#receipt-core)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
tickets|array|[tickets](#receipt-air-tickets)|Air tickets issued.
lineItems|array|[lineItem](#receipt-shared-line-item)|Ancillary airline fees.

### <a name="receipt-air-tickets"></a>tickets

Property Name|Type|Format|Description
---|---|---|---
number|string|-|**Required** Ticket number issued by the airline when the payment is made. Ticket numbers are globally unique for all IATA carriers. The first 3 digits identify the airline. The three digit code for each airline can be found [here](http://www.iata.org/about/members/Pages/airline-list.aspx?All=true). For example the ticket number for American Airlines where 001 is the airline: 0012375432602.
recordLocator|string|-|Confirmation identifier for the ticket created by the airline. For most airlines this is a 6 character alphanumeric code that is unique for a short period of time and could be reused in the future.
issueDateTime|string|[datetime](#format-datetime)|**Required** Date and time the ticket was issued.
pseudoCityCode|string|[IATACityCode](#format-IATACityCode)|IATA city code the ticket was issued from. For example, SEA for Seattle.
IATAAgencyNumber|string|[IATAAgencyNumber](#format-IATAAgencyNumber)|Identifying number assigned by the IATA to the agency issuing the ticket.
agencyName|string|-|Name of the agency issuing the ticket.
passengerName|string|-|**Required** Name of the passenger associated with the ticket.
coupons|array|[coupons](#receipt-air-coupons)|**Required** Flights issued within this transaction.

### <a name="receipt-air-coupons"></a>coupons

Property Name|Type|Format|Description
---|---|---|---
originationAirportIATACode|string|[IATAAirportCode](#format-IATAAirportCode)|**Required** IATA airport code of the flight’s origin.
originationDateTime|string|[datetime](#format-datetime)|**Required** Date and time of origin.
destinationAirportIATACode|string|[IATAAirportCode](#format-IATAAirportCode)|**Required** IATA airport code of the flight’s destination.
destinationDateTime|string|[datetime](#format-datetime)|**Required** Date and time of destination.
flightNumber|string|-|Flight identifier.
couponNumber|string|[nonEmptyString](#format-nonEmptyString)|**Required** Identifier associated with the given coupon.
operatingAirlineCode|string|[IATAAirlineCode](#format-IATAAirlineCode)|**Required** IATA code of the airline operating the flight.
marketingCarrier|string|[flightDesignator](#format-flightDesignator)|**Required** Flight designator booking the flight.
operatingCarrier|string|[flightDesignator](#format-flightDesignator)|**Required** Flight designator operating the flight.
classOfServiceCode|string|[classOfServiceCode](#format-classOfServiceCode)|**Required** Class of service per the airline’s class of service codes. Most airlines use the same codes but some airlines have custom codes.
fareBasisCode|string|[fareBasisCode](#format-fareBasisCode)|**Required** Rate code the airline used to calculate the fare for this flight.
ticketDesignatorCode|string|[ticketDesignatorCode](#format-ticketDesignatorCode)|A valid ticket designator code to indicate what type of discount is applied, such as for a child or infant, or airline employee. This is a 1 to 10 alphanumeric code and can optionally include a single asterisk. Ticket designators are free-form text codes which help identify ticket types. Airlines determine which ticket designators they will use as no standards currently exist.
fare|string|[currency](#format-currency)|**Required** Fare charged for the flight.
taxes|array|[Taxes](#receipt-shared-taxes)|Schema for objects that make up an array of taxes. Used in most receipt types.
lineItems|array|[lineItem](#receipt-shared-line-item)|Line Items/fees specific to a leg of the trip. Eg. Baggage fees, class of service fees, priority boarding, meals.

### <a name="receipt-air-definitions"></a>Definitions

Property Name|Type|Format|Description
---|---|---|---
<a name="format-IATAAirportCode"></a>IATAAirportCode|string|^[a-zA-Z]{3}$|3-letter IATA code for an airport.
<a name="format-IATACityCode"></a>IATACityCode|string|^[a-zA-Z]{3}$|3-letter IATA city code. For example, SEA for Seattle.
<a name="format-IATAAirlineCode"></a>IATAAirlineCode|string|^[a-zA-Z]{2}$|2-letter code for an airline.
<a name="format-IATAAgencyNumber"></a>IATAAgencyNumber|string|^[0-9]{8}$|8-character ID number assigned by the IATA to an agency.
<a name="format-flightDesignator"></a>flightDesignator|string|^[a-zA-Z0-9]{3,8}$|-
<a name="format-classOfServiceCode"></a>classOfServiceCode|string|^[a-zA-Z]$|-
<a name="format-fareBasisCode"></a>fareBasisCode|string|^[a-zA-Z0-9]{2,8}$|-
<a name="format-ticketDesignatorCode"></a>ticketDesignatorCode|string|^[a-zA-Z0-9\*?]{1,10}$|-

## <a name="receipt-car-rental"></a>Car Rental

Schema for car rentals. This does not include ride services or taxis.

* Includes all of [Receipt Core](#receipt-core)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
segmentLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of a single travel event in Concur’s Itinerary Service. An itinerary can contain one or more bookings and each booking can contain one or more segments. The segmentLocator uniquely identifies an event like a car rental with a specific start and end date or a single air segment/sector.
startDateTime|string|[datetime](#format-datetime)|**Required**
endDateTime|string|[datetime](#format-datetime)|**Required**
pickupLocation|object|[Location](#receipt-shared-location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
dropoffLocation|object|[Location](#receipt-shared-location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
rentalDays|integer|-|**Required** Total number of days for which the car was rented.
rentalAgreementNumber|string|-|Agreement identifier.
confirmationNumber|string|-|Booking confirmation identifier.
vehicle|object|[vehicle](#receipt-car-rental-vehicle)|-
driverName|string|-|Name of the driver/renter of the vehicle.
distance|object|[distance](#receipt-car-rental-distance)|Distance traveled.
odometerReadingOut|number|-|Odometer reading at the start of the rental period. A number with up to one decimal place is expected.
odometerReadingIn|number|-|Odometer reading at the end of the rental period. A number with up to one decimal place is expected.
additionalDriver|boolean|-|Additional approved driver (true) or not (false).
lineItems|array|[lineItem](#receipt-shared-line-item)|Break down of all car rental charges. This could include daily rate, fees, insurance, GPS rental and other add-ons.

### <a name="receipt-car-rental-vehicle"></a>vehicle

Property Name|Type|Format|Description
---|---|---|---
registrationNumber|string|-|Registration or license plate identifier.
description|string|-|Vehicle description, including year, make and model.
classReservedCode|string|[acrissCarCode](#format-acrissCarCode)|Four-letter Association of Car Rental Industry Systems Standard (ACRISS) car code.
classRentedCode|string|[acrissCarCode](#format-acrissCarCode)|Actual vehicle rented ACRISS identifier.
classChargedCode|string|[acrissCarCode](#format-acrissCarCode)|Car class code actually charged to the user.
engineSize|string|[engineSize](#format-engineSize)|Engine displacement in cubic centimeters.

### <a name="receipt-car-rental-distance"></a>distance

Property Name|Type|Format|Description
---|---|---|---
totalDistance|number|-|**Required**
unit|-|-|**Required** Can be any of the following values: mi, km

### <a name="receipt-car-rental-definitions"></a>Definitions

Property Name|Type|Format|Description
---|---|---|---
<a name="format-acrissCarCode"></a>acrissCarCode|string|^[a-zA-Z]{4}$|Four-letter Association of Car Rental Industry Systems Standard (ACRISS) car code.
<a name="format-engineSize"></a>engineSize|string|^[0-9]{1,4}$|Engine displacement in cubic centimeters.

## <a name="receipt-general"></a>General

General receipt type for transactions that do not fall under one of the more specific receipt types. This might include retail stores or restaurants.

* Includes all of [Receipt Core](#receipt-core)

Property Name|Type|Format|Description
---|---|---|---
lineItems|array|[lineItem](#receipt-shared-line-item)|Line items specified for general receipts.

## <a name="receipt-ground-transport"></a>Ground Transport

Schema for ground transportation receipts. This includes essentially all forms of non-aerial transportation, _except_ those that run on railed tracks.

* Includes all of [Receipt Core](#receipt-core)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Non-empty string. Length must be at least 1 character.
segmentLocator|string|[nonEmptyString](#format-nonEmptyString)|Non-empty string. Length must be at least 1 character.
classOfService|string|[nonEmptyString](#format-nonEmptyString)|Non-empty string. Length must be at least 1 character.
startDateTime|string|[datetime](#format-datetime)|**Required**
endDateTime|string|[datetime](#format-datetime)|-
travelDuration|string|[duration](#format-duration)|Duration of a time interval as defined in ISO 8601
mapUrl|string|[Google Maps URI Template](#format-googlemaps)|Link to an image of the traveled route.
pickupLocation|object|[Location](#receipt-shared-location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
dropoffLocation|object|[Location](#receipt-shared-location)|Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
distance|object|[distance](#distance)|Object representing a distance.
driverNumber|string|-|Unique identifier assigned by the ride company to a driver.
lineItems|array|[lineItem](#receipt-shared-line-item)|Descriptive breakdown of the fare charged. For example: base fare, distance travelled, discount and other add-ons.

<a name="format-googlemaps"></a>**Google Maps URI Template**

```
^https://(www|maps).(googleapis|google).[a-z]+/maps/
```

## <a name="receipt-hotel"></a>Hotel

Schema for hotel receipts.

* Includes all of [Receipt Core](#receipt-core)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
segmentLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of a single travel event in Concur’s Itinerary Service. An itinerary can contain one or more bookings and each booking can contain one or more segments. The segmentLocator uniquely identifies an event like a car rental with a specific start and end date or a single air segment/sector.
property|object|[Location](#receipt-shared-location)|Physical property location information for the hotel property. This is often different than the merchant location information.
confirmationNumber|string|-|Booking identifier.
checkInDateTime|string|[datetime](#format-datetime)|**Required**
checkOutDateTime|string|[datetime](#format-datetime)|**Required**
guests|array|[guests](#receipt-hotel-guests)|**Required** Guest information.
numberInParty|integer|-|Number of individuals for the stay.
room|object|[room](#receipt-hotel-roomm)|**Required**
nightsStayed|integer|-|**Required** Positive integer value of at least 1
lineItems|array|[lineItem](#receipt-shared-line-item)|**Required**

### <a name="receipt-hotel-guests"></a>guests

Property Name|Type|Format|Description
---|---|---|---
guestNameRecord|string|-|The loyalty or membership number of the hotel guest.
firstName|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
lastName|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
address|object|[Address](#receipt-shared-address)|Address of the guest. It is highly recommended that the business address of the guest is provided if the hotel is provided with one. Doing so will help VAT reclamation partners who work with companies, to have compliant receipts accepted by the tax authority when filing tax reclaims.

### <a name="receipt-hotel-property"></a>property

Property Name|Type|Format|Description
---|---|---|---
name|string|-|The name for the location.
number|string|-|The identifier the company assigned to this location.
latitude|number|-|Numeric latitude value between -90 and 90
longitude|number|-|Numeric longitude value between -180 and 180
internetAddress|string|-|-
emailAddress|string|-|-
telephoneNumber|string|-|-
faxNumber|string|-|-
address|object|[Address](#receipt-shared-address)|**Required** Common address object used by all receipt types except for the JPT IC Card receipt, which uses [Address-Original](#receipt-shared-address-original).

### <a name="receipt-hotel-room"></a>room

Property Name|Type|Format|Description
---|---|---|---
roomNumber|string|-|Room number where the guest stayed.
roomType|string|-|Type of room where the guest stayed. For example, Standard, Deluxe, etc.
ratePlanType|string|-|Name of the rate plan according to which the guest was charged.
averageDailyRoomRate|string|[validString](#format-validString)|**Required** Average of the daily room rates for the duration of the guests stay. Room rates usually differ from day to day.

## <a name="receipt-rail"></a>Rail

Schema for rail or train receipts.

* Includes all of [Receipt Core](#receipt-core)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
lineItems|array|[lineItem](#receipt-shared-line-item)|Break down of all charges which could include insurance purchased for all train tickets, paid wi-fi etc.
railTickets|array|[railTickets](#receipt-rail-tickets)|**Required**

### <a name="receipt-rail-tickets"></a>railTickets

Property Name|Type|Format|Description
---|---|---|---
ticketNumber|string|-|-
recordLocator|string|-|**Required** Confirmation identifier for the ticket. This code is usually unique for a short period of time and could be reused by the rail company in the future.
issueDateTime|string|[datetime](#format-datetime)|**Required** Date and time the ticket was issued.
passengerName|string|-|**Required** Name of the person associated withthe ticket.
fare|string|[currency](#format-currency)|Fare charged for a train ticket. This will be the total of all segments in this train ticket.
segments|array|[segments](#receipt-rail-segments)|**Required** Segments for this train ticket.

### <a name="receipt-rail-segments"></a>segments

Property Name|Type|Format|Description
---|---|---|---
departureStation|string|-|**Required** Name of the station from which the train is departing.
departureDateTime|string|[datetime](#format-datetime)|**Required**
arrivalStation|string|-|**Required** Name of the station where the train is arriving.
arrivalDateTime|string|[datetime](#format-datetime)|**Required**
trainNumber|string|-|**Required** Train identifier
trainType|string|-|Type of train. For example TGV or TER in France.
classOfServiceCode|string|[nonEmptyString](#format-nonEmptyString)|**Required** The class of travel.
fare|string|[currency](#format-currency)|Fare charged for this segment of the train ride.
taxes|array|[Taxes](#receipt-shared-taxes)|Taxes paid for this segment.
lineItems|array|[lineItem](#receipt-shared-line-item)|Line items specific to this segment. This could include meals, seat reservations, insurance etc.

## <a name="receipt-jpt-ic"></a>Japan Public Transportation (JPT) IC Card

Schema specifically for JPT IC Card receipts. Not for use with any other rail transactions.

Property Name|Type|Format|Description
---|---|---|---
user|string|-|**Required**
app|string|-|**Required**
dateTime|string|[datetime](#format-datetime)|**Required**
total|string|[validString](#format-validString)|**Required** -
subtotal|string|[validString](#format-validString)|-
taxesTotal|string|[validString](#format-validString)|-
currencyCode|string|currency-code|**Required** 3-letter currency code as defined in ISO 4217
merchant|object|[merchant](#receipt-shared-merchant)|**Required**
payments|array|[Payments](#receipt-shared-payments)|**Required**
taxes|array|[Taxes](#receipt-shared-taxes)|Schema for objects that make up an array of taxes. Used in most receipt types.
reference|string|-|-
collectionReference|string|-|-
taxInvoice|boolean|-|-
icCardId|string|-|**Required** The unique identifier for the card with a maximum length of 16 characters.
segments|array|[segments](#receipt-jpt-ic-segments)|**Required** The segments for the trip.

### <a name="receipt-jpt-ic-segments"></a>segments (JPT IC)

Property Name|Type|Format|Description
---|---|---|---
sequenceNumber|integer|-|**Required** Unique transaction identifier for every trip taken using the IC card.
dateTime|string|[datetime](#format-datetime)|**Required** Transaction date and time.
fromStationCode|string|-|**Required** Departure station code of the route. This code is specified to the IC Card vendor. Concur Expense has a transcoding table to Expense location codes.
fromStationName|string|-|**Required** Departure station label of the route.
toStationCode|string|-|**Required** Arrival station code of the route. This code is specified to the IC Card vendor. Concur Expense has a transcoding table to Expense location codes.
toStationName|string|-|**Required** Arrival station label of the route.
fromIsCommuterPass|boolean|-|Whether or not the departure route is included in the commuter pass subscription of the employee.
toIsCommuterPass|boolean|-|Whether or not the arrival route is included in the commuter pass subscription of the employee.
distance|number|-|Positive number value of at least value as 0

## <a name="receipt-core"></a>Receipt Core

Core values for all receipt types. All major receipt schemas include these core objects.

Property Name|Type|Format|Description
---|---|---|---
dateTime|string|[datetime](#format-datetime)|**Required** Date and time of the transaction.
total|string|[currency](#format-currency)|**Required** The total amount of the transaction including all lineitems and taxes.
subtotal|string|[currency](#format-currency)|The amount in the transaction excluding taxes.
taxesTotal|string|[currency](#format-currency)|The amount of tax paid in the transaction.
discountsTotal|string|[negativeCurrency](#format-negativeCurrency)|String representing a negative amount of money, normally used for a discount. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
currencyCode|string|currency-code|**Required** Currency paid to the merchant.
broker|object|[Merchant](#receipt-shared-merchant)|The entity that facilitates the transaction between the seller and end user.
seller|object|[Merchant](#receipt-shared-merchant)|**Required** The entity providing service to the end user.
payments|array|[Payments](#receipt-shared-payments)|**Required**
discounts|array|[discounts](#receipt-shared-discount)|The discounts offered on this transaction.
taxes|array|[Taxes](#receipt-shared-taxes)|Taxes paid as part of transaction.
reference|string|-|The unique receipt provider or merchant identifier for this receipt or invoice. This value can also be referred to as transaction number, check number, order ID or similar.
collectionReference|string|-|Use this key to group related receipts into a collection for credits, tips or other adjustments to the original transaction and looking at groups of receipts for analysis purposes. The reference and collectionReference keys typically have separate and unique values but could be the same for the first receipt in a collection.
taxInvoice|boolean|-|A tax invoice (true) or otherwise (false).

## <a name="receipt-shared"></a>Shared Schema

Schema shared across multiple receipt types.

### <a name="receipt-shared-line-item"></a>Line Item

Generic line item. These objects are included in arrays in most receipt types.

Property Name|Type|Format|Description
---|---|---|---
sequenceNumber|integer|-|**Required** The order in which the item appears in the sequence of line items when the receipt is rendered by Concur.
dateTime|string|[datetime](#format-datetime)|-
reference|string|-|The item SKU, identifier or some other attribute the merchant uses to reference the item.
description|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
additionalDescription|string|-|-
semanticsCode|string|[nonEmptyString](#format-nonEmptyString)|The [Concur semantics code](https://developer.concur.com/api-reference/travel/itinerary/itinerary.html#semantics_codes) for the line item.
unitCost|string|[currency](#format-currency)|Amount per unit.
quantity|integer|-|-
total|string|[currency](#format-currency)|**Required** -
subtotal|string|[currency](#format-currency)|-
taxesTotal|string|[currency](#format-currency)|-
taxes|array|[Taxes](#receipt-shared-taxes)|Schema for objects that make up an array of taxes. Used in most receipt types.
vatApplicable|boolean|-|If the line item was subject to a value added tax then true, if not then false.
amountIncludesVat|boolean|-|-
discounts|array|[discounts](#receipt-shared-discount)|The discounts offered for this line item.

### <a name="receipt-shared-location"></a>Location

Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

Property Name|Type|Format|Description
---|---|---|---
name|string|-|The name for the location.
number|string|-|The identifier the company assigned to this location.
latitude|number|-|Numeric latitude value between -90 and 90
longitude|number|-|Numeric longitude value between -180 and 180
internetAddress|string|-|-
emailAddress|string|-|-
telephoneNumber|string|-|-
faxNumber|string|-|-
address|object|[Address](#receipt-shared-address)|**Required** Common address object used by all receipt types except for the JPT IC Card receipt, which uses [Address-Original](#receipt-shared-address-original).

### <a name="receipt-shared-merchant"></a>Merchant

Schema for an object representing a merchant. The broker and seller properties in all receipts use this schema.

Property Name|Type|Format|Description
---|---|---|---
name|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
description|string|-|Description of the service provided by the merchant.
taxId|string|-|The tax identification number assigned to the merchant by the national tax authority. If the partner is providing a tax invoice, then providing a tax identification number is recommended.
location|object|[Location](#receipt-shared-location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

### <a name="receipt-shared-payments"></a>Payments

The payments array allows for one or more payment methods used in the transaction to be defined. All payment methods defined within the array result in the value for total in the base object of the receipt. The JSON keyword ‘anyOf’ indicates at least one of the following is required and multiple can be present: [cash](#cash), [creditCard](#creditcard), [companyPaid](#companypaid), [digitalWallet](#digitalwallet) and / or [unusedTicket](#unusedticket).

#### <a name="receipt-shared-payments-cash"></a>cash

Property Name|Type|Format|Description
---|---|---|---
amount|string|[currency](#format-currency)|**Required** -

#### <a name="receipt-shared-payments-credit-card"></a>creditCard

Property Name|Type|Format|Description
---|---|---|---
amount|string|[currency](#format-currency)|**Required** -
cardDetail|object|[cardDetail](#receipt-shared-payments-card-detail)|**Required** Credit card information.

#### <a name="receipt-shared-payments-company-paid"></a>companyPaid

Property Name|Type|Format|Description
---|---|---|---
source|-|-|**Required** Can be any of the following values: GhostCard, LodgeCard, DirectPay, Invoice
amount|string|[currency](#format-currency)|**Required** -
cardDetail|object|[cardDetail](#receipt-shared-payments-card-detail)|Credit card information.

#### <a name="receipt-shared-payments-digital-wallet"></a>digitalWallet

Property Name|Type|Format|Description
---|---|---|---
source|-|-|**Required** Can be any of the following values: ApplePay, AndroidPay, SamsungPay, PayPal, OlaMoney
amount|string|[currency](#format-currency)|**Required** -

#### <a name="receipt-shared-payments-unused-ticket"></a>unusedTicket

Property Name|Type|Format|Description
---|---|---|---
ticketNumber|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
amount|string|[currency](#format-currency)|**Required** -

#### <a name="receipt-shared-payments-card-detail"></a>cardDetail

Property Name|Type|Format|Description
---|---|---|---
cardType|-|-|**Required** Can be any of the following values: American Express, Diners Club, Discover, MasterCard, Visa, Carte Blanche, Enroute, Universal Air Travel, JCB, EuroCard
creditCardId|string|^[0-9]{4}$|Last four digits of the credit card number to meet FACTA and PCI requirements
authorizationCode|string|-|Authorization code for transaction.

### <a name="receipt-shared-address"></a>Address

Common address object used by all receipt types except for the JPT IC Card receipt, which uses [Address-Original](#receipt-shared-address-original).

Property Name|Type|Format|Description
---|---|---|---
streetAddress|string|-|-
addressLocality|string|-|City
addressRegion|string|^[a-zA-Z0-9]{1,3}$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
addressCountry|string|country-code|**Required** 2 or 3 character country code as defined in ISO 3166-1:2013
postalCode|string|-|-

### <a name="receipt-shared-address-original"></a>Address-Original (JPT Only)

Postal address schema used for JPT (Japan Public Transportation) receipts _only_.

Property Name|Type|Format|Description
---|---|---|---
address|string|-|-
address2|string|-|-
city|string|-|-
countrySubdivisionCode|string|^[a-zA-Z0-9]{1,3}$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
countryCode|string|country-code|**Required** 2 or 3 character country code as defined in ISO 3166-1:2013
postalCode|string|-|-

### <a name="receipt-shared-broker"></a>broker

Property Name|Type|Format|Description
---|---|---|---
name|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
description|string|-|Description of the service provided by the merchant.
taxId|string|-|The tax identification number assigned to the merchant by the national tax authority. If the partner is providing a tax invoice, then providing a tax identification number is recommended.
location|object|[Location](#receipt-shared-location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

### <a name="receipt-shared-seller"></a>seller

Property Name|Type|Format|Description
---|---|---|---
name|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
description|string|-|Description of the service provided by the merchant.
taxId|string|-|The tax identification number assigned to the merchant by the national tax authority. If the partner is providing a tax invoice, then providing a tax identification number is recommended.
location|object|[Location](#receipt-shared-location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

### <a name="receipt-shared-taxes"></a>taxes

Schema for objects that make up an array of taxes. Used in most receipt types.

Property Name|Type|Format|Description
---|---|---|---
authority|object|[authority](#receipt-shared-authority)|**Required** The country or subdivision that charged the tax as per ISO 3166-2:2013.
name|string|-|-
rate|number|-|**Required**
rateType|string|-|The rate type for the tax charged. For value added tax this could be Zero, Standard, Reduced, etc.
amount|string|[currency](#format-currency)|**Required** -

### <a name="receipt-shared-authority"></a>authority

Property Name|Type|Format|Description
---|---|---|---
addressCountry|string|country-code|**Required** 2 or 3 character country code as defined in ISO 3166-1:2013
addressRegion|string|^[a-zA-Z0-9]{1,3}$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013

### <a name="receipt-shared-discount"></a>Discount

Schema for discounts, such as coupons or discount codes, that could be part of a transaction.

Property Name|Type|Format|Description
---|---|---|---
discountName|string|-|The name of the discount.
discountCode|string|-|The code for the discount.
discountRate|string|-|The percentage of discount provided.
discountAmount|string|[negativeCurrency](#format-negativeCurrency)|String representing a negative amount of money, normally used for a discount. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

## <a name="receipt-shared-definitions"></a>Shared Definitions

Shared definitions that are utilized in multiple receipt types.

Format Name|Type|Regex|Description
---|---|---|---
<a name="format-datetime"></a>dateTime|string|-|The dateTime validation validates for a subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; This is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
<a name="format-duration"></a>duration|string|`^(-)?P(?:(-?[0-9,.]*)Y)?(?:(-?[0-9,.]*)M)?(?:(-?[0-9,.]*)W)?(?:(-?[0-9,.]*)D)?(?:T(?:(-?[0-9,.]*)H)?(?:(-?[0-9,.]*)M)?(?:(-?[0-9,.]*)S)?)?$`|Duration of a time interval as defined in ISO 8601
<a name="format-nonEmptyString"></a>nonEmptyString|string|`^(?!\s*$).+`|Non-empty string. Length must be at least 1 character.
<a name="format-validString"></a>validString|string|`^[-]?\d*\.?\d+$`|A valid string
addressRegion|string|`^[a-zA-Z0-9]{1,3}`$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
addressCountry|string|country-code|2 or 3 character country code as defined in ISO 3166-1:2013
<a name="format-currency"></a>currency|string|`^[-]?\d*\.?\d+$`|String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
currencyCode|string|currency-code|3-letter currency code as defined in ISO 4217
latitude|number|-|Numeric latitude value between -90 and 90
longitude|number|-|Numeric longitude value between -180 and 180
positiveInteger|integer|-|Positive integer value of at least 1
positiveNumber|number|-|Positive number value of at least 0
<a name="format-negativeCurrency"></a>negativeCurrency|string|`^[-]\d*\.?\d+$`|String representing a negative amount of money, normally used for a discount. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
