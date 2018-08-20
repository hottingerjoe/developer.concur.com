---
title: Receipt v4 - Schemas of Supported Receipt Types
layout: reference
---

Receipts can be any of the following types

* [Air](#air-receipt)
* [Car Rental](#car-rental-receipt)
* [General](#general-receipt)
* [Ground Transport](#ground-transport-receipt)
* [Hotel](#hotel-receipt)
* [Rail](#rail-receipt)

See the schema documentation below for the specifications of each type, plus the various schemas that are shared components of each receipt schema.

## Air Receipt

Schema for airline receipts.

* Includes all of [Receipt Core Definitions](#receipt-core-definitions)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
tickets|array|[tickets](#tickets)|Air tickets issued.
lineItems|array|[lineItems](#line-item)|Ancillary airline fees.

### tickets

Property Name|Type|Format|Description
---|---|---|---
number|string|-|**Required** Ticket number issued by the airline when the payment is made. Ticket numbers are globally unique for all IATA carriers. The first 3 digits identify the airline. The three digit code for each airline can be found [here](http://www.iata.org/about/members/Pages/airline-list.aspx?All=true). For example the ticket number for American Airlines where 001 is the airline: 0012375432602.
recordLocator|string|-|Confirmation identifier for the ticket created by the airline. For most airlines this is a 6 character alphanumeric code that is unique for a short period of time and could be reused in the future.
issueDateTime|string|date-time|**Required** Date and time the ticket was issued.
pseudoCityCode|string|[IATACityCode](#format-IATACityCode)|IATA city code the ticket was issued from. For example, SEA for Seattle.
IATAAgencyNumber|string|^[0-9]{8}$|Identifying number assigned by the IATA to the agency issuing the ticket.
agencyName|string|-|Name of the agency issuing the ticket.
passengerName|string|-|**Required** Name of the passenger associated with the ticket.
coupons|array|[coupons](#coupons)|**Required** Flights issued within this transaction.

### coupons

Property Name|Type|Format|Description
---|---|---|---
originationAirportIATACode|string|[IATAAirportCode](#format-IATAAirportCode)|**Required** IATA airport code of the flight’s origin.
originationDateTime|string|date-time|**Required** Date and time of origin.
destinationAirportIATACode|string|[IATAAirportCode](#format-IATAAirportCode)|**Required** IATA airport code of the flight’s destination.
destinationDateTime|string|date-time|**Required** Date and time of destination.
flightNumber|string|-|Flight identifier.
couponNumber|string|[nonEmptyString](#format-nonEmptyString)|**Required** Identifier associated with the given coupon.
operatingAirlineCode|string|^[a-zA-Z]{2}$|**Required** IATA code of the airline operating the flight.
marketingCarrier|string|^[a-zA-Z0-9]{3,8}$|**Required** Flight designator booking the flight.
operatingCarrier|string|^[a-zA-Z0-9]{3,8}$|**Required** Flight designator operating the flight.
classOfServiceCode|string|^[a-zA-Z]$|**Required** Class of service per the airline’s class of service codes. Most airlines use the same codes but some airlines have custom codes.
fareBasisCode|string|^[a-zA-Z0-9]{2,8}$|**Required** Rate code the airline used to calculate the fare for this flight.
ticketDesignatorCode|string|^[a-zA-Z0-9\*?]{1,10}$|A valid ticket designator code to indicate what type of discount is applied, such as for a child or infant, or airline employee. This is a 1 to 10 alphanumeric code and can optionally include a single asterisk. Ticket designators are free-form text codes which help identify ticket types. Airlines determine which ticket designators they will use as no standards currently exist.
fare|string|^[-]?\d*\.?\d+$|**Required** Fare charged for the flight.
taxes|array|[Taxes](#taxes)|Schema for objects that make up an array of taxes. Used in most receipt types.
lineItems|array|[lineItems](#line-item)|Line Items/fees specific to a leg of the trip. Eg. Baggage fees, class of service fees, priority boarding, meals.

### Definitions

Property Name|Type|Format|Description
---|---|---|---
<a name="format-IATAAirportCode">IATAAirportCode|string|^[a-zA-Z]{3}$|3-letter IATA code for an airport.
<a name="format-IATACityCode">IATACityCode|string|^[a-zA-Z]{3}$|3-letter IATA city code. For example, SEA for Seattle.
IATAAirlineCode|string|^[a-zA-Z]{2}$|2-letter code for an airline.
IATAAgencyNumber|string|^[0-9]{8}$|8-character ID number assigned by the IATA to an agency.
flightDesignator|string|^[a-zA-Z0-9]{3,8}$|-
classOfServiceCode|string|^[a-zA-Z]$|-
fareBasisCode|string|^[a-zA-Z0-9]{2,8}$|-
ticketDesignatorCode|string|^[a-zA-Z0-9\*?]{1,10}$|-

## Car Rental Receipt

Schema for car rentals. This does not include ride services or taxis.

* Includes all of [Receipt Core Definitions](#receipt-core-definitions)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
segmentLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of a single travel event in Concur’s Itinerary Service. An itinerary can contain one or more bookings and each booking can contain one or more segments. The segmentLocator uniquely identifies an event like a car rental with a specific start and end date or a single air segment/sector.
startDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
endDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
pickupLocation|object|[Location](#location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
dropoffLocation|object|[Location](#location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
rentalDays|integer|-|**Required** Total number of days for which the car was rented.
rentalAgreementNumber|string|-|Agreement identifier.
confirmationNumber|string|-|Booking confirmation identifier.
vehicle|object|[vehicle](#vehicle)|-
driverName|string|-|Name of the driver/renter of the vehicle.
distance|object|[distance](#distance)|Distance traveled.
odometerReadingOut|number|-|Odometer reading at the start of the rental period. A number with up to one decimal place is expected.
odometerReadingIn|number|-|Odometer reading at the end of the rental period. A number with up to one decimal place is expected.
additionalDriver|boolean|-|Additional approved driver (true) or not (false).
lineItems|array|[lineItems](#line-item)|Break down of all car rental charges. This could include daily rate, fees, insurance, GPS rental and other add-ons.

### vehicle

Property Name|Type|Format|Description
---|---|---|---
registrationNumber|string|-|Registration or license plate identifier.
description|string|-|Vehicle description, including year, make and model.
classReservedCode|string|^[a-zA-Z]{4}$|Four-letter Association of Car Rental Industry Systems Standard (ACRISS) car code.
classRentedCode|string|^[a-zA-Z]{4}$|Actual vehicle rented ACRISS identifier.
classChargedCode|string|^[a-zA-Z]{4}$|Car class code actually charged to the user.
engineSize|string|^[0-9]{1,4}$|Engine displacement in cubic centimeters.

### Definitions

Property Name|Type|Format|Description
---|---|---|---
acrissCarCode|string|^[a-zA-Z]{4}$|Four-letter Association of Car Rental Industry Systems Standard (ACRISS) car code.
engineSize|string|^[0-9]{1,4}$|Engine displacement in cubic centimeters.

## Common Definitions

Shared definitions that are utilized in multiple receipt types.

Format Name|Type|Regex|Description
---|---|---|---
dateTime|string|date-time|The dateTime validation validates for a subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; This is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.|
<a name="format-duration"></a>duration|string|`^(-)?P(?:(-?[0-9,.]*)Y)?(?:(-?[0-9,.]*)M)?(?:(-?[0-9,.]*)W)?(?:(-?[0-9,.]*)D)?(?:T(?:(-?[0-9,.]*)H)?(?:(-?[0-9,.]*)M)?(?:(-?[0-9,.]*)S)?)?$`|Duration of a time interval as defined in ISO 8601
<a name="format-nonEmptyString"></a>nonEmptyString|string|`^(?!\s*$).+`|Non-empty string. Length must be at least 1 character.
addressRegion|string|`^[a-zA-Z0-9]{1,3}`$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
addressCountry|string|country-code|2 or 3 character country code as defined in ISO 3166-1:2013
currency|string|`^[-]?\d*\.?\d+$`|String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
currencyCode|string|currency-code|3-letter currency code as defined in ISO 4217
latitude|number|-|Numeric latitude value between -90 and 90
longitude|number|-|Numeric longitude value between -180 and 180
positiveInteger|integer|-|Positive integer value of at least 1
positiveNumber|number|-|Positive number value of at least 0
negativeCurrency|string|`^[-]\d*\.?\d+$`|String representing a negative amount of money, normally used for a discount. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

### distance

Property Name|Type|Format|Description
---|---|---|---
totalDistance|number|-|**Required**
unit|-|-|**Required** Can be any of the following values: mi, km

### Discount

Schema for discounts, such as coupons or discount codes, that could be part of a transaction.

Property Name|Type|Format|Description
---|---|---|---
discountName|string|-|The name of the discount.
discountCode|string|-|The code for the discount.
discountRate|string|-|The percentage of discount provided.
discountAmount|string|^[-]\d*\.?\d+$|String representing a negative amount of money, normally used for a discount. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

## General Receipt

General receipt type for transactions that do not fall under one of the more specific receipt types. This might include retail stores or restaurants.

* Includes all of [Receipt Core Definitions](#receipt-core-definitions)

Property Name|Type|Format|Description
---|---|---|---
lineItems|array|[lineItems](#line-item)|Line items specified for general receipts.

## Ground Transport Receipt

Schema for ground transportation receipts. This includes essentially all forms of non-aerial transportation, _except_ those that run on railed tracks.

* Includes all of [Receipt Core Definitions](#receipt-core-definitions)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Non-empty string. Length must be at least 1 character.
segmentLocator|string|[nonEmptyString](#format-nonEmptyString)|Non-empty string. Length must be at least 1 character.
classOfService|string|[nonEmptyString](#format-nonEmptyString)|Non-empty string. Length must be at least 1 character.
startDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
endDateTime|string|date-time|A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
travelDuration|string|`^(-)?P(?:(-?[0-9,.]*)Y)?(?:(-?[0-9,.]*)M)?(?:(-?[0-9,.]*)W)?(?:(-?[0-9,.]*)D)?(?:T(?:(-?[0-9,.]*)H)?(?:(-?[0-9,.]*)M)?(?:(-?[0-9,.]*)S)?)?$`|Duration of a time interval as defined in ISO 8601
pickupLocation|object|[Location](#location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
dropoffLocation|object|[Location](#location)|Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.
distance|object|[distance](#distance)|Object representing a distance.
driverNumber|string|-|Unique identifier assigned by the ride company to a driver.
lineItems|array|[lineItems](#line-item)|Descriptive breakdown of the fare charged. For example: base fare, distance travelled, discount and other add-ons.

## Hotel Receipt

Schema for hotel receipts.

* Includes all of [Receipt Core Definitions](#receipt-core-definitions)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
segmentLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of a single travel event in Concur’s Itinerary Service. An itinerary can contain one or more bookings and each booking can contain one or more segments. The segmentLocator uniquely identifies an event like a car rental with a specific start and end date or a single air segment/sector.
property|object|[Location](#location)|Physical property location information for the hotel property. This is often different than the merchant location information.
confirmationNumber|string|-|Booking identifier.
checkInDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
checkOutDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
guests|array|[guests](#guests)|**Required** Guest information.
numberInParty|integer|-|Number of individuals for the stay.
room|object|[room](#room)|**Required**
nightsStayed|integer|-|**Required** Positive integer value of at least 1
lineItems|array|[lineItems](#line-item)|**Required**

### guests

Property Name|Type|Format|Description
---|---|---|---
guestNameRecord|string|-|The loyalty or membership number of the hotel guest.
firstName|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
lastName|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
address|object|[Address](#address)|Address of the guest. It is highly recommended that the business address of the guest is provided if the hotel is provided with one. Doing so will help VAT reclamation partners who work with companies, to have compliant receipts accepted by the tax authority when filing tax reclaims.

### property

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
address|object|[Address](#address)|**Required** Common address object used by all receipt types except for the JPT IC Card receipt, which uses [Address-Original](#address-original).

### room

Property Name|Type|Format|Description
---|---|---|---
roomNumber|string|-|Room number where the guest stayed.
roomType|string|-|Type of room where the guest stayed. For example, Standard, Deluxe, etc.
ratePlanType|string|-|Name of the rate plan according to which the guest was charged.
averageDailyRoomRate|string|^[-]?\d*\.?\d+$|**Required** Average of the daily room rates for the duration of the guests stay. Room rates usually differ from day to day.

## Japan Public Transportation (JPT) IC Card Definitions

Schema specifically for JPT IC Card receipts. Not for use with any other rail transactions.

Property Name|Type|Format|Description
---|---|---|---
user|string|-|**Required**
app|string|-|**Required**
dateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
total|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
subtotal|string|^[-]?\d*\.?\d+$|String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
taxesTotal|string|^[-]?\d*\.?\d+$|String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
currencyCode|string|currency-code|**Required** 3-letter currency code as defined in ISO 4217
merchant|object|[merchant](#merchant)|**Required**
payments|array|[Payments](#payments)|**Required**
taxes|array|[Taxes](#taxes)|Schema for objects that make up an array of taxes. Used in most receipt types.
reference|string|-|-
collectionReference|string|-|-
taxInvoice|boolean|-|-
icCardId|string|-|**Required** The unique identifier for the card with a maximum length of 16 characters.
segments|array|[segments](#segments)|**Required** The segments for the trip.

### segments

Property Name|Type|Format|Description
---|---|---|---
sequenceNumber|integer|-|**Required** Unique transaction identifier for every trip taken using the IC card.
dateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
fromStationCode|string|-|**Required** Departure station code of the route. This code is specified to the IC Card vendor. Concur Expense has a transcoding table to Expense location codes.
fromStationName|string|-|**Required** Departure station label of the route.
toStationCode|string|-|**Required** Arrival station code of the route. This code is specified to the IC Card vendor. Concur Expense has a transcoding table to Expense location codes.
toStationName|string|-|**Required** Arrival station label of the route.
fromIsCommuterPass|boolean|-|Whether or not the departure route is included in the commuter pass subscription of the employee.
toIsCommuterPass|boolean|-|Whether or not the arrival route is included in the commuter pass subscription of the employee.
distance|number|-|Positive number value of at least value as 0

### icCardSegment

Property Name|Type|Format|Description
---|---|---|---
sequenceNumber|integer|-|**Required** Unique transaction identifier for every trip taken using the IC card.
dateTime|string|date-time|**Required** Transaction date and time.
fromStationCode|string|-|**Required** Departure station code of the route. This code is specified to the IC Card vendor. Concur Expense has a transcoding table to Expense location codes.
fromStationName|string|-|**Required** Departure station label of the route.
toStationCode|string|-|**Required** Arrival station code of the route. This code is specified to the IC Card vendor. Concur Expense has a transcoding table to Expense location codes.
toStationName|string|-|**Required** Arrival station label of the route.
fromIsCommuterPass|boolean|-|Whether or not the departure route is included in the commuter pass subscription of the employee.
toIsCommuterPass|boolean|-|Whether or not the arrival route is included in the commuter pass subscription of the employee.
distance|number|-|Positive number value of at least value as 0

## Line Item

Generic line item. These objects are included in arrays in most receipt types.

Property Name|Type|Format|Description
---|---|---|---
sequenceNumber|integer|-|**Required** The order in which the item appears in the sequence of line items when the receipt is rendered by Concur.
dateTime|string|date-time|A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
reference|string|-|The item SKU, identifier or some other attribute the merchant uses to reference the item.
description|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
additionalDescription|string|-|-
semanticsCode|string|[nonEmptyString](#format-nonEmptyString)|The [Concur semantics code](https://developer.concur.com/api-reference/travel/itinerary/itinerary.html#semantics_codes) for the line item.
unitCost|string|^[-]?\d*\.?\d+$|Amount per unit.
quantity|integer|-|-
total|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
subtotal|string|^[-]?\d*\.?\d+$|String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
taxesTotal|string|^[-]?\d*\.?\d+$|String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
taxes|array|[Taxes](#taxes)|Schema for objects that make up an array of taxes. Used in most receipt types.
vatApplicable|boolean|-|If the line item was subject to a value added tax then true, if not then false.
amountIncludesVat|boolean|-|-
discounts|array|[discounts](#discount)|The discounts offered for this line item.

### Location

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
address|object|[Address](#address)|**Required** Common address object used by all receipt types except for the JPT IC Card receipt, which uses [Address-Original](#address-original).

### Merchant

Schema for an object representing a merchant. The broker and seller properties in all receipts use this schema.

Property Name|Type|Format|Description
---|---|---|---
name|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
description|string|-|Description of the service provided by the merchant.
taxId|string|-|The tax identification number assigned to the merchant by the national tax authority. If the partner is providing a tax invoice, then providing a tax identification number is recommended.
location|object|[Location](#location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

### Payments

The payments array allows for one or more payment methods used in the transaction to be defined. All payment methods defined within the array result in the value for total in the base object of the receipt. The JSON keyword ‘anyOf’ indicates at least one of the following is required and multiple can be present: [cash](#cash), [creditCard](#creditcard), [companyPaid](#companypaid), [digitalWallet](#digitalwallet) and / or [unusedTicket](#unusedticket).

#### cash

Property Name|Type|Format|Description
---|---|---|---
amount|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

#### creditCard

Property Name|Type|Format|Description
---|---|---|---
amount|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
cardDetail|object|[cardDetail](#cardDetail)|**Required** Credit card information.

#### cardDetail

Property Name|Type|Format|Description
---|---|---|---
cardType|-|-|**Required** Can be any of the following values: American Express, Diners Club, Discover, MasterCard, Visa, Carte Blanche, Enroute, Universal Air Travel, JCB, EuroCard
creditCardId|string|^[0-9]{4}$|Last four digits of the credit card number to meet FACTA and PCI requirements
authorizationCode|string|-|Authorization code for transaction.

#### companyPaid

Property Name|Type|Format|Description
---|---|---|---
source|-|-|**Required** Can be any of the following values: GhostCard, LodgeCard, DirectPay, Invoice
amount|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
cardDetail|object|[cardDetail](#cardDetail)|Credit card information.

#### cardDetail

Property Name|Type|Format|Description
---|---|---|---
cardType|-|-|**Required** Can be any of the following values: American Express, Diners Club, Discover, MasterCard, Visa, Carte Blanche, Enroute, Universal Air Travel, JCB, EuroCard
creditCardId|string|^[0-9]{4}$|Last four digits of the credit card number to meet FACTA and PCI requirements
authorizationCode|string|-|Authorization code for transaction.

#### digitalWallet

Property Name|Type|Format|Description
---|---|---|---
source|-|-|**Required** Can be any of the following values: ApplePay, AndroidPay, SamsungPay, PayPal, OlaMoney
amount|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

#### unusedTicket

Property Name|Type|Format|Description
---|---|---|---
ticketNumber|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
amount|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

#### cardDetail

Property Name|Type|Format|Description
---|---|---|---
cardType|-|-|**Required** Can be any of the following values: American Express, Diners Club, Discover, MasterCard, Visa, Carte Blanche, Enroute, Universal Air Travel, JCB, EuroCard
creditCardId|string|^[0-9]{4}$|Last four digits of the credit card number to meet FACTA and PCI requirements
authorizationCode|string|-|Authorization code for transaction.

## Rail Receipt

Schema for rail or train receipts.

* Includes all of [Receipt Core Definitions](#receipt-core-definitions)

Property Name|Type|Format|Description
---|---|---|---
itineraryLocator|string|[nonEmptyString](#format-nonEmptyString)|Unique ID of an itinerary (also know as a trip) in Concur’s Itinerary Service. An itinerary can contain one or more bookings from various sources.
lineItems|array|[lineItems](#line-item)|Break down of all charges which could include insurance purchased for all train tickets, paid wi-fi etc.
railTickets|array|[railTickets](#railtickets)|**Required**

### railTickets

Property Name|Type|Format|Description
---|---|---|---
ticketNumber|string|-|-
recordLocator|string|-|**Required** Confirmation identifier for the ticket. This code is usually unique for a short period of time and could be reused by the rail company in the future.
issueDateTime|string|date-time|**Required** Date and time the ticket was issued.
passengerName|string|-|**Required** Name of the person associated withthe ticket.
fare|string|^[-]?\d*\.?\d+$|Fare charged for a train ticket. This will be the total of all segments in this train ticket.
segments|array|[segments](#segments)|**Required** Segments for this train ticket.

### segments

Property Name|Type|Format|Description
---|---|---|---
departureStation|string|-|**Required** Name of the station from which the train is departing.
departureDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z - notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
arrivalStation|string|-|**Required** Name of the station where the train is arriving.
arrivalDateTime|string|date-time|**Required** A subset of ISO 8601 date-times. The first restriction is that the dateTime requires a date, a time (at least the hour portion), and a UTC offset. The second restriction is that the dateTime does not allow a time to be formatted in UTC time (2015-11-02T14:30Z* notice the Z) without an offset; this is because it would be impossible for us to know the original offset so we could not generate a receipt with the correct local time.
trainNumber|string|-|**Required** Train identifier
trainType|string|-|Type of train. For example TGV or TER in France.
classOfServiceCode|string|[nonEmptyString](#format-nonEmptyString)|**Required** The class of travel.
fare|string|^[-]?\d*\.?\d+$|Fare charged for this segment of the train ride.
taxes|array|[Taxes](#taxes)|Taxes paid for this segment.
lineItems|array|[lineItems](#line-item)|Line items specific to this segment. This could include meals, seat reservations, insurance etc.

## Receipt Core Definitions

Core values for all receipt types. All major receipt schemas include these core objects.

Property Name|Type|Format|Description
---|---|---|---
dateTime|string|date-time|**Required** Date and time of the transaction.
total|string|^[-]?\d*\.?\d+$|**Required** The total amount of the transaction including all lineitems and taxes.
subtotal|string|^[-]?\d*\.?\d+$|The amount in the transaction excluding taxes.
taxesTotal|string|^[-]?\d*\.?\d+$|The amount of tax paid in the transaction.
discountsTotal|string|^[-]\d*\.?\d+$|String representing a negative amount of money, normally used for a discount. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.
currencyCode|string|currency-code|**Required** Currency paid to the merchant.
broker|object|[Merchant](#merchant)|The entity that facilitates the transaction between the seller and end user.
seller|object|[Merchant](#merchant)|**Required** The entity providing service to the end user.
payments|array|[Payments](#payments)|**Required**
discounts|array|[discounts](#discount)|The discounts offered on this transaction.
taxes|array|[Taxes](#taxes)|Taxes paid as part of transaction.
reference|string|-|The unique receipt provider or merchant identifier for this receipt or invoice. This value can also be referred to as transaction number, check number, order ID or similar.
collectionReference|string|-|Use this key to group related receipts into a collection for credits, tips or other adjustments to the original transaction and looking at groups of receipts for analysis purposes. The reference and collectionReference keys typically have separate and unique values but could be the same for the first receipt in a collection.
taxInvoice|boolean|-|A tax invoice (true) or otherwise (false).

### Address-Original

Postal address schema used for JPT (Japan Public Transportation) receipts _only_.

Property Name|Type|Format|Description
---|---|---|---
address|string|-|-
address2|string|-|-
city|string|-|-
countrySubdivisionCode|string|^[a-zA-Z0-9]{1,3}$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
countryCode|string|country-code|**Required** 2 or 3 character country code as defined in ISO 3166-1:2013
postalCode|string|-|-

### Address

Common address object used by all receipt types except for the JPT IC Card receipt, which uses [Address-Original](#address-original).

Property Name|Type|Format|Description
---|---|---|---
streetAddress|string|-|-
addressLocality|string|-|City
addressRegion|string|^[a-zA-Z0-9]{1,3}$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
addressCountry|string|country-code|**Required** 2 or 3 character country code as defined in ISO 3166-1:2013
postalCode|string|-|-

### broker

Property Name|Type|Format|Description
---|---|---|---
name|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
description|string|-|Description of the service provided by the merchant.
taxId|string|-|The tax identification number assigned to the merchant by the national tax authority. If the partner is providing a tax invoice, then providing a tax identification number is recommended.
location|object|[Location](#location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

### seller

Property Name|Type|Format|Description
---|---|---|---
name|string|[nonEmptyString](#format-nonEmptyString)|**Required** Non-empty string. Length must be at least 1 character.
description|string|-|Description of the service provided by the merchant.
taxId|string|-|The tax identification number assigned to the merchant by the national tax authority. If the partner is providing a tax invoice, then providing a tax identification number is recommended.
location|object|[Location](#location)|**Required** Schema representing a location, including geographical information and a postal address. Used in multiple receipt types.

### Taxes

Schema for objects that make up an array of taxes. Used in most receipt types.

Property Name|Type|Format|Description
---|---|---|---
authority|object|[authority](#authority)|**Required** The country or subdivision that charged the tax as per ISO 3166-2:2013.
name|string|-|-
rate|number|-|**Required**
rateType|string|-|The rate type for the tax charged. For value added tax this could be Zero, Standard, Reduced, etc.
amount|string|^[-]?\d*\.?\d+$|**Required** String representing an amount of money. Should not include a currency code or symbol, as this information is included in the currencyCode field of the receipt.

### authority

Property Name|Type|Format|Description
---|---|---|---
addressCountry|string|country-code|**Required** 2 or 3 character country code as defined in ISO 3166-1:2013
addressRegion|string|^[a-zA-Z0-9]{1,3}$|1 to 3 character country subdivision code as defined in ISO 3166-2:2013
