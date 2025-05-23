# Airtable Table Schema: Bids

This document outlines the schema for the `Bids` table in Airtable. This table is designed to store bids made by transporters for delivering orders.

## Fields:

1.  **`BidID`**:
    *   **Airtable Type**: `Autonumber` (While `UUID` could be used if generated externally, Airtable's `Autonumber` is a practical choice for a unique, auto-incrementing primary key within Airtable.)
    *   **Description**: Unique identifier for each bid.

2.  **`OrderID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Orders`
    *   **Description**: Links to the order that this bid is for. An order might receive multiple bids from different transporters.

3.  **`TransporterID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Users` (It is advisable to configure the link to filter or suggest records where the `Role` field in the `Users` table is "Transporter".)
    *   **Description**: Links to the transporter (from the `Users` table) who submitted this bid.

4.  **`BidAmount`**:
    *   **Airtable Type**: `Currency`
    *   **Description**: The amount the transporter is bidding for the delivery service.

5.  **`ProposedDeliveryDate`**: (Optional)
    *   **Airtable Type**: `Date` (with time included)
    *   **Description**: The date and time the transporter proposes for delivery, if they wish to suggest a time different from the customer's requested window. For the Minimum Viable Product (MVP), it might be assumed that transporters bid on the existing delivery window specified in the order, making this field optional or used for notes on timing.

6.  **`BidStatus`**:
    *   **Airtable Type**: `Single Select`
    *   **Options**: "Submitted", "Accepted", "Rejected", "Withdrawn"
    *   **Description**: Current status of the bid (e.g., a farmer accepts one bid, which then rejects others for the same order, or a transporter withdraws their bid).

7.  **`BidDate`**:
    *   **Airtable Type**: `Created Time`
    *   **Description**: Date and time when the bid was submitted. This is automatically recorded by Airtable when the record is created.

8.  **`Notes`**:
    *   **Airtable Type**: `Long Text`
    *   **Description**: Any additional notes or comments from the transporter regarding their bid (e.g., specific conditions, information about their vehicle, or clarifications on the proposed delivery).

9.  **`LastUpdated`**:
    *   **Airtable Type**: `Last Modified Time`
    *   **Description**: Timestamp automatically updated by Airtable whenever any field in the bid record is modified, indicating the last time the bid details were updated.

## Purpose:

The `Bids` table facilitates the process where transporters can offer their services for specific delivery orders. It allows farmers or the system to review multiple bids and select the most suitable transporter based on factors like bid amount, proposed delivery time (if applicable), and transporter reputation (which could be a future enhancement). Once a bid is accepted, the `OrderStatus` in the `Orders` table would be updated, and the `TransporterID` in the `Orders` table would be set to the `TransporterID` from the accepted bid.
