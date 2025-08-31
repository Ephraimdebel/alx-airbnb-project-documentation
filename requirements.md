# Backend Requirement Specifications

This document outlines the technical and functional requirements for three core backend features of the Airbnb project.

---

## 1. User Authentication

### Description
The authentication system manages user registration, login, and session handling to ensure secure access.

### API Endpoints
- **POST /api/auth/register**
  - Input: `{ username, email, password }`
  - Output: `{ success: true, message: "User registered successfully" }`
- **POST /api/auth/login**
  - Input: `{ email, password }`
  - Output: `{ token, user: { id, username, email } }`
- **GET /api/auth/profile**
  - Input: `Authorization: Bearer <token>`
  - Output: `{ id, username, email }`

### Validation Rules
- `email`: must be unique and valid format.
- `password`: at least 8 characters, must include letters and numbers.
- `username`: required, minimum 3 characters.

### Performance Criteria
- Registration and login should respond in < 2 seconds.
- Token-based authentication (JWT) with expiration time.

---

## 2. Property Management

### Description
Hosts can create, update, and manage their property listings.

### API Endpoints
- **POST /api/properties**
  - Input: `{ title, description, location, price, images[], hostId }`
  - Output: `{ id, title, description, location, price, images[], hostId }`
- **PUT /api/properties/:id**
  - Input: `{ title?, description?, location?, price?, images? }`
  - Output: `{ success: true, updatedProperty }`
- **GET /api/properties/:id**
  - Input: `propertyId`
  - Output: `{ id, title, description, location, price, images[], hostId }`
- **DELETE /api/properties/:id**
  - Input: `propertyId`
  - Output: `{ success: true, message: "Property deleted" }`

### Validation Rules
- `title`: required, max 100 characters.
- `price`: must be numeric and > 0.
- `location`: required string.
- `images`: optional but must be valid URLs.

### Performance Criteria
- Must support search and filter queries efficiently.
- Property creation should respond in < 3 seconds.

---

## 3. Booking System

### Description
Allows users to book properties for specific dates and manages availability.

### API Endpoints
- **POST /api/bookings**
  - Input: `{ propertyId, userId, startDate, endDate }`
  - Output: `{ id, propertyId, userId, startDate, endDate, status }`
- **GET /api/bookings/:id**
  - Input: `bookingId`
  - Output: `{ id, propertyId, userId, startDate, endDate, status }`
- **DELETE /api/bookings/:id**
  - Input: `bookingId`
  - Output: `{ success: true, message: "Booking canceled" }`

### Validation Rules
- `startDate` and `endDate` must be valid dates.
- `endDate` must be after `startDate`.
- Property must be available (no overlapping booking).

### Performance Criteria
- Double booking should be prevented using transaction locks.
- Booking creation should respond in < 3 seconds.

---

## General Notes
- All APIs must follow RESTful design principles.
- Responses should be in JSON format.
- Error handling must return appropriate HTTP status codes (400, 401, 404, 500).
