**Routofy B2B Integration Documentation**
---

---
### **iFrame URL**

_BASE_URL/dashboard/?vendorKey=ABC123&tokenNo=token1&txnId=txn1_

**Explanation:**

* BASE_URL: 
	* `Testing: http://staging.routofy.com` 
	* `Production: https://www.routofy.com`
* vendorKey: `Unique Vendor key provided by Routofy to partners.`
* tokenNo: `Partner generated user token number.` _Should have one to one mapping with a unique user_
* txnId: `Partner generated transaction ID` _Should have one to one mapping with each transaction and login session_

---
### **APIs TO BE PROVIDED BY PARTNERS**

#### **Required**
#### **1. Purpose**

  _Fetching verified user details for automatic authentication and for auto-filling travelling passengers_

#### **URL**

  `As chosen by partner`
  
**Recommended:**

 VENDOR_BASE_URL/vendor/user/details

**Example:**  

- Vendor : googol 
- User access token: AY132A871234
- Transaction Id: jkas3204-asljkd123-asdkj12-alksjd134
- Api Key(Optional): 2766491asd723

 Example Url: https://www.googol.com/vendor/user/details?uat=AY132A871234&txnId=jkas3204-asljkd123-asdkj12-alksjd134&apiKey=2766491asd723

#### **Method:**
  
  _GET_
  
####  **Query Params**

**Required:**

`uat : Partner generated user token number` _Should have one to one mapping with a unique user_

`txnId : Partner generated transaction ID` _Should have one to one mapping with each transaction and login session_
   
**Optional:**

   `apiKey : The provided api key by partner to Routofy to authenticate API requests`


####  **Headers**
   
   `Content-Type = application/json`

   `Accept = application/json`

   
#### **Data Params**

`Nothing`

