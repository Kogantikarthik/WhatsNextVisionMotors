# WhatsNext Vision Motors

## Project Overview

WhatsNext Vision Motors is a Salesforce CRM implementation designed to streamline vehicle sales and customer service operations. The project manages vehicles, customers, dealers, vehicle orders, test drives, and service requests through a centralized Salesforce application.

The system uses Salesforce Flow and Apex automation to improve order processing, vehicle stock management, dealer assignment, and test drive reminders.

## Key Features

* Manage vehicle details, pricing, availability, and stock quantity.
* Maintain customer and dealer information.
* Create and manage vehicle orders.
* Assign dealers to orders based on customer location.
* Prevent vehicle orders when the vehicle is out of stock.
* Automatically reduce vehicle stock when an order is confirmed.
* Process pending orders automatically when stock becomes available.
* Send scheduled email reminders for upcoming test drives.
* Automate vehicle order processing using Batch Apex and Scheduled Apex.

## Salesforce Objects

| Object                       | Purpose                                                          |
| ---------------------------- | ---------------------------------------------------------------- |
| `Vehicle__c`                 | Stores vehicle model, price, stock quantity, dealer, and status. |
| `Vehicle_Dealer__c`          | Stores authorized dealer information and location.               |
| `Vehicle_Customer__c`        | Stores customer contact information and preferred vehicle type.  |
| `Vehicle_Order__c`           | Tracks vehicle purchase orders and their status.                 |
| `Vehicle_Test_Drive__c`      | Manages test drive bookings and status.                          |
| `Vehicle_Service_Request__c` | Tracks vehicle servicing requests and their status.              |

## Object Relationships

* `Vehicle_Order__c` is related to `Vehicle_Customer__c` and `Vehicle__c`.
* `Vehicle_Test_Drive__c` is related to `Vehicle_Customer__c` and `Vehicle__c`.
* `Vehicle_Service_Request__c` is related to `Vehicle_Customer__c` and `Vehicle__c`.
* `Vehicle__c` is related to `Vehicle_Dealer__c`.

## Automation

### 1. Auto Assign Dealer Flow

**Flow:** `Auto_Assign_Dealer`

A record-triggered Flow runs when a vehicle order is created with a `Pending` status. It retrieves the related customer and identifies the dealer based on the customer's location, then assigns the dealer information for the order process.

### 2. Test Drive Reminder Flow

**Flow:** `Test_Drive_Reminder`

A record-triggered Flow runs when a test drive is scheduled. A scheduled path is configured to run one day before the test drive and sends an email reminder to the customer.

## Apex Implementation

### Vehicle Order Trigger

**Trigger:** `VehicleOrderTrigger`

The trigger executes during:

* Before Insert
* Before Update
* After Insert
* After Update

It calls the `VehicleOrderTriggerHandler` to perform order validation and stock updates.

### Vehicle Order Trigger Handler

**Class:** `VehicleOrderTriggerHandler`

The handler:

* Prevents orders when the selected vehicle has zero or negative stock.
* Checks vehicle stock before processing orders.
* Decreases vehicle stock when an order is confirmed.
* Uses bulkified SOQL and DML operations.

### Vehicle Order Batch

**Class:** `VehicleOrderBatch`

The Batch Apex class processes pending vehicle orders. When stock becomes available, eligible pending orders are changed to `Confirmed` and the corresponding vehicle stock is reduced.

### Vehicle Order Batch Scheduler

**Class:** `VehicleOrderBatchScheduler`

The Scheduler class executes the `VehicleOrderBatch` as a scheduled Apex job.

## Technology Used

* Salesforce CRM
* Salesforce Lightning Experience
* Salesforce Flow
* Apex
* SOQL
* Batch Apex
* Scheduled Apex
* Salesforce DX
* Salesforce CLI
* VS Code
* Git & GitHub

## Project Structure

```text
WhatsNextVisionMotors/
│
├── force-app/
│   └── main/
│       └── default/
│           ├── applications/
│           ├── classes/
│           ├── flows/
│           ├── objects/
│           ├── tabs/
│           └── triggers/
│
├── scripts/
├── config/
├── .forceignore
├── .gitignore
├── sfdx-project.json
└── README.md
```

## Project Outcome

The implementation provides a centralized Salesforce solution for managing vehicle sales and customer interactions. Automation reduces manual work in dealer assignment, stock validation, order processing, and test drive communication, helping improve operational efficiency and customer experience.

## Author

**Karthik Chowdary Koganti**
