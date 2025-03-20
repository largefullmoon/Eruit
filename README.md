# Eruit iOS App API Documentation

This document provides a comprehensive reference for all API endpoints used in the Eruit iOS application. For each endpoint, you'll find details about its purpose, HTTP method, parameters, required headers, and response format.

## Base URL
http://54.74.47.46:82/


## Authentication

Most API endpoints require authentication. Include the following headers in authenticated requests:

Authorization: Bearer {token}
Language: English or Hebrew
platformType: ios



The authentication token is obtained from the login API.

## Common Response Structure

Most API responses follow this standard structure:

```json
{
  "RequestResponse": boolean,  // Success indicator (true/false)
  "Messages": ["string"],      // Array of response messages
  "Data": {}                   // Response data - structure varies by endpoint
}
```

---

## Table of Contents

### Authentication APIs
1. [Login](#1-login)
2. [Register](#2-register)
3. [Forgot Password](#3-forgot-password)
4. [Change User Password](#4-change-user-password)
5. [Deactivate User](#5-deactivate-user)

### Dashboard APIs
6. [Get Dashboard Events](#6-get-dashboard-events)

### Quick Select APIs
7. [Get Main By First Number](#7-get-main-by-first-number)
8. [Get Order Status](#8-get-order-status)

### User APIs
9. [Get Profile Details](#9-get-profile-details)
10. [Update Profile](#10-update-profile)
11. [Get Login User Detail](#11-get-login-user-detail)

### Order APIs
12. [Get Order List](#12-get-order-list)
13. [Get Order By Id](#13-get-order-by-id)
14. [Get Order Mob Details By Id](#14-get-order-mob-details-by-id)
15. [Get Order Bookings](#15-get-order-bookings)
16. [Save Order Data](#16-save-order-data)
17. [Get Order Summary](#17-get-order-summary)
18. [Download Order PDF](#18-download-order-pdf)
19. [Get Order Event Mob Details By Id](#19-get-order-event-mob-details-by-id)
20. [Get Events And Item By Order Number](#20-get-events-and-item-by-order-number)
21. [Save Order Summary](#21-save-order-summary)
22. [Get Menu Portion Item](#22-get-menu-portion-item)
23. [Get Hebdate And Day Event](#23-get-hebdate-and-day-event)
24. [Get Calculation Order Summary](#24-get-calculation-order-summary)
25. [Get New Menu Item](#25-get-new-menu-item)
26. [Update Events](#26-update-events)
27. [Update Event Item](#27-update-event-item)
28. [Delete Event Item](#28-delete-event-item)
29. [Get Portion Exist With Item](#29-get-portion-exist-with-item)
30. [Update Item Description](#30-update-item-description)
31. [Upload Item Image](#31-upload-item-image)

### Meeting APIs
32. [Get Meetings](#32-get-meetings)
33. [Get Meeting By Id](#33-get-meeting-by-id)
34. [Save Meeting](#34-save-meeting)
35. [Send Meeting Confirmation](#35-send-meeting-confirmation)
36. [Update Meeting Exist](#36-update-meeting-exist)

### Comment APIs
37. [Get Order Comments](#37-get-order-comments)
38. [Save Order Comment](#38-save-order-comment)

### Notification APIs
39. [Get Notifications](#39-get-notifications)
40. [Get Notification Lead Details](#40-get-notification-lead-details)
41. [Get Notification Follow Up Details](#41-get-notification-follow-up-details)

---

## Authentication APIs

### 1. Login

Authenticates a user and returns an authorization token.

- **Endpoint**: `Account/Login`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "userName": "string",  // Required: Username for authentication
    "password": "string",  // Required: User password
    "rememberMe": boolean  // Optional: Whether to remember login credentials
  }
  ```
  - Query Parameter: `firmName=string` (Required: Company/Firm identifier)
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,  // Success indicator (true/false)
    "Messages": ["string"],      // Array of response messages
    "Data": "string"             // Authentication token for API access
  }
  ```

### 2. Register

Registers a new user in the system.

- **Endpoint**: `Account/CreateUser`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "FirstName": "string",       // Required: User's first name
    "LastName": "string",        // Required: User's last name
    "UserName": "string",        // Required: Unique username
    "Password": "string",        // Required: User password
    "EmailId": "string",         // Required: Email address
    "Gender": "string",          // Optional: User gender
    "UserType": "string",        // Required: Type of user (e.g., "Customer")
    "Language": "string",        // Optional: Preferred language (e.g., "English", "Hebrew")
    "Phone": "string",           // Optional: Phone number
    "ProfilePic": "string",      // Optional: Profile picture (Base64 encoded)
    "IsDisplayName": boolean,    // Optional: Whether to display name
    "IsDisplayEmail": boolean,   // Optional: Whether to display email
    "IsDisplayNumber": boolean   // Optional: Whether to display phone number
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,  // Success indicator (true/false)
    "Messages": ["string"],      // Array of response messages
    "Data": "string"             // User ID or registration success message
  }
  ```

### 3. Forgot Password

Initiates the password recovery process for a user.

- **Endpoint**: `AccountMob/ForgotPassword`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "EmailId": "string",         // Required: User's registered email address
    "FirmName": "string"         // Required: Company/Firm identifier
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,  // Success indicator (true/false)
    "Messages": ["string"],      // Array of response messages (e.g., "Password reset link sent to your email")
    "Data": "string"             // Optional: Additional information
  }
  ```

### 4. Change User Password

Changes the password for an authenticated user.

- **Endpoint**: `AccountMob/ChangeUserPassword`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "UserId": "string",          // Required: User ID
    "CurrentPassword": "string", // Required: Current password for verification
    "NewPassword": "string"      // Required: New password to set
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,  // Success indicator (true/false)
    "Messages": ["string"],      // Array of response messages (e.g., "Password changed successfully")
    "Data": "string"             // Optional: Additional information
  }
  ```

### 5. Deactivate User

Deactivates a user account.

- **Endpoint**: `AccountMob/DeactiveUser`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "UserId": "string"  // Required: ID of the user to deactivate
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,  // Success indicator (true/false)
    "Messages": ["string"],      // Array of response messages (e.g., "User deactivated successfully")
    "Data": "string"             // Optional: Additional information
  }
  ```

## Dashboard APIs

### 6. Get Dashboard Events

Retrieves events for the dashboard view.

- **Endpoint**: `Dashboard/GetDashboardEvents`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "StartDate": "string",         // Required: Start date for events (format: YYYY-MM-DD)
    "EndDate": "string",           // Required: End date for events (format: YYYY-MM-DD)
    "EventType": "string",         // Optional: Filter by event type (e.g., "Meetings", "Holidays", "Events")
    "IsFilterEnable": boolean,     // Optional: Whether filtering is enabled
    "IsMonthView": boolean,        // Optional: Whether to return month view data
    "Language": "string"           // Optional: Language preference (e.g., "Hebrew", "English")
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,        // Success indicator (true/false)
    "Messages": ["string"],            // Array of response messages
    "Data": [                          // Array of dashboard events
      {
        "id": "string",                // Event ID
        "title": "string",             // Event title
        "type": "string",              // Event type
        "start": "string",             // Start date
        "end": "string",               // End date
        "allDay": boolean,             // Whether it's an all-day event
        "backgroundColor": "string",    // Background color for UI
        "borderColor": "string",       // Border color for UI
        "color": "string",             // Text color for UI
        "order": number,               // Order/priority number
        "heb_date": "string",          // Hebrew date string
        "isHolidy": boolean,           // Whether it's a holiday
        "isMenuItem": boolean,         // Whether it's a menu item
        "roundCircleColor": "string",  // Round circle color for UI
        "eventType": "string",         // Type of event (e.g., "Meetings", "Holidays")
        "lstEventName": ["string"],    // List of associated event names
        "colorHexa": "string",         // Hex color code
        "startMeetingTime": "string",  // Start time for meetings
        "endMeetingTime": "string",    // End time for meetings
        "meetingAddress": "string",    // Meeting address
        "mobileNumber": "string",      // Contact mobile number
        "email": "string",             // Contact email
        "start_time": "string",        // Start time
        "end_time": "string",          // End time
        "hallName": "string",          // Hall/venue name
        "name": "string",              // Associated name
        "StatusLabel": "string"        // Status label
      }
    ]
  }
  ```

## Quick Select APIs

### 7. Get Main By First Number

Retrieves main data based on first number.

- **Endpoint**: `QuickSelect/GetMainByfirst_number`
- **Method**: GET
- **Parameters**: None
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,    // Success indicator (true/false)
    "Messages": ["string"],        // Array of response messages
    "Data": [                      // Array of main data items
      {
        "ID": "string",            // Item ID
        "Title": "string",         // Item title
        "Number": "string",        // Number identifier
        "FirstNumber": "string",   // First number identifier
        "Description": "string",   // Item description
        "OrderBy": number,         // Sorting order
        "ParentId": "string",      // Parent item ID
        "IsMenuItem": boolean      // Whether it's a menu item
      }
    ]
  }
  ```

### 8. Get Order Status

Retrieves available order status options.

- **Endpoint**: `QuickSelect/GetOrderStatus`
- **Method**: GET
- **Parameters**: None
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,    // Success indicator (true/false)
    "Messages": ["string"],        // Array of response messages
    "Data": [                      // Array of order status options
      {
        "StatusId": "string",      // Status ID
        "StatusName": "string",    // Status name (e.g., "Pending", "Confirmed", "Completed")
        "StatusOrder": number,     // Display order
        "StatusColor": "string",   // Color code for the status
        "IsDefault": boolean       // Whether it's the default status
      }
    ]
  }
  ```

## User APIs

### 9. Get Profile Details

Retrieves the authenticated user's profile information.

- **Endpoint**: `UserMob/GetUserMobProfile`
- **Method**: GET
- **Parameters**: None (uses authorization token)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,       // Success indicator (true/false)
    "Messages": ["string"],           // Array of response messages
    "Data": {                         // User profile data
      "UserId": "string",             // User ID
      "FirstName": "string",          // First name
      "LastName": "string",           // Last name
      "UserName": "string",           // Username
      "EmailId": "string",            // Email address
      "Gender": "string",             // Gender
      "UserType": "string",           // User type
      "Language": "string",           // Preferred language
      "Phone": "string",              // Phone number
      "Status": "string",             // Account status
      "ProfilePic": "string",         // Profile picture URL
      "IsDisplayName": boolean,       // Whether to display name
      "IsDisplayEmail": boolean,      // Whether to display email
      "IsDisplayNumber": boolean,     // Whether to display phone number
      "ClientModel": {                // Client/company information
        "ClientId": "string",         // Client ID
        "FirmName": "string",         // Firm name
        "CompanyName": "string",      // Company name
        "EmailId": "string",          // Company email
        "Address": "string",          // Address
        "ContactNo": "string",        // Contact number
        "Website": "string",          // Website
        "Logo": "string"              // Company logo URL
      }
    }
  }
  ```

### 10. Update Profile

Updates the authenticated user's profile information.

- **Endpoint**: `UserMob/UpdateUserMobProfile`
- **Method**: POST
- **Content-Type**: `multipart/form-data` (for image upload)
- **Parameters**:
  ```
  // Form Fields:
  UserId: string              // Required: User ID
  FirstName: string           // Required: First name
  LastName: string            // Optional: Last name
  UserName: string            // Required: Username
  EmailId: string             // Required: Email address
  Gender: string              // Optional: Gender
  Language: string            // Optional: Preferred language
  Phone: string               // Optional: Phone number
  IsDisplayName: boolean      // Optional: Whether to display name
  IsDisplayEmail: boolean     // Optional: Whether to display email
  IsDisplayNumber: boolean    // Optional: Whether to display phone number
  
  // File Upload:
  ProfilePic: file            // Optional: Profile image file (JPEG/PNG)
  ```
- **Headers**:
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `EditProfileBaseModel`
  ```json
  {
    "RequestResponse": boolean,      // Success indicator (true/false)
    "Messages": ["string"],          // Array of response messages
    "Data": {
      "UserId": "string",            // User ID
      "ProfilePic": "string",        // Updated profile picture URL
      "Message": "string"            // Success or error message
    }
  }
  ```

### 11. Get Login User Detail

Retrieves detailed information about the authenticated user.

- **Endpoint**: `Account/GetLoginUserDetail`
- **Method**: GET
- **Parameters**: None (uses authorization token)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `LoginUserDetailBaseModel`
  ```json
  {
    "RequestResponse": boolean,       // Success indicator (true/false)
    "Messages": ["string"],           // Array of response messages
    "Data": {                         // Detailed user data
      "UserId": "string",             // User ID
      "FirstName": "string",          // First name
      "LastName": "string",           // Last name
      "UserName": "string",           // Username
      "Password": "string",           // Password (likely masked)
      "EmailId": "string",            // Email address
      "Gender": "string",             // Gender
      "UserType": "string",           // User type
      "Language": "string",           // Preferred language
      "Phone": "string",              // Phone number
      "Status": "string",             // Account status
      "ProfilePic": "string",         // Profile picture URL
      "IsDeleted": boolean,           // Whether user is deleted
      "IsDisplayName": boolean,       // Whether to display name
      "IsDisplayEmail": boolean,      // Whether to display email
      "IsDisplayNumber": boolean,     // Whether to display phone number
      "OrderStatus": "string",        // Order status
      "RoleId": "string",             // Role ID
      "ClientModel": {                // Client/company information
        "ClientId": "string",         // Client ID
        "FirstName": "string",        // Client first name
        "LastName": "string",         // Client last name
        "FirmName": "string",         // Firm name
        "CompanyName": "string",      // Company name
        "EmailId": "string",          // Company email
        "Address": "string",          // Address
        "PostalCode": "string",       // Postal code
        "ContactNo": "string",        // Contact number
        "Status": "string",           // Client status
        "Website": "string",          // Website
        "DateFormat": "string",       // Preferred date format
        "Language": "string",         // Client language preference
        "Logo": "string",             // Company logo URL
        "IsHebDate": boolean,         // Whether to use Hebrew date
        "PrintHebDate": boolean,      // Whether to print Hebrew date
        "Client_GeneralSetting": {},  // Client general settings
        "Client_SMTP": {},            // Client SMTP settings
        "GoogleApiCredential": {},    // Google API credentials
        "SMSCredential": {}           // SMS credentials
      },
      "UserRole": {                   // User role information
        "RoleId": "string",           // Role ID
        "RoleName": "string",         // Role name
        "IsDeleted": boolean          // Whether role is deleted
      },
      "UserHallXref": [],             // User-Hall cross references
      "UserAgentXref": [],            // User-Agent cross references
      "User_SMTP": ["string"],        // User SMTP settings
      "EventColors": [],              // Event color preferences
      "UserRolesList": []             // List of user roles
    },
    "File": "string"                  // Optional: File reference
  }
  ```

## Order APIs

### 12. Get Order List

Retrieves a list of orders for the authenticated user.

- **Endpoint**: `Order/GetOrders`
- **Method**: GET
- **Parameters**: None (uses authorization token)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,      // Success indicator (true/false)
    "Messages": ["string"],          // Array of response messages
    "Data": [                        // Array of orders
      {
        "OrderId": "string",         // Order ID
        "OrderNumber": "string",     // Order number
        "CustomerName": "string",    // Customer name
        "OrderDate": "string",       // Order date
        "EventDate": "string",       // Event date
        "EventAddress": "string",    // Event address
        "EventType": "string",       // Event type
        "Status": "string",          // Order status
        "OrderTotal": number,        // Order total amount
        "IsVAT": boolean,            // Whether VAT is applied
        "CreatedBy": "string",       // User who created the order
        "CreatedDate": "string",     // Creation date
        "ModifiedBy": "string",      // User who last modified the order
        "ModifiedDate": "string"     // Last modification date
      }
    ]
  }
  ```

### 13. Get Order By Id

Retrieves detailed information for a specific order.

- **Endpoint**: `Order/GetOrderById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/Order/GetOrderById?orderId={orderId}` (Required: Order ID)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Order details
      "OrderId": "string",          // Order ID
      "OrderNumber": "string",      // Order number
      "CustomerName": "string",     // Customer name
      "OrderDate": "string",        // Order date
      "EventDate": "string",        // Event date
      "EventAddress": "string",     // Event address
      "EventType": "string",        // Event type
      "Status": "string",           // Order status
      "OrderTotal": number,         // Order total amount
      "IsVAT": boolean,             // Whether VAT is applied
      "CreatedBy": "string",        // User who created the order
      "CreatedDate": "string",      // Creation date
      "ModifiedBy": "string",       // User who last modified the order
      "ModifiedDate": "string",     // Last modification date
      "OrderItems": [               // Array of order items
        {
          "ItemId": "string",       // Item ID
          "ItemName": "string",     // Item name
          "Quantity": number,       // Quantity
          "Price": number,          // Price per unit
          "Total": number,          // Total price for this item
          "ItemDescription": "string" // Item description
        }
      ]
    }
  }
  ```

### 14. Get Order Mob Details By Id

Retrieves mobile-specific details for a specific order.

- **Endpoint**: `OrderMob/GetOrderMobDetailsById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetOrderMobDetailsById?orderId={orderId}` (Required: Order ID)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Mobile-specific order details
      "OrderId": "string",          // Order ID
      "OrderNumber": "string",      // Order number
      "CustomerName": "string",     // Customer name
      "CustomerPhone": "string",    // Customer phone number
      "CustomerEmail": "string",    // Customer email
      "OrderDate": "string",        // Order date
      "EventDate": "string",        // Event date
      "EventAddress": "string",     // Event address
      "EventType": "string",        // Event type
      "Status": "string",           // Order status
      "OrderTotal": number,         // Order total amount
      "VAT": number,                // VAT amount
      "IsVAT": boolean,             // Whether VAT is applied
      "CreatedBy": "string",        // User who created the order
      "CreatedDate": "string",      // Creation date
      "ModifiedBy": "string",       // User who last modified the order
      "ModifiedDate": "string",     // Last modification date
      "Events": [                   // Array of order events
        {
          "EventId": "string",      // Event ID
          "EventName": "string",    // Event name
          "StartDate": "string",    // Start date
          "EndDate": "string",      // End date
          "ItemCount": number,      // Number of items
          "Items": [                // Array of event items
            {
              "ItemId": "string",   // Item ID
              "ItemName": "string", // Item name
              "Quantity": number,   // Quantity
              "Price": number,      // Price per unit
              "Total": number,      // Total price for this item
              "ItemDescription": "string", // Item description
              "ImageUrl": "string"  // Item image URL
            }
          ]
        }
      ]
    }
  }
  ```

### 15. Get Order Bookings

Retrieves order bookings for the authenticated user.

- **Endpoint**: `OrderMob/GetOrderBookings`
- **Method**: GET
- **Parameters**: None (uses authorization token)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,      // Success indicator (true/false)
    "Messages": ["string"],          // Array of response messages
    "Data": [                        // Array of order bookings
      {
        "BookingId": "string",       // Booking ID
        "OrderId": "string",         // Order ID
        "OrderNumber": "string",     // Order number
        "CustomerName": "string",    // Customer name
        "EventDate": "string",       // Event date
        "EventType": "string",       // Event type
        "EventAddres": "string",     // Event address
        "BookingStatus": "string",   // Booking status
        "StartDate": "string",       // Start date
        "EndDate": "string"          // End date
      }
    ]
  }
  ```

### 16. Save Order Data

Creates or updates an order with events and items.

- **Endpoint**: `OrderMob/SaveOrderData`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Optional: Order ID (for update; omit for new order)
    "CustomerName": "string",       // Required: Customer name
    "CustomerPhone": "string",      // Required: Customer phone
    "CustomerEmail": "string",      // Optional: Customer email
    "OrderDate": "string",          // Required: Order date (format: YYYY-MM-DD)
    "Status": "string",             // Required: Order status
    "IsVAT": boolean,               // Optional: Whether VAT is applied
    "Events": [                     // Array of event information
      {
        "EventId": "string",        // Optional: Event ID (for update)
        "EventName": "string",      // Required: Event name
        "StartDate": "string",      // Required: Start date (format: YYYY-MM-DD)
        "EndDate": "string",        // Required: End date (format: YYYY-MM-DD)
        "EventAddress": "string",   // Optional: Event address
        "Items": [                  // Array of event items
          {
            "ItemId": "string",     // Optional: Item ID (for update)
            "ItemName": "string",   // Required: Item name
            "Quantity": number,     // Required: Quantity
            "Price": number,        // Required: Price per unit
            "ItemDescription": "string" // Optional: Item description
          }
        ]
      }
    ]
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "OrderId": "string",          // Created/updated order ID
      "OrderNumber": "string",      // Order number
      "Message": "string"           // Success or error message
    }
  }
  ```

### 17. Get Order Summary

Retrieves a summary of a specific order.

- **Endpoint**: `OrderMob/GetOrderSummary`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetOrderSummary?orderId={orderId}` (Required: Order ID)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Order summary
      "OrderId": "string",          // Order ID
      "OrderNumber": "string",      // Order number
      "CustomerName": "string",     // Customer name
      "OrderDate": "string",        // Order date
      "OrderTotal": number,         // Order total amount
      "VAT": number,                // VAT amount
      "GrandTotal": number,         // Grand total (including VAT)
      "Status": "string",           // Order status
      "Events": [                   // Array of events summary
        {
          "EventId": "string",      // Event ID
          "EventName": "string",    // Event name
          "ItemCount": number,      // Number of items
          "EventTotal": number      // Event total amount
        }
      ]
    }
  }
  ```

### 18. Download Order PDF

Downloads a PDF document for a specific order.

- **Endpoint**: `OrderMob/DownloadOrderPdf`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/DownloadOrderPdf?orderId={orderId}` (Required: Order ID)
- **Headers**:
  - `Accept`: `application/pdf`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: Binary PDF file data
  - Content-Type: `application/pdf`
  - The response is a PDF file that can be downloaded or displayed

### 19. Get Order Event Mob Details By Id

Retrieves detailed information for a specific event within an order.

- **Endpoint**: `OrderMob/GetOrderEventMobDetailsById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetOrderEventMobDetailsById?orderId={orderId}&eventId={eventId}` 
    - `orderId`: Required: Order ID
    - `eventId`: Required: Event ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Mobile-specific event details
      "EventId": "string",          // Event ID
      "EventName": "string",        // Event name
      "StartDate": "string",        // Start date
      "EndDate": "string",          // End date
      "EventAddress": "string",     // Event address
      "Items": [                    // Array of event items
        {
          "ItemId": "string",       // Item ID
          "ItemName": "string",     // Item name
          "Quantity": number,       // Quantity
          "Price": number,          // Price per unit
          "Total": number,          // Total price for this item
          "ItemDescription": "string", // Item description
          "ImageUrl": "string"      // Item image URL
        }
      ]
    }
  }
  ```

### 20. Get Events And Item By Order Number

Retrieves events and items for a specific order by order number.

- **Endpoint**: `OrderMob/GetEventsAndItemByOrderNumber`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetEventsAndItemByOrderNumber?orderNumber={orderNumber}` 
    - `orderNumber`: Required: Order number
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "OrderId": "string",          // Order ID
      "OrderNumber": "string",      // Order number
      "Events": [                   // Array of events with items
        {
          "EventId": "string",      // Event ID
          "EventName": "string",    // Event name
          "EventDate": "string",    // Event date
          "EventAddress": "string", // Event address
          "Items": [                // Array of event items
            {
              "ItemId": "string",   // Item ID
              "ItemName": "string", // Item name
              "Quantity": number,   // Quantity
              "Price": number,      // Price per unit
              "ItemDescription": "string" // Item description
            }
          ]
        }
      ]
    }
  }
  ```

### 21. Save Order Summary

Updates the summary information for an order.

- **Endpoint**: `OrderMob/SaveOrderSummary`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Required: Order ID
    "TotalAmount": number,          // Required: Total amount
    "VAT": number,                  // Optional: VAT amount
    "GrandTotal": number,           // Required: Grand total
    "Status": "string",             // Required: Order status
    "IsVAT": boolean                // Optional: Whether VAT is applied
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "OrderId": "string",          // Order ID
      "Message": "string"           // Success or error message
    }
  }
  ```

### 22. Get Menu Portion Item

Retrieves menu portion items with optional filtering.

- **Endpoint**: `OrderMob/GetMenuPortionItem`
- **Method**: GET
- **Parameters**: 
  - URL Parameters: 
    - `menuId`: Optional: Menu ID filter
    - `parentId`: Optional: Parent menu ID filter
    - `search`: Optional: Search text
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": [                       // Array of menu portion items
      {
        "PortionId": "string",      // Portion ID
        "PortionName": "string",    // Portion name
        "MenuId": "string",         // Menu ID
        "MenuName": "string",       // Menu name
        "Price": number,            // Price
        "IsVAT": boolean,           // Whether VAT is applied
        "Description": "string",    // Description
        "ImageUrl": "string"        // Image URL
      }
    ]
  }
  ```

### 23. Get Hebdate And Day Event

Retrieves Hebrew date and day events for a specified date.

- **Endpoint**: `OrderMob/GetHebdateAndDayEvent`
- **Method**: GET
- **Parameters**: 
  - URL Parameters: 
    - `engDate`: Required: English date (format: YYYY-MM-DD)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "HebDate": "string",          // Hebrew date
      "DayEvents": [                // Array of day events
        {
          "EventId": "string",      // Event ID
          "EventName": "string",    // Event name
          "EventType": "string",    // Event type
          "IsHoliday": boolean      // Whether it's a holiday
        }
      ]
    }
  }
  ```

### 24. Get Calculation Order Summary

Calculates and retrieves financial summary for an order.

- **Endpoint**: `OrderMob/GetCalculationOrderSummary`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetCalculationOrderSummary?orderId={orderId}` 
    - `orderId`: Required: Order ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "OrderId": "string",          // Order ID
      "SubTotal": number,           // Subtotal amount
      "VATPercent": number,         // VAT percentage
      "VAT": number,                // VAT amount
      "GrandTotal": number,         // Grand total amount
      "IsVAT": boolean              // Whether VAT is applied
    }
  }
  ```

### 25. Get New Menu Item

Retrieves new menu items with optional filtering.

- **Endpoint**: `OrderMob/GetNewMenuItem`
- **Method**: GET
- **Parameters**: 
  - URL Parameters: 
    - `search`: Optional: Search text
    - `categoryId`: Optional: Category ID filter
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": [                       // Array of menu items
      {
        "ItemId": "string",         // Item ID
        "ItemName": "string",       // Item name
        "CategoryId": "string",     // Category ID
        "CategoryName": "string",   // Category name
        "Description": "string",    // Description
        "Price": number,            // Price
        "IsVAT": boolean,           // Whether VAT is applied
        "ImageUrl": "string",       // Image URL
        "HasPortions": boolean      // Whether item has portions
      }
    ]
  }
  ```

### 26. Update Events

Updates an event within an order.

- **Endpoint**: `OrderMob/UpdateEvents`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Required: Order ID
    "EventId": "string",            // Required: Event ID
    "EventName": "string",          // Required: Event name
    "StartDate": "string",          // Required: Start date (format: YYYY-MM-DD)
    "EndDate": "string",            // Required: End date (format: YYYY-MM-DD)
    "EventAddress": "string"        // Optional: Event address
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "EventId": "string",          // Updated event ID
      "Message": "string"           // Success or error message
    }
  }
  ```

### 27. Update Event Item

Updates an item within an event.

- **Endpoint**: `OrderMob/UpdateEventItem`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Required: Order ID
    "EventId": "string",            // Required: Event ID
    "ItemId": "string",             // Required for update: Item ID
    "ItemName": "string",           // Required: Item name
    "Quantity": number,             // Required: Quantity
    "Price": number,                // Required: Price per unit
    "ItemDescription": "string"     // Optional: Item description
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "ItemId": "string",           // Updated item ID
      "Message": "string"           // Success or error message
    }
  }
  ```

### 28. Delete Event Item

Deletes an item from an event.

- **Endpoint**: `OrderMob/DeleteEventItem`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Required: Order ID
    "EventId": "string",            // Required: Event ID
    "ItemId": "string"              // Required: Item ID to delete
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "Message": "string"           // Success or error message
    }
  }
  ```

### 29. Get Portion Exist With Item

Checks if portions exist for a specific item.

- **Endpoint**: `OrderMob/GetPortionExistWithItem`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetPortionExistWithItem?itemId={itemId}` 
    - `itemId`: Required: Item ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": [                       // Array of portions for the item
      {
        "PortionId": "string",      // Portion ID
        "PortionName": "string",    // Portion name
        "ItemId": "string",         // Item ID
        "Price": number,            // Price
        "Description": "string"     // Description
      }
    ]
  }
  ```

### 30. Update Item Description

Updates the description of an item within an event.

- **Endpoint**: `OrderMob/UpdateItemDescription`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Required: Order ID
    "EventId": "string",            // Required: Event ID
    "ItemId": "string",             // Required: Item ID
    "ItemDescription": "string"     // Required: Updated item description
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "Message": "string"           // Success or error message
    }
  }
  ```

### 31. Upload Item Image

Uploads an image for an item within an event.

- **Endpoint**: `OrderMob/UploadItemImage`
- **Method**: POST
- **Content-Type**: `multipart/form-data`
- **Parameters**:
  ```
  // Form Fields:
  OrderId: string              // Required: Order ID
  EventId: string              // Required: Event ID
  ItemId: string               // Required: Item ID
  
  // File Upload:
  ItemImage: file              // Required: Item image file (JPEG/PNG)
  ```
- **Headers**:
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "ImageUrl": "string",         // URL of the uploaded image
      "Message": "string"           // Success or error message
    }
  }
  ```

## Meeting APIs

### 32. Get Meetings

Retrieves a list of meetings with optional filtering.

- **Endpoint**: `Meeting/GetMeetings`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "StartDate": "string",       // Optional: Start date filter (format: YYYY-MM-DD)
    "EndDate": "string",         // Optional: End date filter (format: YYYY-MM-DD)
    "Search": "string",          // Optional: Search text for filtering meetings
    "PageNumber": number,        // Optional: Page number for pagination
    "PageSize": number           // Optional: Number of meetings per page
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `MeetingsBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": [                       // Array of meetings
      {
        "MeetingId": "string",      // Meeting ID
        "UserId": "string",         // User ID associated with the meeting
        "UserName": "string",       // User name
        "Title": "string",          // Meeting title
        "Date": "string",           // Meeting date
        "StartTime": "string",      // Start time
        "EndTime": "string",        // End time
        "Address": "string",        // Meeting location address
        "Description": "string",    // Meeting description
        "ContactPerson": "string",  // Contact person
        "ContactEmail": "string",   // Contact email
        "ContactPhone": "string",   // Contact phone
        "IsConfirmed": boolean,     // Whether meeting is confirmed
        "ConfirmationDate": "string", // Date of confirmation
        "CreatedBy": "string",      // User who created the meeting
        "CreatedDate": "string",    // Creation date
        "ModifiedBy": "string",     // User who last modified the meeting
        "ModifiedDate": "string"    // Last modification date
      }
    ],
    "TotalCount": number,           // Total count of meetings
    "PageNumber": number,           // Current page number
    "PageSize": number,             // Page size
    "TotalPages": number            // Total number of pages
  }
  ```

### 33. Get Meeting By Id

Retrieves detailed information for a specific meeting.

- **Endpoint**: `Meeting/GetMeetingById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/Meeting/GetMeetingById?meetingId={meetingId}` 
    - `meetingId`: Required: Meeting ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `MeetingByIdBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Meeting details
      "MeetingId": "string",        // Meeting ID
      "UserId": "string",           // User ID associated with the meeting
      "UserName": "string",         // User name
      "Title": "string",            // Meeting title
      "Date": "string",             // Meeting date
      "StartTime": "string",        // Start time
      "EndTime": "string",          // End time
      "Address": "string",          // Meeting location address
      "Latitude": number,           // Location latitude
      "Longitude": number,          // Location longitude
      "Description": "string",      // Meeting description
      "ContactPerson": "string",    // Contact person
      "ContactEmail": "string",     // Contact email
      "ContactPhone": "string",     // Contact phone
      "IsConfirmed": boolean,       // Whether meeting is confirmed
      "ConfirmationDate": "string", // Date of confirmation
      "CreatedBy": "string",        // User who created the meeting
      "CreatedDate": "string",      // Creation date
      "ModifiedBy": "string",       // User who last modified the meeting
      "ModifiedDate": "string",     // Last modification date
      "Attachments": [              // Array of meeting attachments
        {
          "AttachmentId": "string", // Attachment ID
          "FileName": "string",     // File name
          "FileUrl": "string",      // File URL
          "FileType": "string",     // File type
          "FileSize": number        // File size
        }
      ],
      "Notes": [                    // Array of meeting notes
        {
          "NoteId": "string",       // Note ID
          "NoteText": "string",     // Note text
          "CreatedBy": "string",    // User who created the note
          "CreatedDate": "string"   // Creation date
        }
      ]
    }
  }
  ```

### 34. Save Meeting

Creates or updates a meeting.

- **Endpoint**: `Meeting/SaveMeeting`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "MeetingId": "string",          // Optional: Meeting ID (omit for new meeting)
    "Title": "string",              // Required: Meeting title
    "Date": "string",               // Required: Meeting date (format: YYYY-MM-DD)
    "StartTime": "string",          // Required: Start time (format: HH:MM)
    "EndTime": "string",            // Required: End time (format: HH:MM)
    "Address": "string",            // Optional: Meeting location address
    "Latitude": number,             // Optional: Location latitude
    "Longitude": number,            // Optional: Location longitude
    "Description": "string",        // Optional: Meeting description
    "ContactPerson": "string",      // Optional: Contact person
    "ContactEmail": "string",       // Optional: Contact email
    "ContactPhone": "string",       // Optional: Contact phone
    "IsConfirmed": boolean,         // Optional: Whether meeting is confirmed
    "Notes": [                      // Optional: Array of meeting notes
      {
        "NoteText": "string"        // Note text
      }
    ]
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `MeetingsBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "MeetingId": "string",        // Created/updated meeting ID
      "Message": "string"           // Success or error message
    }
  }
  ```

### 35. Send Meeting Confirmation

Sends a confirmation for a meeting.

- **Endpoint**: `Meeting/SendMeetingConfirmation`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "MeetingId": "string"           // Required: Meeting ID
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `MeetingConfirmationBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "MeetingId": "string",        // Meeting ID
      "IsConfirmed": boolean,       // Whether confirmation was successful
      "ConfirmationDate": "string", // Date of confirmation
      "Message": "string"           // Success or error message
    }
  }
  ```

### 36. Update Meeting Exist

Checks if a meeting exists and if there are any conflicts.

- **Endpoint**: `Meeting/UpdateMeetingExist`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/Meeting/UpdateMeetingExist?meetingId={meetingId}` 
    - `meetingId`: Required: Meeting ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `MeetingByIdBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Updated meeting information
      "MeetingId": "string",        // Meeting ID
      "IsExist": boolean,           // Whether the meeting exists
      "IsConflict": boolean,        // Whether there's a scheduling conflict
      "ConflictReason": "string",   // Reason for conflict (if any)
      "ConflictMeetings": [         // Array of conflicting meetings (if any)
        {
          "MeetingId": "string",    // Conflicting meeting ID
          "Title": "string",        // Meeting title
          "Date": "string",         // Meeting date
          "StartTime": "string",    // Start time
          "EndTime": "string"       // End time
        }
      ],
      "Message": "string"           // Success or error message
    }
  }
  ```

## Comment APIs

### 37. Get Order Comments

Retrieves comments/remarks for a specific order.

- **Endpoint**: `OrderMob/GetOrderRemarkById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/OrderMob/GetOrderRemarkById?orderId={orderId}` 
    - `orderId`: Required: Order ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `CommentBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": [                       // Array of order comments/remarks
      {
        "RemarkId": "string",       // Remark/Comment ID
        "OrderId": "string",        // Order ID
        "RemarkText": "string",     // Comment text
        "RemarkType": "string",     // Comment type (e.g., "General", "Important")
        "CreatedBy": "string",      // User who created the comment
        "CreatedDate": "string",    // Creation date and time
        "UserName": "string",       // Username of comment creator
        "ProfilePic": "string"      // Profile picture URL of comment creator
      }
    ],
    "TotalCount": number           // Total count of comments
  }
  ```

### 38. Save Order Comment

Creates or updates a comment/remark for an order.

- **Endpoint**: `OrderMob/SaveOrderRemark`
- **Method**: POST
- **Parameters**:
  ```json
  {
    "OrderId": "string",            // Required: Order ID
    "RemarkId": "string",           // Optional: Remark ID (omit for new comment)
    "RemarkText": "string",         // Required: Comment text
    "RemarkType": "string"          // Optional: Comment type (e.g., "General", "Important")
  }
  ```
- **Headers**:
  - `Content-Type`: `application/json`
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**: `CommentBaseModel`
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {
      "RemarkId": "string",         // Created/updated comment ID
      "Message": "string"           // Success or error message
    }
  }
  ```

## Notification APIs

### 39. Get Notifications

Retrieves notifications for the authenticated user.

- **Endpoint**: `NotificationMob/GetNotifications`
- **Method**: GET
- **Parameters**: None (uses authorization token)
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,        // Success indicator (true/false)
    "Messages": ["string"],            // Array of response messages
    "Data": [                          // Array of notifications
      {
        "NotificationId": "string",    // Notification ID
        "UserId": "string",            // User ID the notification is for
        "Title": "string",             // Notification title
        "Message": "string",           // Notification message
        "NotificationType": "string",  // Type of notification (e.g., "Order", "Lead", "Followup")
        "ReferenceId": "string",       // Reference ID (e.g., OrderId, LeadId, FollowupId)
        "IsRead": boolean,             // Whether notification has been read
        "CreatedDate": "string",       // Creation date and time
        "ReadDate": "string",          // Date and time when notification was read
        "ImageUrl": "string"           // Optional image URL associated with notification
      }
    ],
    "UnreadCount": number              // Count of unread notifications
  }
  ```

### 40. Get Notification Lead Details

Retrieves details for a lead referenced in a notification.

- **Endpoint**: `Lead/GetLeadById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/Lead/GetLeadById?leadId={leadId}` 
    - `leadId`: Required: Lead ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,     // Success indicator (true/false)
    "Messages": ["string"],         // Array of response messages
    "Data": {                       // Lead details
      "LeadId": "string",           // Lead ID
      "CustomerName": "string",     // Customer name
      "CustomerPhone": "string",    // Customer phone
      "CustomerEmail": "string",    // Customer email
      "LeadSource": "string",       // Source of the lead
      "LeadStatus": "string",       // Status of the lead
      "LeadType": "string",         // Type of lead
      "EventType": "string",        // Event type
      "EventDate": "string",        // Event date
      "EstimatedBudget": number,    // Estimated budget
      "ExpectedGuests": number,     // Expected number of guests
      "Description": "string",      // Lead description
      "AssignedTo": "string",       // User ID lead is assigned to
      "AssignedToName": "string",   // Name of user lead is assigned to
      "CreatedBy": "string",        // User who created the lead
      "CreatedDate": "string",      // Creation date
      "ModifiedBy": "string",       // User who last modified the lead
      "ModifiedDate": "string",     // Last modification date
      "Notes": [                    // Array of lead notes
        {
          "NoteId": "string",       // Note ID
          "NoteText": "string",     // Note text
          "CreatedBy": "string",    // User who created the note
          "CreatedDate": "string",  // Creation date
          "UserName": "string"      // Username of note creator
        }
      ],
      "Activities": [               // Array of lead activities
        {
          "ActivityId": "string",   // Activity ID
          "ActivityType": "string", // Activity type
          "ActivityDate": "string", // Activity date
          "Description": "string",  // Activity description
          "CreatedBy": "string",    // User who created the activity
          "CreatedDate": "string"   // Creation date
        }
      ]
    }
  }
  ```

### 41. Get Notification Follow Up Details

Retrieves details for a follow-up referenced in a notification.

- **Endpoint**: `Followup/GetFollowupById`
- **Method**: GET
- **Parameters**: 
  - URL Parameter: `/Followup/GetFollowupById?followupId={followupId}` 
    - `followupId`: Required: Follow-up ID
- **Headers**:
  - `Accept`: `application/json`
  - `Authorization`: `Bearer {token}` // Required: Authentication token
  - `Language`: `English` or `Hebrew` // Optional: User's preferred language
  - `platformType`: `ios`             // Optional: Platform identifier
- **Returns**:
  ```json
  {
    "RequestResponse": boolean,       // Success indicator (true/false)
    "Messages": ["string"],           // Array of response messages
    "Data": {                         // Follow-up details
      "FollowupId": "string",         // Follow-up ID
      "ReferenceId": "string",        // Reference ID (e.g., LeadId, OrderId)
      "ReferenceType": "string",      // Reference type (e.g., "Lead", "Order")
      "FollowupDate": "string",       // Follow-up date
      "FollowupTime": "string",       // Follow-up time
      "FollowupType": "string",       // Type of follow-up
      "FollowupStatus": "string",     // Status of follow-up
      "Description": "string",        // Follow-up description
      "AssignedTo": "string",         // User ID follow-up is assigned to
      "AssignedToName": "string",     // Name of user follow-up is assigned to
      "CustomerName": "string",       // Customer name
      "CustomerPhone": "string",      // Customer phone
      "CustomerEmail": "string",      // Customer email
      "CreatedBy": "string",          // User who created the follow-up
      "CreatedDate": "string",        // Creation date
      "ModifiedBy": "string",         // User who last modified the follow-up
      "ModifiedDate": "string",       // Last modification date
      "CompletedBy": "string",        // User who completed the follow-up
      "CompletedDate": "string",      // Completion date
      "Notes": [                      // Array of follow-up notes
        {
          "NoteId": "string",         // Note ID
          "NoteText": "string",       // Note text
          "CreatedBy": "string",      // User who created the note
          "CreatedDate": "string",    // Creation date
          "UserName": "string"        // Username of note creator
        }
      ],
      "Reminders": [                  // Array of follow-up reminders
        {
          "ReminderId": "string",     // Reminder ID
          "ReminderDate": "string",   // Reminder date
          "ReminderTime": "string",   // Reminder time
          "Description": "string",    // Reminder description
          "IsCompleted": boolean,     // Whether reminder is completed
          "CreatedBy": "string",      // User who created the reminder
          "CreatedDate": "string"     // Creation date
        }
      ]
    }
  }
  ```

## Error Handling

All API endpoints may return error responses in the following format:

```json
{
  "RequestResponse": false,
  "Messages": ["Error message describing what went wrong"],
  "Data": null
}
```

Common HTTP status codes:
- 200: Successful operation
- 400: Bad request (invalid parameters)
- 401: Unauthorized (invalid or expired token)
- 403: Forbidden (insufficient permissions)
- 404: Resource not found
- 422: Validation error
- 500: Server error

## Rate Limiting

The API does not explicitly state rate limiting parameters. However, it's recommended to implement reasonable throttling in your application to prevent overloading the server.

## API Updates

This documentation is based on the current implementation as of the date of creation. As the API evolves, new endpoints may be added or existing ones modified. Always verify with the backend team if you encounter any discrepancies.
