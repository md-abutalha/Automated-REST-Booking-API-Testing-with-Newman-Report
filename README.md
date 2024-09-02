
### Automated REST Booking API Testing with Newman Report

This project showcases API testing with Postman, featuring a comprehensive set of tests to ensure the accuracy and reliability of different API endpoints.




### **Features**

- Tests for GET, POST, PUT, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations


## API Documentation:
https://documenter.getpostman.com/view/37041522/2sAXjM5C3J



### **Technology used:**

- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library




## Installation

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:

```bash
 git clone https://github.com/md-abutalha/Automated-REST-Booking-API-Testing-with-Newman-Report.git
```
3. Import the Postman collection:
- Open Postman.
- Click on the Import button.
- Select the file from the repository.
4. Import the Postman environment:
- In Postman, click on the gear icon in the top right corner.
- Select **Import** and choose the file.
5. Newman and Report Installation Process:
- Newman Install Command:  
```bash
 npm install -g newman
```
- Newman Html Report Install Command:
```bash
 npm install -g newman-reporter-htmlextra
```

### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
 -   Once the tests are complete, view the results in the Runner tab.
- Detailed test results can be viewed for each request.

## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:

```javascript
//firstName
var firstName = pm.variables.replaceIn("{{$randomFirstName}}");
// console.log(firstName);
pm.environment.set("firstName", firstName)

//last name
var lastName = pm.variables.replaceIn("{{$randomLastName}}");
// console.log(lastName)
pm.environment.set("lastName", lastName);

//Price
var price = pm.variables.replaceIn("{{$randomInt}}");
pm.environment.set("totalPrice", price);

//Deposite ID

var depositeID = pm.variables.replaceIn("{{$randomBoolean}}");
pm.environment.set("deposite_id", depositeID);

//Additional Needs

var additionalNeed = pm.variables.replaceIn("{{$randomWord}}");
pm.environment.set("additionalNeed", "Bread " + additionalNeed);


//check_In_date
const moment = require('moment');
const today= moment();
console.log(today.format("YYYY-MM-DD HH:mm:ss"));
pm.environment.set("date",today.format("YYYY-MM-DD"));

//Checkout_Date
const date1 = require('moment');
const future = date1();
var check_out = future.add(5, 'd').format("YYYY-MM-DD");
// console.log(future.add(5, 'd').add(2,'M').format("YYYY-MM-DD"));
pm.environment.set("future_date", check_out);
// console.log(future.subtract(5, 'd').format("YYYY-MM-DD"))

```
 **Request Body:** 
```javascript
{
	"firstname" : "{{firstName}}",
	"lastname" : "{{lastName}}",
	"totalprice" : {{totalPrice}},
	"depositpaid" : {{deposite_id}},
	"bookingdates" : {
    	"checkin" : "{{date}}",
    	"checkout" : "{{future_date}}"
	},
	"additionalneeds" : "{{additionalNeed}}"
}

```
 **Response Body:**
```javascript
{
    "bookingid": 629,
    "booking": {
        "firstname": "Cameron",
        "lastname": "Mueller",
        "totalprice": 987,
        "depositpaid": false,
        "bookingdates": {
            "checkin": "2024-09-02",
            "checkout": "2024-09-07"
        },
        "additionalneeds": "Bread withdrawal"
    }
}
```
## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Response Body:
```javascript
{
    "firstname": "Ima",
    "lastname": "Effertz",
    "totalprice": 430,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-09-02",
        "checkout": "2024-09-07"
    },
    "additionalneeds": "Bread override"
}
```
### Test Script:
```javascript
pm.test("200 Status code check", function () {
     pm.response.to.have.status(200);
});

// var jsonData = pm.response.json();
// pm.test("firstName Validate", function () {
//      pm.expect(pm.response.json()).to.have.property('firstname').that.is.a('string').and.not.empty;
// });


// pm.test("LastName Validate", function (){
//      var responseBody = pm.response.json();
//      pm.expect(responseBody).to.be.an('object');
//      pm.expect(responseBody).to.have.property('lastname').that.is.a('string').and.not.empty;
// });

// first name
var jsonData = pm.response.json();
pm.test("Validate FirstName", function () {
    pm.expect(pm.environment.get("firstName")).to.eql(jsonData.firstname);
});
//last name
pm.test("LastName Validate", function(){
    pm.expect(pm.environment.get("lastName")).to.eql(jsonData.lastname);
});

//total price 
pm.test("Verify Total Price", function (){
    var totalPrice = parseFloat(pm.environment.get("totalPrice"));  // Convert string to number
    pm.expect(totalPrice).to.eql(jsonData.totalprice);
});

//depositpaid
pm.test("Verify depositpaid", function () {
    var depositPaid = pm.environment.get("deposite_id") === 'true';  // Convert string to boolean
    pm.expect(depositPaid).to.eql(jsonData.depositpaid);
});

// Date 
var jsonData = pm.response.json();
pm.test("Checkin Date Validate", function () {
    pm.expect(pm.environment.get("date")).to.eql(jsonData.bookingdates.checkin);
});

pm.test("CheckOut Date", function() {
    pm.expect(pm.environment.get("future_date")).to.eql(jsonData.bookingdates.checkout);
});

// check additionalneeds

pm.test(" Verify additionalneeds", function() {
    pm.expect(pm.environment.get("additionalNeed")).to.eql(jsonData.additionalneeds);
});

```

