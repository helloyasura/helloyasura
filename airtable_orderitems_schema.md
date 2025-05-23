# Airtable Table Schema: OrderItems

This document outlines the schema for the `OrderItems` table in Airtable. This table serves as a junction table, linking products from the `Products` table to specific orders in the `Orders` table. Each record in this table represents a single line item within an order.

## Fields:

1.  **`OrderItemID`**:
    *   **Airtable Type**: `Autonumber` (While `UUID` could be used if generated externally, Airtable's `Autonumber` is a practical choice for a unique, auto-incrementing primary key within Airtable.)
    *   **Description**: Unique identifier for each line item in an order.

2.  **`OrderID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Orders`
    *   **Description**: Links to the specific order this item belongs to. This is a many-to-one link, meaning many order items can be associated with a single order.

3.  **`ProductID`**:
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Products`
    *   **Description**: Links to the specific product being ordered from the `Products` table.

4.  **`Quantity`**:
    *   **Airtable Type**: `Number` (Ensure this is configured for Integer values)
    *   **Description**: The quantity of this specific product ordered by the customer.

5.  **`PricePerUnitAtOrder`**:
    *   **Airtable Type**: `Currency`
    *   **Description**: Stores the price per unit of the product at the exact time the order was placed. This is crucial because prices in the `Products` table might change over time. Capturing the price at the moment of order ensures that the customer is billed correctly based on the price they saw during checkout. This value should be populated by looking up the `Products.Price` field when the order item record is created.

6.  **`Subtotal`**:
    *   **Airtable Type**: `Formula`
    *   **Formula**: `{Quantity} * {PricePerUnitAtOrder}` (Actual formula syntax in Airtable might vary slightly, e.g., using field names directly).
    *   **Description**: The total price for this specific line item, calculated by multiplying the `Quantity` by the `PricePerUnitAtOrder`. The `Orders.TotalPrice` field in the `Orders` table will typically be a rollup field that sums the `Subtotal` of all linked order items for a given order.

7.  **`FarmerID`**:
    *   **Airtable Type**: `Lookup`
    *   **Configuration**: This field should be configured to look up the `FarmerID` from the linked `ProductID` record in the `Products` table.
    *   **Description**: Identifies the Farmer who supplies this specific product. This is a convenience field (denormalization) that can simplify reporting and order processing, especially if an order could potentially contain products from multiple farmers in future iterations. It directly reflects the `FarmerID` associated with the `ProductID`.

## Purpose:

The `OrderItems` table is essential for a normalized database structure when dealing with orders that can contain multiple products. Instead of trying to store a list of products within the `Orders` table (which is inefficient and hard to query), this table breaks down each order into its constituent product lines. This allows for:

*   Accurate tracking of the quantity and price of each item in an order.
*   Correct calculation of order subtotals and grand totals.
*   Efficient inventory management by allowing easy updates to `Products.QuantityAvailable` based on ordered quantities.
*   Detailed sales reporting and analysis (e.g., which products are sold most often).
