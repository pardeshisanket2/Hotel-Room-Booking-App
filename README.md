# Hotel-Room-Booking-App
Backend project 

We are breaking the Hotel room booking application into three different microservices, which are as follows:

API-Gateway - This service is exposed to the outer world and is responsible for routing all requests to the microservices internally.
Booking service - This service is responsible for collecting all information related to user booking and sending a confirmation message once the booking is confirmed.
Payment service - This is a dummy payment service; this service is called by the booking service for initiating payment after confirming rooms.
 
Part 1 - Rationale behind the architecture of the application
The main reason for breaking the application into different microservices is to follow the single responsibility principle, where each service is responsible for its own single service, which is considered one of the best industry practices. Let’s try to understand this with the following scenario:


Suppose we are taking only UPI payment for now, and later, we find the necessity to include credit card/debit card payment as well; then, we have to update only the payment service keeping the remaining services untouched.


Application workflow
Let’s now try to understand the workflow of the application using the following architecture diagram.
Note: The implementation details of the workflow will be discussed later in the problem statement.

Application Workflow
Application Workflow
Initially, the API Gateway, Booking service and Payment service register themselves on the Eureka server. Note that the user does not directly interact with Booking or Payment service. All requests are sent to API Gateway which then sends the requests to the relevant microservice.
The user initiates room booking using the 'Booking' service by providing information such as toDate, fromDate, aadharNumber and number of rooms required (numOfRooms).
The 'Booking' service returns back the list of room numbers and price associated and prompts the user to enter the payment details if they wish to continue ahead with the booking. It also stores the details provided by the user in its database. 
If the user wants to go ahead with the booking, then they can simply provide the payment related details like bookingMode, upiId/ cardNumber to the 'Booking' service which will be further sent to the 'Payment' service. Based on this data, the payment service will perform a dummy transaction and return back the transaction Id associated with the transaction to the Booking service. All the information related to transactions is saved in the database of the payment service.
Once the transaction completes, booking is confirmed and therefore Booking service sends a confirmation message on the console.

Part 2 - Rationale behind the communication of the application
The rationale behind choosing synchronous communication (REST) between the Booking and Payment services is that the response('transactionId') is required before confirming the booking and sending a message.
