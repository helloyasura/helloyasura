# Airtable Table Schema: Users

This document outlines the schema for the `Users` table in Airtable. This table stores information common to all user roles (Customer, Farmer, Transporter, Salesman) and role-specific details where applicable.

## Fields:

1.  **`UserID`**:
    *   **Airtable Type**: `Autonumber` (Primary Key - Airtable's equivalent for a unique, auto-incrementing ID. While UUID is ideal, Autonumber is a practical choice within Airtable if direct UUID generation isn't a primary field type.)
    *   **Description**: Unique identifier for each user.

2.  **`Role`**:
    *   **Airtable Type**: `Single Select`
    *   **Options**: "Customer", "Farmer", "Transporter", "Salesman"
    *   **Description**: Defines the user's primary role in the system.

3.  **`Name`**:
    *   **Airtable Type**: `Single Line Text`
    *   **Description**: Full name of the user.

4.  **`Email`**:
    *   **Airtable Type**: `Email`
    *   **Description**: User's email address. This will be used for login with Firebase Authentication and for sending notifications.

5.  **`PhoneNumber`**:
    *   **Airtable Type**: `Phone Number`
    *   **Description**: User's contact phone number, which can be used for SMS notifications or direct contact.

6.  **`ProfilePictureURL`**:
    *   **Airtable Type**: `URL`
    *   **Description**: Link to the user's profile picture. This could be hosted on Firebase Storage or another image hosting service.

7.  **`Address`**:
    *   **Airtable Type**: `Long Text` (Consider breaking into Street, City, State, Zip Code, Country fields if granular searching/filtering on address components is required in the future.)
    *   **Description**: Physical address of the user. This is important for delivery logistics (for customers and farmers) and defining service areas.

8.  **`FarmerRating`**: (Applies to users with the "Farmer" role)
    *   **Airtable Type**: `Rating` (or `Number` with a defined scale, e.g., 1-5, if a specific numerical system is preferred)
    *   **Description**: Average rating for users with the "Farmer" role. This will be calculated from customer feedback and reviews (a future feature). It can be initially blank or manually set.

9.  **`DateJoined`**:
    *   **Airtable Type**: `Created Time`
    *   **Description**: Timestamp indicating when the user record was created in Airtable. This is automatically managed by Airtable.

10. **`LastLogin`**:
    *   **Airtable Type**: `Date` (with time included)
    *   **Description**: Timestamp of the user's last login. This is useful for activity tracking. Note: Firebase Authentication might provide this information externally, which could be synced to this field if necessary.

11. **`Certifications`**: (Primarily applies to users with the "Farmer" role)
    *   **Airtable Type**: `Link to Another Record`
    *   **Linked Table**: `Certifications` (A separate table to manage certification details)
    *   **Description**: Links to the certifications uploaded or claimed by this farmer.

12. **`StripeCustomerID`**: (Or other payment gateway ID)
    *   **Airtable Type**: `Single Line Text`
    *   **Description**: Stores the unique customer identifier from Stripe (or another payment gateway). This will be used for processing payments and managing subscriptions. This field can be added when payment integration is implemented and is optional for the Minimum Viable Product (MVP).

## Regarding `PasswordHash`:

The `PasswordHash` field is **not included** in this schema. As Firebase Authentication will be used for managing user logins, Firebase will securely handle password storage and authentication. Storing password hashes separately in Airtable would be redundant and less secure. Firebase Auth provides a robust and secure authentication solution, making a dedicated `PasswordHash` field unnecessary.
