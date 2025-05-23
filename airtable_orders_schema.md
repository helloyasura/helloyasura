# Airtable Table Schema: Orders

This document outlines the schema for the `Orders` table in Airtable. This table is designed to track customer orders, including details about the customer, order items, delivery, and payment status.

## Fields:

1.  **`OrderID`**:
    *   **Airtable Type**: `Autonumber` (While `UUID` could be used if generated externally, Airtable's `Autonumber` is a practical choice for a unique, auto-incrementing primary key within Airtable.)
    *   **Description**: Unique identifier for each order.

2.  **`CustomerID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Users` (It's advisable to configure the link to filter or suggest records where the `Role` field in the `Users` table is "Customer".)
    *   **Description**: Links to the customer (from the `Users` table) who placed the order.

3.  **`OrderDate`**:
    *   **Airtable Type**: `Created Time` (This automatically records the date and time the order record is created in Airtable, which usually corresponds to when the order was placed.)
    *   **Description**: Date and time when the order was placed.

4.  **`OrderItemsLink`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `OrderItems`
    *   **Description**: Links to multiple records in the `OrderItems` table. Each linked record in `OrderItems` will detail a specific product, its quantity, and subtotal for this order. This is the recommended approach for handling orders with multiple products.

5.  **`TotalPrice`**:
    *   **Airtable Type**: `Rollup`
    *   **Configuration**: This field should be configured to sum the `Subtotal` field from the linked records in the `OrderItems` table.
    *   **Description**: Total calculated price for the entire order, automatically aggregated from all items in the order.

6.  **`DeliveryAddress`**:
    *   **Airtable Type**: `Long Text` (At the time of order placement, this could be copied from the `Users.Address` field of the customer. Storing it here ensures that the delivery address for this specific order remains unchanged even if the user updates their default address later.)
    *   **Description**: Full delivery address for the order.

7.  **`DeliveryWindowStart`**:
    *   **Airtable Type**: `Date` (with time included)
    *   **Description**: The beginning of the customer's chosen or assigned delivery window.

8.  **`DeliveryWindowEnd`**:
    *   **Airtable Type**: `Date` (with time included)
    *   **Description**: The end of the customer's chosen or assigned delivery window.

9.  **`OrderStatus`**:
    *   **Airtable Type**: `Single Select`
    *   **Options**: "Pending Confirmation", "Confirmed", "Farmer Preparing", "Awaiting Transporter", "Out for Delivery", "Delivered", "Canceled", "Disputed"
    *   **Description**: Current status of the order throughout its lifecycle.

10. **`TransporterID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Users` (It's advisable to configure the link to filter or suggest records where the `Role` field in the `Users` table is "Transporter".)
    *   **Description**: Links to the transporter (from the `Users` table) assigned to deliver this order. This is typically assigned after a bid is accepted or through an assignment process.

11. **`PaymentStatus`**:
    *   **Airtable Type**: `Single Select`
    *   **Options**: "Pending", "Paid", "Failed", "Refunded"
    *   **Description**: Status of the payment for this order.

12. **`DeliveryNotes`**:
    *   **Airtable Type**: `Long Text`
    *   **Description**: Any special instructions or notes from the customer regarding the delivery (e.g., "Leave at front door", "Call upon arrival").

13. **`LastUpdated`**:
    *   **Airtable Type**: `Last Modified Time`
    *   **Description**: Timestamp automatically updated by Airtable whenever any field in the order record is modified.

14. **`FarmerID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Users` (Filtered to show only records where the `Role` field is "Farmer")
    *   **Description**: Links to the Farmer fulfilling this order. This assumes an order is fulfilled by a single farmer. If an order can contain products from multiple farmers, this field might be better handled at the `OrderItems` level or through a more complex marketplace logic. For the initial MVP, assuming a single farmer per order simplifies the model.

## Note on `OrderItems` Table:

Using a separate `OrderItems` table is crucial for managing orders with multiple products. Each record in the `OrderItems` table would typically link to an `OrderID` (from this `Orders` table) and a `ProductID` (from the `Products` table), and include fields like `QuantityOrdered` and `Subtotal` (Price * Quantity) for that specific product within the order. This structure allows for detailed order composition and accurate inventory management. The `TotalPrice` in the `Orders` table would then be a rollup of these `Subtotal` values from the linked `OrderItems`.
