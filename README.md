# MySFProject / Nordic Solutions

## Overview
This Salesforce DX project contains the metadata for my assignment. It includes an expanded custom data model as well as a Flow automation as defined in the assignment.

## Data Model Design

- Custom object "Subscription" 
- I went with a completely separate custom object to create and track the subscriptions. I feel that this approach is more scalable and customizable in the future and it is more clear because it has a direct clear purpose from a business perspective and even for supposed end users.
- Another reasoning for the custom object which is also related to scalability, is that technically the fields can be added to the Opportunity Product object, however, these fields would then always be present with other products which may have no need of the fields related to these subscriptions.
- The Subscription object consits of two Date fields: 'Start Date' & 'End Date', and a picklist field with the values 'Trial', 'Active' and 'Expired'.
- The Opportunity Product contains a lookup relationship field to the Subscription object.
    - This allows the Subscription record to be displayed fields of the Opportunity Product's related list on  the opportunity object (as shown in the screenshot: "https://ibb.co/rKc0hqT9") which can be cliked to directly navigate to the Subscription record. The lookup relationship was also used due to the fact that the "Opportunity Product" object does not allow for Master-Detail relationships.
- Another reasoning for this is that if a user adds a Opportunity Product manually, they can then edit the Opportunity Product from the list view, and in the edit window they can create a new Subscription record to link to the product, without ever having to leave the Opportunity record page.


## Flow Design
The project also includes a record triggered flow 'Opportunity After Save Flow'.

- The flow runs after being created, without any start conditions currently.

- The first step of the Flow is a decision element, where it checks the value of the 'Type' field.

- For 'Type' value 'New Business'
    - The automation creates a Subscription record
    - Gets the Price Book Entry record for the product name'Analytics Suite'.
    - Gets the Product record for the product name 'Analytics Suite'.
    - Creates the Opportunity Product with assigned values and links it to the triggering Opportunity record.

- For 'Type' value 'New Business'
-   Gets the most recent 'Close Date' value from the Account linked to the Opportunity utilizing a roll up field on the Account for the 'Closed Won' on related Opportunities.
-   Gets the Opportunity Product records from the most recent won Opportunity
-   Loops through all the prodcuts on that Opportunity, Creates Subscription records and adds it to the Opportunity Product, then creates the Opportunity Products through a collection variable.

- For the default outcome of this decision
    - The automation creates a Task linked to the Opportunity, assigned to the Owner of the triggering Opportunity, with a due date and reminder set one week from current date.


- Location of flow: `force-app/main/default/flows/`