## _**3. Create A Token For Authentication.**_
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Request Body:
```javascript
{
	"username": "admin",
	"password": "password123"
}
```
 **Response Body:**
```javascript
"token": "06eb798bf6f2caa"
```
## _**4. Update the Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: PUT
### Pre-request Script:
```javascript
//firstName
var firstName = pm.variables.replaceIn("{{$randomFirstName}}");
// console.log(firstName);
pm.environment.set("firstName1", firstName)

//last name
var lastName = pm.variables.replaceIn("{{$randomLastName}}");
// console.log(lastName)
pm.environment.set("lastName1", lastName);

//Price
var price = pm.variables.replaceIn("{{$randomInt}}");
pm.environment.set("totalPrice1", price);

//Deposite ID

var depositeID = pm.variables.replaceIn("{{$randomBoolean}}");
pm.environment.set("deposite_id1", depositeID);

//Additional Needs

var additionalNeed = pm.variables.replaceIn("{{$randomWord}}");
pm.environment.set("additionalNeed1", "Bread " + additionalNeed);


//check_In_date
const moment = require('moment');
const today= moment();
console.log(today.format("YYYY-MM-DD HH:mm:ss"));
pm.environment.set("date1",today.format("YYYY-MM-DD"));

//Checkout_Date
const date1 = require('moment');
const future = date1();
var check_out = future.add(6, 'y').format("YYYY-MM-DD");
pm.environment.set("future_date1", check_out);

```
  **Request Body:** 
```javascript
{
	"firstname" : "{{firstName1}}",
	"lastname" : "{{lastName1}}",
	"totalprice" : {{totalPrice1}},
	"depositpaid" : {{deposite_id1}},
	"bookingdates" : {
    	"checkin" : "{{date1}}",
    	"checkout" : "{{future_date1}}"
	},
	"additionalneeds" : "{{additionalNeed1}}"
}

```
  **Response Body:**
```javascript
{
    "firstname": "Otha",
    "lastname": "Bauch",
    "totalprice": 824,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-09-02",
        "checkout": "2030-09-02"
    },
    "additionalneeds": "Bread Toys"
}
```
 ## _**5. Delete Booking Record**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: DELETE
### Response Body: None 

## Run Command:  
- Run Command for Console: 
```javascript
newman run Booking.postman_collection.json -e Booking.postman_environment.json
```
- Run Command for Report: 
```javascript
newman run Booking.postman_collection.json -e Booking.postman_environment.json -r cli,htmlextra
```
## Newman Report Summary:
![Newman Report Summary-1](https://github.com/user-attachments/assets/8c5b6525-736f-4a56-b90e-1258bdf21da0)
![Newman Report Summary-2](https://github.com/user-attachments/assets/485fcf47-0ddf-48a8-a121-1abbcf947b43)
![Newman Report Summary-3](https://github.com/user-attachments/assets/6ec9e3a0-ad31-44df-88f4-13048567f2cf)
![Newman Report Summary-4](https://github.com/user-attachments/assets/7801249c-d566-49a2-985e-a7673a1fe2ca)


## Authors

- [@abutalha](https://github.com/md-abutalha)


## 🔗 Links
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://github.com/md-abutalha)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/abu-talha1/)
[![twitter](https://img.shields.io/badge/twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://x.com/abu_talha0x)

