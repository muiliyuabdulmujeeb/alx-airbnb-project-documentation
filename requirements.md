#  Backend Requirement Specifications – Airbnb Clone

This document defines the technical and functional requirements for the backend features of the Airbnb Clone project.  
It covers **User Management**, **Property Listings Management**, and **Search & Filtering**.

---

## 1.  User Management

### Functional Requirements
- Users can register as **Guests** or **Hosts**.
- Secure authentication using **JWT (JSON Web Tokens)**.
- Login with **email & password** or via **OAuth (Google, Facebook)**.
- Users can update their profiles, including:
  - Profile photo
  - Contact info
  - Preferences

### API Endpoints
| Method | Endpoint              | Description                  | Input                                   | Output |
|--------|-----------------------|------------------------------|-----------------------------------------|--------|
| POST   | `/api/v1/auth/register` | Register a new user          | `{name, email, password, role}`         | `{userId, token}` |
| POST   | `/api/v1/auth/login`    | Login user                   | `{email, password}`                     | `{token, user}` |
| GET    | `/api/v1/users/:id`     | Get user profile             | `id` (path param)                       | `{user}` |
| PUT    | `/api/v1/users/:id`     | Update user profile          | `{profile fields}`                      | `{updatedUser}` |

### Validation Rules
- Email must be unique and valid format.
- Password must be ≥ 8 characters with alphanumeric characters.
- JWT tokens expire after a set time (e.g., 1h).
- Profile photo must be an image (jpeg/png, ≤ 5MB).

### Performance Criteria
- Registration and login requests should complete in ≤ 300ms under normal load.
- System must support at least 1000 concurrent logins.

---

## 2.  Property Listings Management

### Functional Requirements
- Hosts can **create, edit, and delete** property listings.
- Each listing contains:
  - Title, description, location
  - Price per night
  - Amenities (Wi-Fi, pool, parking, etc.)
  - Availability (dates)
- Only the host who created a listing can modify or delete it.

### API Endpoints
| Method | Endpoint               | Description                 | Input                                   | Output |
|--------|------------------------|-----------------------------|-----------------------------------------|--------|
| POST   | `/api/v1/properties`    | Create a property listing   | `{title, description, location, price, amenities, availability}` | `{propertyId}` |
| GET    | `/api/v1/properties/:id`| Get property details        | `id` (path param)                       | `{property}` |
| PUT    | `/api/v1/properties/:id`| Update property listing     | `{updated fields}`                      | `{updatedProperty}` |
| DELETE | `/api/v1/properties/:id`| Delete property listing     | `id` (path param)                       | `{message}` |

### Validation Rules
- Title ≤ 100 characters.
- Price must be positive numeric value.
- Availability must follow `YYYY-MM-DD` format.
- Amenities should be from a predefined list.

### Performance Criteria
- API should handle at least 500 property CRUD ops per minute.
- Query results must return within ≤ 500ms.

---

## 3.  Search and Filtering

### Functional Requirements
- Users can search properties by:
  - Location
  - Price range
  - Number of guests
  - Amenities
- Include **pagination** for large datasets.
- Results sorted by **relevance** or **price**.

### API Endpoints
| Method | Endpoint               | Description                | Input                                   | Output |
|--------|------------------------|----------------------------|-----------------------------------------|--------|
| GET    | `/api/v1/properties`   | Search/filter properties   | Query params: `location, priceMin, priceMax, guests, amenities, page, limit` | `{results, pagination}` |

### Validation Rules
- Location is required.
- Price range must be numeric and logical (`min ≤ max`).
- Guests must be integer > 0.
- Page/limit must be integers with default fallback.

### Performance Criteria
- Must support querying **10,000+ listings** with pagination.
- Response time ≤ 700ms under standard load.
- Indexing on location, price, and amenities to improve search speed.

---

##  Summary
This requirements document ensures the backend is:
- **Secure** (JWT, OAuth, validation rules),
- **Scalable** (pagination, query optimization),
- **User-friendly** (clear endpoints, fast response times),
- **Maintainable** (modular design, role-based access).