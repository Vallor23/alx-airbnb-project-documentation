# 📄 Backend Requirements — Airbnb Clone

This document outlines the functional and technical specifications for three major backend features of the Airbnb Clone project: User Authentication, Property Management, and Booking System.

---

## 1. 🔐 User Authentication

### ✅ Functional Requirements

- Users must be able to register using their name, email, phone number, and password.
- Registered users can log in using their credentials.
- Token-based authentication (JWT) must be used for session handling.
- Passwords must be securely hashed.
- Login failures (e.g., wrong password) must return meaningful errors.

### 🔧 Technical Specifications

**API Endpoints:**

| Method | Endpoint           | Description          |
|--------|--------------------|----------------------|
| POST   | `/api/register`    | Register new user    |
| POST   | `/api/login`       | Log in a user        |

**Input/Output:**

`POST /api/register`  
**Request Body:**

```json
{
  "first_name": "Alice",
  "last_name": "Smith",
  "email": "alice@example.com",
  "phone_number": "+254712345678",
  "password": "mypassword"
}
```

**Validations:**

- Email must be unique and valid.
- Password must be at least 8 characters.
- All fields are required.

**Response:**

```json
{
  "message": "Registration successful",
  "user": { "id": 1, "email": "alice@example.com" }
}
```

`POST /api/login`  
**Request Body:**

```json
{
  "email": "alice@example.com",
  "password": "mypassword"
}
```

**Response:**

```json
{
  "token": "jwt_token_here",
  "user": { "id": 1, "role": "user" }
}
```

**Performance Criteria:**

- Login and registration requests must complete in < 500ms.
- Password hashing using bcrypt with at least 10 salt rounds.

---

## 2. 🏠 Property Management

- Hosts can add new property listings.
- Properties must include location, name, description, price, and availability.
- Admins can approve, reject, or remove listings.

### 🔧 Technical Specifications

**API Endpoints:**

| Method | Endpoint              | Description              |
|--------|-----------------------|--------------------------|
| POST   | `/api/properties`     | Host adds a property     |
| GET    | `/api/properties`     | Retrieve property list   |
| PUT    | `/api/properties/:id` | Update property details  |
| DELETE | `/api/properties/:id` | Delete a property        |

**Input/Output:**

`POST /api/properties`  
**Request Body:**

```json
{
  "host_id": 2,
  "name": "Modern Studio in Westlands",
  "description": "Perfect for business travelers",
  "location": "Nairobi",
  "price_per_night": 180.00
}
```

**Validations:**

- Name and location must not be empty.
- Price must be a positive number.
- Host must be authenticated.

**Response:**

```json
{
  "message": "Property listed successfully",
  "property": { "id": 10, "location": "Nairobi" }
}
```

**Performance Criteria:**

- Retrieval of all properties should support pagination (default 10 per page).
- Response time < 700ms for search queries.

---

## 3. 📅 Booking System

### ✅ Functional Requirements

- Guests can book properties for specific dates.
- Bookings must check availability.
- Once a booking is made, availability is blocked.
- Payments are associated with bookings.

### 🔧 Technical Specifications

**API Endpoints:**

| Method | Endpoint             | Description              |
|--------|----------------------|--------------------------|
| POST   | `/api/bookings`      | Create new booking       |
| GET    | `/api/bookings/:id`  | Get booking details      |

**Input/Output:**

`POST /api/bookings`  
**Request Body:**

```json
{
  "guest_id": 1,
  "property_id": 5,
  "start_date": "2025-06-01",
  "end_date": "2025-06-05"
}
```

**Validations:**

- Dates must not overlap existing bookings.
- End date must be after start date.
- Guest must be authenticated.

**Response:**

```json
{
  "message": "Booking confirmed",
  "booking": {
    "id": 22,
    "property_id": 5,
    "guest_id": 1,
    "total_price": 600.00
  }
}
```

**Performance Criteria:**

- Check availability and respond in < 800ms.
- Payment and booking must occur in a transactional operation to prevent double booking.

---

## 📌 Notes

- All endpoints must return standardized error messages.
- Use appropriate HTTP status codes (`201 Created`, `400 Bad Request`, `401 Unauthorized`, etc.).
- Secure endpoints must use token authentication (`Authorization: Bearer <token>`).