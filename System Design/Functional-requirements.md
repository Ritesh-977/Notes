# Functional Requirements (FRs) — System Design Interview Notes

In **system design interviews**, functional requirements describe **what the system should do** — the features, actions, and business operations it must support.

For example, in a hotel booking system, users should be able to search hotels, check room availability, book rooms, make payments, and cancel reservations.

Unlike non-functional requirements (NFRs), which describe *how well* a system performs, functional requirements define **what functionality the system must provide**.

---

## 1. Common Functional Requirements

### 1.1 User Authentication & Authorization

- **What it means:** Users can register, log in, log out, and access features according to their roles.
- **What it drives:**
  - Authentication APIs and session management
  - JWT, OAuth 2.0
  - User database
  - Role-Based Access Control (RBAC)

**Example:** A hotel customer can book rooms, but only an admin can manage hotel listings.

### 1.2 Create, Read, Update, Delete (CRUD)

- **What it means:** Users can create, view, modify, and delete resources.
- **What it drives:**
  - REST APIs (`POST`, `GET`, `PUT/PATCH`, `DELETE`)
  - Database schema and relationships
  - Input validation
  - Authorization checks

**Example:** Users can create and edit their profiles, and admins can update hotel information.

### 1.3 Search & Filtering

- **What it means:** Users can search for information and narrow down results using filters.
- **What it drives:**
  - Database indexes
  - Search engines such as Elasticsearch or OpenSearch
  - Filtering and sorting logic
  - Pagination

**Example:** Users can search hotels by city, price, rating, and check-in date.

### 1.4 Booking & Reservation Management

- **What it means:** Users can reserve resources for a specific time or period.
- **What it drives:**
  - Availability checking
  - Booking APIs and reservation states
  - Database transactions
  - Concurrency control to prevent double-booking

**Example:** If two users try to book the same room for the same dates, the system must prevent both bookings from succeeding.

### 1.5 Payment Processing

- **What it means:** Users can pay for services or products and receive payment confirmation.
- **What it drives:**
  - Payment gateway integration
  - Payment records and transaction states
  - Idempotency keys to avoid duplicate charges
  - Webhooks for payment status updates

**Example:** A booking becomes confirmed only after payment succeeds, according to the defined business rules.

### 1.6 Notifications

- **What it means:** The system informs users about important events or changes.
- **What it drives:**
  - Email, SMS, and push notification services
  - Event-driven architecture
  - Message queues such as Kafka or RabbitMQ
  - Notification templates and delivery tracking

**Example:** A user receives an email after successfully booking a hotel.

### 1.7 User Activity & History

- **What it means:** Users can view their previous activities and transactions.
- **What it drives:**
  - History tables or event records
  - Query APIs
  - Pagination and sorting
  - Audit logs where required

**Example:** Users can view their previous bookings, payments, and cancellations.

### 1.8 Cancellation & Refunds

- **What it means:** Users can cancel eligible operations and receive refunds according to business rules.
- **What it drives:**
  - Cancellation policies
  - State transitions
  - Refund APIs and payment gateway integration
  - Transaction tracking

**Example:** A user cancels a hotel booking and receives a full or partial refund depending on the cancellation policy.

### 1.9 Admin & Management Operations

- **What it means:** Administrators can manage system resources, users, and business operations.
- **What it drives:**
  - Admin APIs and dashboards
  - Role-based permissions
  - Reporting and analytics
  - Audit trails

**Example:** Hotel administrators can add rooms, update prices, and block unavailable dates.

---

## 2. Functional Requirements You Should Identify

There is no fixed list that applies to every system. Start by identifying the system's core user journeys.

For most consumer applications, consider these categories:

1. **User operations** — registration, login, profiles.
2. **Core business functionality** — the main purpose of the system.
3. **Search and discovery** — search, filters, sorting.
4. **Data management** — create, read, update, delete.
5. **Transactions** — payments, orders, bookings, where applicable.
6. **Notifications** — confirmations and status updates.
7. **History and tracking** — previous actions and current status.
8. **Admin operations** — management and moderation, where needed.

**Interview tip:** Don't blindly include every feature. Select requirements based on the system you're designing and the time available.

---

## 3. Example Mapping: Hotel Booking System

