# Metro Ticket Booking System in ServiceNow

## Overview

The **Metro Ticket Booking System** is a ServiceNow-based solution built for the Hyderabad Metro Rail that digitizes and automates the entire metro ticket booking process. Passengers can book a QR-code-based digital ticket or recharge their metro smart card directly through a **Service Catalog** item, with fare calculation, request generation, database logging, QR code generation, and email confirmation all handled automatically within the ServiceNow platform — eliminating long queues, manual fare calculation, and physical ticket issuance.

## Project Objectives

- Digitize and automate metro ticket booking to eliminate long queues and manual fare calculation
- Provide a self-service Service Catalog interface for booking QR tickets and recharging metro smart cards
- Automatically calculate fares based on source/destination stations, journey type, and passenger count
- Generate a digital, QR-code-based ticket for contactless verification at metro gates
- Instantly notify passengers of their booking confirmation and ticket details via email
- Maintain a centralized, queryable record of every booking for reporting and administration

## Key Features

- **Service Catalog Booking Form ("Book A Metro Ticket"):** Single catalog item offering two options — *Book QR Ticket* and *Recharge Metro Card*
- **Metro Ticket Booking:** Passengers select starting/destination stations, travel date, journey type (Single/Return), and number of passengers
- **Metro Card Recharge:** Passengers can recharge their smart card by entering card holder name, card number, and recharge amount
- **Multiple Payment Modes:** UPI, Credit Card, Debit Card, and Other
- **Automated Flow (Flow Designer):** A "Metro Ticket Booking Flow" triggers on catalog submission, retrieves catalog variables, and creates a record in the custom Metro Database table
- **Digital Ticket & QR Code Generation:** A unique Ticket ID (e.g. `MTCA776F43`) is generated and a scannable QR code is created via a QR code API and attached to the confirmation email
- **Automated Email Notifications:** Passengers receive a "Your Hyderabad Metro Ticket" email with journey details, amount, and the QR code for boarding
- **Centralized Metro Database:** A custom ServiceNow table (`Metro Databases`) stores every ticket/recharge transaction — Ticket ID, Passenger, Email, Travel Date, Journey Type, Stations, Payment Mode, Amount, Passenger Count, and QR Code URL
- **Request Tracking:** Every booking generates a standard ServiceNow Request (REQ) and Requested Item (RITM) for full audit and status tracking
- **Scalable Cloud Platform:** Built entirely on ServiceNow's SaaS architecture, ready to scale to more stations, routes, and users

## Technology Stack

| Layer | Technology |
|---|---|
| Platform | ServiceNow (Service Catalog, Cloud Platform) |
| User Interface | Service Portal, Service Catalog Item |
| Client-Side Logic | Catalog Client Scripts (JavaScript) |
| Server-Side Logic | Script Includes, GlideAjax |
| Automation | Flow Designer, Business Rules |
| Database | ServiceNow Tables (custom Metro Database table) |
| Notifications | ServiceNow Notification Engine (Email/SMTP) |
| Ticketing | QR Code Generator API |
| Reporting | ServiceNow Reports & Dashboards |

## Project Phases

1. **Ideation Phase** — Brainstorming, problem statement, and empathy mapping for metro commuters
2. **Project Design Phase** — Solution fit, proposed solution, solution requirements, and solution architecture
3. **Project Planning Phase** — Product backlog, sprint planning, and story point estimation
4. **Project Development Phase** — Building the Service Catalog item, Flow, database table, and notifications
5. **Acceptance Testing Phase** — UAT execution, defect analysis, and test case reporting

## Installation & Setup

### Prerequisites

- A ServiceNow developer or PDI instance (Washington DC release or later recommended)
- Administrative access to the ServiceNow instance
- Basic familiarity with Service Catalog, Flow Designer, and ServiceNow tables

### Deployment Steps

1. **Access your ServiceNow instance**
   - Navigate to: `https://<your-instance>.service-now.com`

2. **Import the application**
   - Go to **System Applications > Studio** (or **Update Sets > Retrieved Update Sets**)
   - Import the Metro Ticket Booking application/update set

3. **Configure the Service Catalog**
   - Verify the **"Book A Metro Ticket"** catalog item is active under the Service Catalog
   - Confirm catalog variables: *What do you want to do Today?*, *Passenger Name*, *Email Address*, *Travel Date*, *Startingg From*, *Goingg To*, *Journey Type*, *No Of Passengers*, *Smart Card Holder Name*, *Smart Card Number*, *Recharge Amount*, *Mode Of Payment*

4. **Activate the Flow**
   - Open **Flow Designer** and confirm the **"Metro Ticket Booking Flow"** is Active
   - This flow triggers on Service Catalog submission, retrieves catalog variables, and creates a **Metro Database** record

5. **Configure notifications**
   - Verify the **"Metro Ticket Confirmation"** email notification is active and linked to the `notification_engine.process` event
   - Confirm SMTP settings are configured for outbound email

6. **Test the system**
   - Submit a test booking through the catalog item
   - Verify a REQ/RITM is generated
   - Confirm a record appears in the Metro Database table with a QR Code URL
   - Confirm the confirmation email is received with ticket details and QR code

## Usage

### Booking a Metro Ticket

