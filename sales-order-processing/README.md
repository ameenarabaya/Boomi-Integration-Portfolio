# Sales Order Processing & Discount Logic

## Description
This Boomi integration processes sales order data and applies business
rules to calculate discounts based on the total order amount.

## Integration Flow
- Receives sales order data
- Converts data between JSON and XML profiles
- Calculates the total order amount
- Applies discount logic when the total exceeds a defined threshold
- Sets process properties based on discount rules
- Sends email notifications when discount conditions are met
- Ends the process after successful execution

## Business Rules
- If total order amount is greater than a specific value, a discount is applied
- Discount percentage is calculated dynamically
- Notifications are triggered for eligible orders

## Key Concepts Used
- Decision Shapes
- Data Mapping
- Process Properties
- Email Notification
- Conditional Logic

## Tools & Technologies
- Boomi AtomSphere
- JSON / XML Profiles
- Mail Connector