| Functional requirement | What the system must do | Possible design components |
|---|---|---|
| Hotel search | Search by location, date, and price | Search service, indexes |
| Availability check | Show rooms available for selected dates | Inventory service, database queries |
| Room booking | Reserve a room | Booking service, transactions |
| Payment | Process payment | Payment service, payment gateway |
| Cancellation | Cancel eligible bookings | Booking state machine, refund service |
| Notifications | Send booking confirmation | Queue, notification service |
| Booking history | Show past and upcoming bookings | Booking database, history API |
| Admin management | Manage hotels, rooms, and prices | Admin APIs, authorization |

Remember: **functional requirements define behavior**, while the components in the final column are possible design choices used to implement that behavior.

---

## 4. Functional Requirements → Design Choices Cheat Sheet

### 4.1 Authentication & User Management

- `POST /register` — create an account.
- `POST /login` — authenticate a user.
- `POST /logout` — end a session.
- `GET /users/{id}` — retrieve a profile.
- Possible components: JWT, OAuth, session management, user database.

### 4.2 Search & Discovery

- `GET /hotels?city=Lucknow` — search hotels.
- Filter by price, rating, location, or availability.
- Sort results and paginate them.
- Possible components: database indexes, Elasticsearch/OpenSearch, caching when useful.

### 4.3 Core Business Operations

- Define the primary resource: product, post, message, booking, or order.
- Identify its lifecycle and valid state transitions.
- Design APIs and database tables around those operations.
- Use transactions and concurrency control where required.

### 4.4 Payments & Transactions

- Initiate a payment.
- Confirm or reject the payment.
- Track transaction status.
- Handle duplicate requests safely.
- Possible components: payment gateway, idempotency keys, webhooks, transaction records.

### 4.5 Notifications & Events

- Notify users when important events occur.
- Support email, SMS, or push notifications as needed.
- Use asynchronous queues when delivery should not delay the main request.
- Track failures and retry delivery safely.

### 4.6 History & Tracking

- Retrieve previous orders, bookings, or messages.
- Display current status and timestamps.
- Support pagination and filtering.
- Possible components: indexed queries, history tables, audit logs.

### 4.7 Admin & Moderation

- Create, update, or disable resources.
- Manage users and permissions.
- Review reports and resolve disputes.
- Possible components: admin APIs, RBAC, audit logging.

---

## 5. Complete Example: Hotel Booking System

1. **Search hotels:** Users can search by destination, check-in/check-out dates, number of guests, and room preferences.
2. **Check availability:** The system displays available rooms and their prices for the selected dates.
3. **Create a booking:** Authenticated users can reserve an available room and receive a booking ID.
4. **Process payment:** Users can pay for bookings and view the payment status.
5. **Manage bookings:** Users can view, modify where permitted, or cancel their reservations.
6. **Send notifications:** The system sends booking confirmations, cancellation updates, and payment notifications.
7. **Manage the platform:** Hotel administrators can manage listings, room inventory, pricing, and reservations.

---

## 6. How Functional Requirements Influence System Design

A typical flow is:

1. **Functional requirements** — What features must the system support?
2. **API design** — What endpoints, requests, and responses are needed?
3. **Data model and business logic** — What entities, relationships, rules, and state transitions are required?
4. **Architecture and components** — What services, databases, queues, and caches may be needed?

For example, the requirement **“Users can book rooms”** leads to questions such as:

- What APIs create and retrieve bookings?
- How are rooms and reservations represented in the database?
- How do we prevent two users from booking the same room?
- How do we handle payment failure?
- How can the booking service scale during peak demand?

These questions connect functional requirements to the actual system design.

---

## 7. Functional Requirements vs. Non-Functional Requirements

| Functional requirements (FRs) | Non-functional requirements (NFRs) |
|---|---|
| What should the system do? | How well should it work? |
| Search for hotels | Search results return within 200 ms |
| Create a booking | Support 50,000 booking requests per minute |
| Process payments | Prevent duplicate charges and protect payment data |
| Send notifications | Deliver 99% of notifications within a defined time |
| View booking history | Remain available 99.99% of the time |

*The numbers above are illustrative targets, not universal requirements.*

---

## 8. Interview-Ready Answer

> Functional requirements define the core features and business operations a system must support. For a hotel booking system, these include searching hotels, checking availability, creating bookings, processing payments, cancelling reservations, and sending notifications. Once I identify and prioritize these requirements, I use them to define the APIs, data models, business logic, and major system components. After that, I establish the non-functional requirements to guide decisions about scalability, availability, latency, and consistency.

---

## Key Takeaway

In a system design interview, first clarify **what the system must do**, then determine **how well it must do it**, and finally design an architecture that satisfies both.