#### **Success Response:**

  * **Code:** 200
  * **Sample Content:** [TRAVEL GROUP REST DTO](#travel-group-rest-dto)
  ```
  {
       "fn":"Prateek",
       "ln":"Grover",
       "em":"prateekgrover.rns@gmail.com",
       "mb":9999999999,
       "psgrs":[
           {
               "fn":"TravellerOneFirstName",
               "ln":"TravellerOneLastName",
               "em":"travelleroneemailid@gmail.com",
               "dob":1465037004000,
               "sx":"female",
               "mobNo":9999999999,
               "pp":"ABCD123",
               "pc":"India",
               "pi":1465037004000,
               "pe":1465037004000
           },
           {
               "fn":"TravellerTwoFirstName",
               "ln":"TravellerTwoLastName",
               "em":"travelleroneemailid@gmail.com",
               "dob":1465037004000,
               "sx":"female",
               "mobNo":9999999999,
               "pp":"ABCD123",
               "pc":"India",
               "pi":1465037004000,
               "pe":1465037004000
           }
       ]
  }
  ```
NOTE: If travelling passenger list is not known, then send empty list (```[]```) in "psgrs".

  * **Explanation:** `See` [TRAVEL GROUP REST DTO](#travel-group-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

 
#### **Error Response:**

  `See` [partner common error responses](#partner-common-error-responses) `section`

---

---
#### **Optional**
#### **2. Purpose**

  _Authorizing an action for a user_

#### **URL**

  `As chosen by partner`
  
**Recommended:**

_VENDOR_BASE_URL/itinerary/vendor/confirm_

**Example:**  

- Vendor : googol 
- User access token: AY132A871234
- Transaction Id: jkas3204-asljkd123-asdkj12-alksjd134
- Api Key(Optional): 2766491asd723

 Example Url: https://www.googol.com/itinerary/vendor/confirm?uat=AY132A871234&txnId=jkas3204-asljkd123-asdkj12-alksjd134&apiKey=2766491asd723

#### **Method:**
  
  _POST_
  
####  **URL Params**
 **Required:**
 
   `uat : Partner generated user token number` _Should have one to one mapping with a unique user_

   `txnId : Partner generated transaction ID` _Should have one to one mapping with each transaction and login session_
   
**Optional:**

   `apiKey : The provided api key by partner to Routofy to authenticate API requests`


####  **Headers**

   `Content-Type = application/json`

   `Accept = application/json`

#### **Data Params**

   * **Required Post Body:** [AUTHORIZATION REQUEST REST DTO](#authorization-request-rest-dto)
   ``` 
   {
       "amount":2000,
       "action":"book"
   }
   ```

   * **Explanation:** `See` [AUTHORIZATION REQUEST REST DTO](#authorization-request-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`


#### **Success Response:**

  * **Code:** 200
  * **Sample Content:** [AUTHORIZATION RESPONSE REST DTO](authorization-response-rest-dto)
```
  {
      "code": "0",
      "message": "authorized",
      "reason": "admin"
  }
```
  
  * **Explanation:** `See` [AUTHORIZATION RESPONSE REST DTO](#authorization-response-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

#### **Error Response:**

  * **Code:** Other than 200 for failed response. 
  * **Content:**
```
{
    "code":"error code here",
    "message":"error message here",
    "reason":"error reason here"
}
```

---

---
#### **Optional**
NOTE: **Required** if partner needs to store booking details of each booking.
#### **3. Purpose**

  _Acknowledgement of a completed action by a user_

#### **URL**

  `As chosen by partner`
  
**Recommended:**

_VENDOR_BASE_URL/itinerary/vendor/acknowledge_

**Example:**  

- Vendor : googol 
- User access token: AY132A871234
- Transaction Id: jkas3204-asljkd123-asdkj12-alksjd134
- Api Key(Optional): 2766491asd723

 Example Url: https://www.googol.com/itinerary/vendor/acknowledge?uat=AY132A871234&txnId=jkas3204-asljkd123-asdkj12-alksjd134&apiKey=2766491asd723
 
#### **Method:**
  
  _POST_
  
####  **URL Params**
 
  **Required:**

   `uat : Partner generated user token number` _Should have one to one mapping with a unique user_

   `txnId : Partner generated transaction ID` _Should have one to one mapping with each transaction and login session_

  **Optional:**

   `apiKey : The provided api key by partner to Routofy to authenticate API requests`

####  **Headers**

   `Content-Type = application/json`

   `Accept = application/json`

#### **Data Params**

   * **Required Post Body:** [ACKNOWLEDGEMENT REQUEST REST DTO](#acknowledgement-request-rest-dto)
    
   ```
   {
       "amount":2000,
       "status":"book"
   }
   ```

   * **Explanation:** `See` [ACKNOWLEDGEMENT REQUEST REST DTO](#acknowledgement-request-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`


#### **Success Response:** 

  * **Code:** 200
  * **Sample Content:** [ACKNOWLEDGEMENT RESPONSE REST DTO](#acknowledgement-response-rest-dto)
```
{
    "code": "0",
    "message": "acknowledged"
}
```
  
  * **Explanation:** `See` [ACKNOWLEDGEMENT RESPONSE REST DTO](#acknowledgement-response-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

 
#### **Error Response:**

`Not required`

---

---
### **APIs TO BE CONSUMED BY PARTNERS**

#### **Optional**
#### **1. Purpose**

  _Fetching previous booking for a specific transaction ID_

#### **URL**

  _/api/v2/itinerary/vendor/retrieve_

#### **Method:**
  
  _GET_
  
####  **URL Params**
 
  **Required:**
 
   `vendorKey : The provided vendor key for partner` _Routofy will provide this to partner_

   `tokenNo : Partner generated user token number`

   `txnId : Partner generated transaction ID` _Should be unique for each transaction and login session_

####  **Headers**

   `Content-Type = application/json`

   `Accept = application/json`

#### **Data Params**

   `Nothing`

#### **Success Response:**

  * **Code:** 200
  * **Sample Content:** [BOOKING DETAIL REST DTO](#booking-detail-rest-dto)
```
{
  "brn": "16AA1001161771",
  "bd": 1465394572381,
  "bs": "BOOKED",
  "pr": 812,
  "jsdt": 1465488300000,
  "jedt": 1465510800000,
  "jscn": "Jaipur",
  "jdcn": "New Delhi",
  "jssn": "Jaipur",
  "jdsn": "New Delhi",
  "edl": [
    {
      "type": "BusBookingDetailRestDto",
      "vdr": "1",
      "bs": "CONFIRMED",
      "rbs": "",
      "cindex": 1,
      "oci": {
        "pr": 842,
        "m": "b",
        "du": 22500000,
        "ds": 262050,
        "st": 1465488300000,
        "et": 1465510800000,
        "dd": 0,
        "ad": 3600000,
        "bd": 1800000,
        "dbd": 900000,
        "wt": 900000,
        "sc": "Mateshwari travels sindhi camp bus stand ",
        "dc": "Filmistan",
        "sfr": "Mateshwari travels sindhi camp bus stand ",
        "sto": "Filmistan",
        "fr": "Jaipur",
        "to": "New Delhi",
        "slat": 26.912433624267578,
        "slong": 75.78726959228516,
        "dlat": 28.632244110107422,
        "dlong": 77.2207260131836,
        "dtzoff": 19800000,
        "atzoff": 19800000,
        "dtzid": "Asia/Kolkata",
        "atzid": "Asia/Kolkata"
      },
      "pl": [
        {
          "age": 25,
          "id": 198,
          "name": "Prateek Grover",
          "sn": "S",
          "ic": false,
          "fpnr": null,
          "tno": null,
          "on": true
        }
      ],
      "url": "ticket/16AA1001161771/1",
      "ioe": true,
      "cn": "Rishabh     Travels",
      "bpn": "Mateshwari travels sindhi camp bus stand ",
      "bt": "Non A/C Seater/Sleeper (2+1)",
      "pa": "Banagala Bazar Chauraha, Paani Ki Tanki,ashiyana, Near Pwd Colony, Lucknow",
      "pci": "9718832761/9718832708/9250516432/9213065147//9891323316",
      "ploc": "Saheed Path",
      "plm": "Eco Park",
      "pt": "Thu, Jun 09, 09:10 PM",
      "fb": {
        "Base Fare": 800,
        "Other Taxes": 42
      }
    }
  ],
  "dtzoff": 19800000,
  "atzoff": 19800000,
  "bemail": "prateekgrover.rns@gmail.com",
  "dtzid": "Asia/Kolkata",
  "atzid": "Asia/Kolkata",
  "isint": false,
  "isrt": false
}
```
  * **Explanation:** `See` [BOOKING DETAIL REST DTO](#booking-detail-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

 
#### **Error Response:**

  `See` [Routofy common error responses](#routofy-common-error-responses) `section`

---

---
#### **Optional**
#### **2. Purpose**

  _Cancellation Summary_

#### **URL**

  _/api/v2/itinerary/vendor/cancel/summary_

#### **Method:**
  
  _POST_
  
####  **URL Params**
 
**Required:**

  `vendorKey : The provided vendor key for partner` _Routofy will provide this to partner_

  `uat : partner generated user token number`

  `txnId : partner generated transaction ID for each transaction` _Should be unique for each transaction_

####  **Headers**
   
   `Content-Type = application/json`

   `Accept = application/json`

#### **Data Params**

  * **Required Post Body:** [ROUTE CANCELLATION REQUEST REST DTO](#route-cancellation-request-rest-dto)
```
{
    "brn":"dfvdfvdfv",
    "ecl":[
        {
            "co":123,
            "pdl":[
                123,
                456,
                678,
                987
            ]
        }
    ],
    "mnl":false
}
```
  * **Explantion:** `See` [ROUTE CANCELLATION REQUEST REST DTO](#route-cancellation-request-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

#### **Success Response:**

  * **Code:** 200
  * **Sample Content:** [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto)
```
{
    "cs":{
          "1":"Confirmed"
      },
    "ecl":[
        {
            "co": 1,
            "tf": 2220,
            "ra": 0,
            "sc": 2220
        }
    ]
}
```

 * **Explantion:** `See` [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

 
#### **Error Response:**

  * **Code:** 200 OK
  * **Content:** [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto)
```
{
    "cs":{
        "1":"Cancelled"
    },
    "ecl":[
        {
            "co": 1,
            "tf": 2220,
            "ra": -1, //refund amount has -1
            "sc": 0
        }
    ]
}
```
  * **Possible reason:** `Cancellation information not provided by vendor. Offline cancellation request taken.`
  * **Resolution:** `Send the same request again with 'mnl' as true and we will take your request for refund amount offline.`

  OR

  * **Code:** 200 OK
  * **Content:** [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto)
```
{
    "cs":{
        "1":"Cancelled"
    },
    "ecl":[
        {
            "co": 1,
            "tf": 2220,
            "ra": -1, //refund amount has -1
            "sc": -1 //service charge has -1
        }
    ]
}
```
  * **Possible reason:** `Unhandled error`
  * **Resolution:** `Mail at support@routofy.com or call at 011-65669966 for assistance.`

  `See` [Routofy common error responses](#routofy-common-error-responses) `section`

---

---
#### **Optional**
#### **3. Purpose**

  _Cancellation of Booking_

#### **URL**

  _/api/v1/itinerary/vendor/cancel_

#### **Method:**
  
  _POST_
  
####  **URL Params**
 
   **Required:**
 
   `vendorKey : The provided vendor key for partner` _Routofy will provide this to partner_

   `uat : Partner generated user token number`

   `txnId : Partner generated transaction ID` _Should be unique for each transaction and login session_

####  **Headers**
   
   `Content-Type = application/json`

   `Accept = application/json`

#### **Data Params**

  * **Required Post Body:** [ROUTE CANCELLATION REQUEST REST DTO](#route-cancellation-request-rest-dto)
```
{
    "brn":"dfvdfvdfv",
    "ecl":
        [
            {
                "co":123,
                "pdl":[
                    123,
                    456,
                    678,
                    987
                ]
            }
        ],
    "mnl":false
}
```
  * **Explanation:** `See` [ROUTE CANCELLATION REQUEST REST DTO](#route-cancellation-request-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

#### **Success Response:**

  * **Code:** 200
  * **Sample Content:** [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto)
```
{
    "cs":{
        "1":"Cancelled"
     },
     "ecl":[
         {
             "co": 1,"
             tf": 2220,"
             ra": 0,
             "sc": 2220
         }
     ]
 }
```
* **Explanation:** `See` [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto) `explanation in` [DTO explanation](#dto-explanation) `section for details.`

#### **Error Response:**

  * **Code:** 200 OK
    **Content:** [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto)
```
{
    "cs":{
        "1":"Cancelled"
    },
    "ecl":[
        {
            "co": 1,
            "tf": 2220,
            "ra": -1, //refund amount has -1
            "sc": 0
        }
    ]
}
```
    
**Possible reason:** `Cancellation information not provided by vendor. Offline cancellation request taken.`

  OR

  * **Code:** 200 OK
    **Content:** [ROUTE CANCELLATION RESPONSE REST DTO](#route-cancellation-response-rest-dto)
```
{
    "cs":{
        "1":"Cancelled"
    },
    "ecl":[
        {
            "co": 1,
            "tf": 2220,
            "ra": -1, //refund amount has -1
            "sc": -1 //service charge has -1
        }
    ]
}
```
    
 **Possible reason:** `Unhandled error`
 **Resolution:** `Mail at support@routofy.com or call at 011-65669966 for assistance.`

  `See` [Routofy common error responses](#routofy-common-error-responses) `section`

---
#### **Partner Common Error Responses**

* **Format**
  * **Code:** Other than 200 for failed response. 
  * **Content:**
```
{
    "code":"error code here",
    "message":"error message here"
}
```

---

---
#### **Routofy Common Error Responses**

  * **Code:** 400 Bad Request
  * **Content:** 
  ```
  {
      "code":"40",
      "message":"Vendor key is null or empty"
  }
  ```

  OR

  * **Code:** 400 Bad Request
  * **Content:**
  ```
  {
      "code":"41",
      "message":"User access token is null or empty"
  }
  ```

  OR

  * **Code:** 400 Bad Request
  * **Content:**
  ```
  {
      "code":"42",
      "message":"Transacion ID is null or empty"
  }
  ```

  OR

  * **Code:** 401 Unauthorized
  * **Content:**
  ```
  {
      "code":"43",
      "message":"Invalid vendor key"
  }
  ```

  OR

  * **Code:** 401 Unauthorized
  * **Content:**
  ```
  {
      "code":"44",
      "message":"Invalid user access token"
  }
  ```

  OR

  * **Code:** 500 Internal server error
  * **Content:**
```
  {
      "code":"50",
      "message":"Could not find bookings for the specified combination of transaction ID, user access token and Vendor Key"
  }
```
  
  * **Possible reasons:** `Transaction ID and User Access Token combination might be wrong`

  OR

  * **Code:** 500 Internal server error
  * **Content:**
```
  {
      "code":"51",
      "message":"Multiple bookings found for the specified combination of transaction ID, user access token and Vendor Key"
  }
```
  
  * **Possible reasons:** `Duplicate Transaction ID and User Access Token combination was used for booking` 

  OR

  * **Code:** 500 Internal server error
  * **Content:**
```
  {
      "code":"500",
      "message":"Your request could not be processed"
  }
```
  
  * **Possible reason:** `Unhandled situation/error` 

---

---
#### **DTO Explanation**

* **Authentication**
    * #####TRAVEL GROUP REST DTO
        * "fn" - `Booker first name`
        * "ln" - `Booker last name`
        * "em" - `Booker email ID`
        * "mb" - `Booker mobile number`
        * "psgrs" - `List<`[PassengerDetailRestDto](#passenger-detail-rest-dto)`> passengerRestDtoList` _List of all the passengers for whom ticket will be booked_
        
        NOTE: If travelling passengers' details are not known, send an empty JSON list.
    
    * #####PASSENGER DETAIL REST DTO
        * "fn":`Passenger first name`
        * "ln":`Passenger last name`
        * "em":`Passenger email ID`
        * "mobNo":`Passenger mobile number`
        * "sx": `Passenger gender`
        * "dob":`Passenger date of birth in epoch milliseconds`
        * "pp":`Passenger passport number` _Required in case of international booking otherwise empty string_
        * "pi":`Passenger passport issue date in epoch milliseconds`  _Required in case of international booking otherwise 0_
        * "pe":`Passenger passport expiry date in epoch milliseconds`  _Required in case of international booking otherwise 0_
        * "pc":`Passenger passport issue country`  _Required in case of international booking otherwise empty string_

* **Authorization**
    * #####AUTHORIZATION REQUEST REST DTO
        * "amount" - `The amount related to the action if applicable`
        * "action" - `The action to be authorized` _Possible values: "book", "cancel"_
    
    * #####AUTHORIZATION RESPONSE REST DTO
        * "code":`Success/Error code string` _None zero value is error_
        * "message":`Success/Error message`
        * "remarks":`Any other relevant information`

* **ACKNOWLEDGEMENT**
    * #####ACKNOWLEDGEMENT REQUEST REST DTO
        * "amount" - `The amount related to action if applicable`
        * "action" - `The action performed` _Possible values: "book", "cancel"_
    
    * #####ACKNOWLEDGEMENT RESPONSE REST DTO
        * "code":`Success code string` _"0" is success code string_
        * "message":`Any relevant information`
      
* **Previous Booking**
  
    * #####BOOKING DETAIL REST DTO:

        * "brn" - `String bookingRefNo`
        * "bd" - `Long bookingDate`
        * "bs" - `String bookingStatus`
        * "pr" - `double price`
        * "jsdt" - `Long journeyStartDateAndTime`
        * "jedt" - `Long journeyEndDateAndTime`
        * "jscn" - `String journeySourceCityName`
        * "jdcn" - `String journeyDestinationCityName`
        * "jssn" - `String journeySourceStopName`
        * "jdsn" - `String journeyDestinationStopName`
        * "edl" - `List<`[EdgeBookingDetailRestDto](#edge-booking-detail-rest-dto)`> edgeBookingDetailRestDtoList`
        * "dtzoff" - `Long departureTimeZoneOffset` _Time Zone Offset is the offset from GMT_
        * "atzoff" - `Long arrivalTimeZoneOffset`
        * "bemail" - `String bookerEmailId`
        * "dtzid" - `String departureTimeZoneId` _Time Zone ID is like "Asia/Kolkata"_
        * "atzid" - `String arrivalTimeZoneId`
        * "isint" - `boolean international`
        * "isrt" - `boolean roundTrip`

    * #####EDGE BOOKING DETAIL REST DTO:

        * "vdr" - `String vendor` _Not needed but still save_
        * "bs" - `String bookingStatus`
        * "rbs" - `String returnBookingStatus` _Booking status for return journey_
        * "cindex" - `int componentIndex` _It is a randomly generated integer for every edge including onward journey and return journey_
        * "oci" - [ComponentInfoRestDto](#component-info-rest-dto) `onwardComponentInfoRestDto` _Detail about onward journey components_
        * "rci" - [ComponentInfoRestDto](#component-info-rest-dto)  `returnComponentInfoRestDto` _Detail about return journey components_
        * "pl" - `List<`[PassengerDetailRestDto](#passenger-detail-rest-dto)`> passengerDetailRestDtoList` _Detail about passengers_
        * "url" - `String ticketUrl`
        * "pnr" - `String pnr`
        * "ioe" - `boolean onwardEdge` _Boolean to determine if it is an onward edge_

    * #####COMPONENT INFO REST DTO

        * "pr" - `Double price`
        * "m" - `String travelMode` _f - flight, b - bus, c - cab, t - train, o - other_
        * "du" - `Long travelDuration`
        * "ds" - `int distance`
        * "st" - `Long departureTime`
        * "et" - `Long arrivalTime`
        * "dd" - `Long delayInDepartureTime`
        * "ad" - `Long delayInArrivalTime`
        * "bd" - `Long boardingDuration`
        * "dbd" - `Long deBoardingDuration`
        * "wt" - `Long waitingTime`
        * "sc" - `String sourceStopCode` _Example: DEL for Delhi_
        * "dc" - `String destinationStopCode`
        * "sfr" - `String sourceStopName`
        * "sto" - `String destinationStopName`
        * "fr" - `String sourceCityName`
        * "to" - `String destinationCityName`
        * "slat" - `Double sourceStopLatitude`
        * "slong" - `Double sourceStopLongitude`
        * "dlat" - `Double destinationStopLatitude`
        * "dlong" - `Double destinationStopLongitude`
        * "dtzoff" - `Long departureTimeZoneOffset`
        * "atzoff" - `Long arrivalTimeZoneOffset`
        * "dtzid" - `String departureTimeZoneId`
        * "atzid" - `String arrivalTimeZoneId`

    * #####CAB BOOKING DETAIL REST DTO inherits EDGE BOOKING DETAIL REST DTO

        * "nod" - `int numberOfDays` _Not needed_
        * "vdl" - `List<`[VehicleDetailRestDto](#vehicle-detail-rest-dto)`> vehicleDetailRestDtoList` _Detail about vehicle_
        * "out" - `int travelType` _Not needed_
        * "fb" - `Map<String, Double> fareBreakUp` _Fare break up desription and value_

    * #####VEHICLE DETAIL REST DTO

        * "dn" - `String driverName`
        * "dp" - `String driverPhone`
        * "vn" - `String vehicleName`
        * "vs" - `int vehicleSeater` _Number of seats in vehicle_
        * "vnum" - `String vehicleNumber`

    * #####BUS BOOKING DETAIL REST DTO inherits EDGE BOOKING DETAIL REST DTO

        * "cn" - `String carrierNam`e _Which company bus_
        * "bpn" - `String boardingPointName` _Where to board from_
        * "bt" - `String busType` _Type of bus_
        * "pa" - `String pickupAddress`
        * "pci" - `String pickupContactInfo`
        * "ploc" - `String pickupLocation`
        * "plm" - `String pickupLandMark`
        * "pt" - `String pickupTime`
        * "fb" - `Map<String, Double> fareBreakUp` _Same as cab_

    * #####FLIGHT BOOKING DETAIL REST DTO inherits EDGE BOOKING DETAILS REST DTO

        * "sac" - `String sourceAirportCode` _Example : DXB for Dubai Airport_
        * "scit" - `String sourceAirportCity`
        * "fc" - `String flighpartnerass`
        * "ft" - `String fareType` //Refundable or non-refundable
        * "dac" - `String destinationAirportCode` _Same as above_
        * "dcit" - `String destinationAirportCity`
        * "adl" - `int adults`
        * "cld" - `int children`
        * "inf" - `int infants`
        * "sterm" - `String startTerminal` _Which terminal of the airport_
        * "eterm" - `String endTerminal`
        * "fb" - `Map<String, Double> fareBreakUp` _Same as bus_
        * "flc" - `FlightTravelClassEnum flightTravelClassEnum` _E - Economy, F - First Class, P - Premium Economy, B - Business_
        * "an" - `String airlineName`
        * "iin" - `boolean isInternational`
        * "oc" - `List<`[BookedFlightJourneyComponentRestDto](#booked-flight-journey-component-rest-dto)`> onwardComponentRestDtoList`
        * "rc" - `List<`[BookedFlightJourneyComponentRestDto](#booked-flight-journey-component-rest-dto)`> returnComponentRestDtoList`
        * "tn" - `List<String> telephoneNo`

    * #####BOOKED FLIGHT JOURNEY COMPONENT REST DTO

        * "dac" - `String departureAirport`
        * "dcit" - `String departureAirportCity`
        * "aac" - `String arrivalAirport`
        * "acit" - `String arrivalAirportCity`
        * "fn" - `String flightNumber`
        * "alc" - `String airlineCode`
        * "aln" - `String airlineName`
        * "dt" - `Long departureTime`
        * "at" - `Long arrivalTime`
        * "sterm" - `String startTerminal`
        * "eterm" - `String endTerminal`
        * "cib" - `String checkInBaggage`
        * "cb" - `String cabinBaggage`
        * "sd" - `List<`[FlightStopDetailsInfoRestDto](#flight-stop-details-info-rest-dto)`> stopDetails`
        * "dtzoff" - `Long departureTimeZoneOffset`
        * "atzoff" - `Long arrivalTimeZoneOffset`
        * "logo" - `String airlineLogo`
        * "dtzid" - `String departureTimeZoneId`
        * "atzid" - `String arrivalTimeZoneId`

    * #####FLIGHT STOP DETAILS INFO REST DTO _for connected flights_

        * "in" - `BigInteger stopIndex` _which number of stop_
        * "lac" - `String layoverAirport`
        * "dt" - `Long departureDateTime`
        * "at" - `Long arrivalDateTime`
        * "ld" - `BigInteger layoverDuration`
        * "dtzoff" - `Long departureTimeZoneOffset`
        * "atzoff" - `Long arrivalTimeZoneOffset`
        * "dtzid" - `String departureTimeZoneId`
        * "atzid" - `String arrivalTimeZoneId`

* **Cancellation Summary & Cancellation**
  
    * #####ROUTE CANCELLATION REQUEST REST DTO:
    
        * "brn" - `String Booking reference number`
        * "ecl" - `List<`[EdgeCancellationRequestRestDto](#edge-cancellation-request-rest-dto)`> edgeCancellationRequestRestList`
        * "mnl" - `isManualIntervention`
    
    * #####EDGE CANCELLATION REQUEST REST DTO
        * "co" - `component index` _can be found in previous booking response_
        * "pdl" - `passenger id list` _can be found in previous booking response_

    * #####ROUTE CANCELLATION RESPONSE REST DTO:
    
        * "cs" - `Map<String,String> componentStatus` _component index string as key with its status as value._
        * "ecl" - `List<`[EdgeCancellationResponseRestDto](#edge-cancellation-response-rest-dto)`> edgeCancellationResponseRestDtoList`

    * #####EDGE CANCELLATION RESPONSE REST DTO:
    
        * "co" - `int componentIndex`
        * "tf" - `Double totalFare`
        * "ra" - `Double refund amount`
        * "sc" - `Double serviceCharge`