1. Search for or navigate to **"Book A Metro Ticket"** in the Service Catalog
2. Select **Book QR Ticket**
3. Enter Passenger Name, Email Address, Travel Date, Starting From, Going To stations, Journey Type, and Number of Passengers
4. Review the auto-calculated fare (Amount for Single Journey / Amount Including Return)
5. Click **Order Now**
6. Receive the Request Summary (REQ) and track it under **My Requests**
7. Receive an email with the Ticket ID and QR code for boarding

### Recharging a Metro Card

1. Navigate to **"Book A Metro Ticket"** and select **Recharge Metro Card**
2. Enter Smart Card Holder Name, Smart Card Number, and Recharge Amount
3. Select Mode of Payment (UPI/Credit Card/Debit Card/Other)
4. Enter Email Address and click **Order Now**

### Tracking a Request

- Open **Requests > My Requests** to view the Request Summary
- Open the **RITM** and go to the **Additional Details** tab to view all submitted booking information

## Architecture

```
┌───────────────────────────────────────────────────┐
│               ServiceNow Platform                  │
├───────────────────────────────────────────────────┤
│  User Interaction Layer                            │
│  ├─ Service Catalog Item (Book A Metro Ticket)      │
│  └─ Service Portal UI                               │
├───────────────────────────────────────────────────┤
│  Business Logic Layer                               │
│  ├─ Catalog Client Scripts                          │
│  ├─ GlideAjax & Script Includes (fare calculation)  │
│  └─ Flow Designer (Metro Ticket Booking Flow)       │
├───────────────────────────────────────────────────┤
│  Data & Service Layer                                │
│  ├─ Metro Database Table                            │
│  ├─ QR Code Generator API                           │
│  ├─ Notification Engine (Email/SMTP)                │
│  └─ Reports & Dashboards                             │
└───────────────────────────────────────────────────┘
```

## Configuration

### Key Configuration Areas

- **Catalog Variables:** Add/modify booking fields on the "Book A Metro Ticket" catalog item
- **Flow Actions:** Extend the Metro Ticket Booking Flow to add validations or additional integrations
- **Fare Rules:** Update fare calculation logic in Script Includes/Client Scripts for new routes
- **QR Code Settings:** Configure the QR code generation API endpoint and image size
- **Notifications:** Customize the "Metro Ticket Confirmation" email template, subject, and recipients
- **Metro Database Table:** Add custom fields as new booking attributes are required

## API Reference

- QR codes are generated using the QR Code Generator API (`https://api.qrserver.com/v1/create-qr-code/`), encoded with the Ticket ID, passenger, route, journey, passenger count, amount, and travel date
- Fare retrieval and validation are handled server-side via **Script Includes** invoked through **GlideAjax**
- For platform-level APIs, refer to the [ServiceNow Developer Documentation](https://developer.servicenow.com)

## Testing

User Acceptance Testing (UAT) was performed across the following areas:

| Section | Total Cases | Pass | Fail | Not Tested |
|---|---|---|---|---|
| Metro Ticket Booking Form | 8 | 8 | 0 | 0 |
| Fare Calculation | 10 | 9 | 1 | 0 |
| Station Selection Validation | 6 | 6 | 0 | 0 |
| GlideAjax & Script Include Integration | 5 | 5 | 0 | 0 |
| Ticket Generation (REQ/RITM) | 8 | 8 | 0 | 0 |
| Email Notifications | 5 | 4 | 1 | 0 |
| QR Code Generation | 4 | 4 | 0 | 0 |

Test coverage includes catalog form validation, fare calculation accuracy, database record creation, QR code generation, and email notification delivery.

## Troubleshooting

| Issue | Solution |
|---|---|
| Ticket record not created in Metro Database | Verify the Metro Ticket Booking Flow is Active and the trigger condition matches the catalog item |
| QR code not appearing in email | Check the QR Code Generator API URL and ensure the record has all required fields (Ticket ID, route, amount, date) |
| Confirmation email not received | Check SMTP configuration and the Email Log on the notification record |
| Fare not calculating | Verify GlideAjax calls to the Script Include and confirm station/fare data exists |
| Required field errors on submission | Confirm mandatory catalog variables (Email Address, Smart Card details, etc.) are correctly marked as required |

## Support & Maintenance

- **Documentation:** Ideation, Design, Planning, and Testing artifacts are maintained as part of the project documentation set (Brainstorming, Problem Statement, Empathy Map, Solution Fit, Proposed Solution, Solution Requirements, Solution Architecture, Technology Stack, Project Planning, UAT Report)
- **Issues:** Report defects or enhancement requests through the project's issue tracker

## Future Enhancements

- Mobile app integration for on-the-go ticket booking and QR scanning
- Integration with digital payment gateways for real-time payment processing
- Smart card balance and recharge history tracking
- Real-time dashboards for revenue, ridership, and booking trend analysis
- Multi-language support for the booking portal
- Integration with turnstile/IoT hardware for automated QR verification at metro gates

## License

This project is provided as-is for metro transportation ticketing system implementations.

## Authors & Contributors

**Team ID:** 23A51A250
**Project Lead:** TAMPA LIKHITA  **

## Acknowledgments

This project demonstrates an end-to-end implementation of ServiceNow Service Catalog, Flow Designer, GlideAjax, and Notifications to solve a real-world public transportation ticketing problem for Hyderabad Metro Rail.

---

**Last Updated:** July 2026
**Version:** 1.0
**Project Name:** Metro Ticket Generating System in ServiceNow
