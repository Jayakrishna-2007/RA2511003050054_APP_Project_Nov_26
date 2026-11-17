# ✈️ Airplane Ticket Booking Management System

**B.Tech Computer Science and Engineering Academic Project**

---

## ✈️ 1. Project Overview

The **Airplane Ticket Booking Management System (AirLink)** is an enterprise-grade full-stack flight reservation system developed for B.Tech CSE academic evaluations. It simulates a commercial airline reservation platform featuring flight search with dynamic filters, real-time aircraft seat map selection, passenger manifests, simulated multi-rail payment processing (UPI, Credit/Debit Cards, Net Banking), instantaneous 6-character PNR generation, digital e-ticket generation, booking cancellations with automated inventory release, and a comprehensive role-based administrator dashboard.

---

## 🎓 2. Academic & Team Details

### Course Information

| Field | Details |
|---|---|
| Course Code | 21CSC203P |
| Course Name | Advanced Programming Practice |
| Academic Year | 2026–2027 |
| Institution | SRM Institute of Science and Technology |
| Campus | Tiruchirappalli Campus |

### Team Members

| S.No. | Full Name | Register Number | Department | Section | Academic Year |
|---|---|---|---|---|---|
| 1 | M. Jayakrishna | RA2511003050054 | CSE | II CSE-A | 2026–2027 |
| 2 | M. Siddharth | RA2511003050030 | CSE | II CSE-A | 2026–2027 |
| 3 | K. Jai Akaash | RA2511003050010 | CSE | II CSE-A | 2026–2027 |

### Mentor Details

| Field | Details |
|---|---|
| Mentor Name | Dr. S. Shargunam |
| Designation | Subject Handling Faculty |

---

## 🏛️ 3. Architecture & Layered Design

The system implements the **Layered Architectural Pattern** following Spring Boot best practices:

```text
[ Client / React Frontend ] (Single Page Application)
          │  REST APIs (JSON / JWT Bearer)
          ▼
[ Controller Layer ] (@RestController, Request Validation, HTTP Response Mapping)
          │  DTOs & Model entities
          ▼
[ Service Layer ] (Business Logic, Transaction Management, Polymorphic Payment)
          │  Spring Data Interfaces
          ▼
[ Repository Layer ] (Spring Data JPA, Hibernate ORM, Custom JPQL Queries)
          │  JDBC / Connection Pooling
          ▼
[ Database Layer ] (H2 in-memory for testing / MySQL for production)
```

---

## 🧩 4. Object-Oriented Programming (OOP) Demonstration

During project defense and viva, this codebase demonstrates core OOP paradigms:

### 1. Encapsulation

- All entity fields (`User`, `Flight`, `Booking`, `Seat`, etc.) use private access modifiers with public getters and setters.
- Sensitive fields like passwords are annotated with `@JsonIgnore` and hashed via BCrypt before persistence.
- Currency computations are encapsulated inside `FareBreakdownDto` with `BigDecimal` rounding scales.

### 2. Inheritance

- `Admin.java` extends `User.java` using JPA Single Table Inheritance (`@DiscriminatorColumn(name = "user_type")`).
- Specializes administrative attributes (e.g., `adminDepartment`, `employeeCode`) while inheriting authentication credentials and profile information.

### 3. Abstraction

- The `PaymentService` interface exposes high-level contracts `processPayment()` and `processRefund()`.
- Controllers and booking services interact only with the `PaymentService` abstraction, decoupled from specific gateway implementation details.

### 4. Polymorphism

- Multiple concrete implementations:
  - `UpiPaymentService` (validates VPA format, simulates UPI tokenization)
  - `CardPaymentService` (validates card length, expiration, CVV, masks PAN)
  - `NetBankingPaymentService` (validates partner banking institution)
- Dynamically resolved at runtime using `PaymentServiceFactory`.

---

## 🗄️ 5. Database Schema & Entities

| **Entity** | **Primary Key** | **Key Relationships** | **Purpose** |
|---|---|---|---|
| **User** | `id` (Long) | Base class for `Admin` | Registered customer accounts |
| **Admin** | `id` (Long) | Inherits from `User` | Privileged system administrators |
| **Flight** | `id` (Long) | `@OneToMany` Seats | Schedules, routes, multi-tier fares |
| **Seat** | `id` (Long) | `@ManyToOne` Flight | Aircraft cabin seat map (1A - 30F) |
| **Booking** | `id` (Long) | `@ManyToOne` User, `@ManyToOne` Flight, `@OneToMany` Passengers, `@OneToOne` Payment, `@OneToOne` Ticket | Confirmed reservation record with unique PNR |
| **Passenger** | `id` (Long) | `@ManyToOne` Booking | Individual traveler passport & seat info |
| **Payment** | `id` (Long) | `@OneToOne` Booking | Simulated transaction logs & audit trail |
| **Ticket** | `id` (Long) | `@OneToOne` Booking | Issued digital boarding pass & e-ticket |

---

## 🚀 6. How to Run the Java Backend Locally

### Prerequisites

- JDK 17 or later
- Apache Maven 3.8+
- (Optional) MySQL 8.0+

### Steps

Navigate to the backend folder:

```bash
cd backend
```

Compile and package the application:

```bash
mvn clean package -DskipTests
```

Run the Spring Boot application:

```bash
mvn spring-boot:run
```

The backend starts on:

```text
http://localhost:8080
```

- **H2 Console:** `http://localhost:8080/h2-console`
- **JDBC URL:** `jdbc:h2:mem:airbookingdb`
- **Username:** `sa`
- **Password:** Empty
- **Swagger OpenAPI Docs:** `http://localhost:8080/swagger-ui/index.html`

### Switching to MySQL

Open `backend/src/main/resources/application.properties` and configure your local MySQL credentials:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/air_booking_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
```

---

## 🛡️ 7. Default Credentials for Viva Demonstration

The following credentials are provided for demonstration purposes:

| Role | Email | Password |
|---|---|---|
| Administrator | admin@airbooking.com | Password@123 |
| Passenger | user@airbooking.com | Password@123 |

Self-registration is also supported via:

```text
/api/auth/register
```

**Note:** These are demonstration credentials. Do not use default credentials in a production environment.

---

## 🏫 Institution

**SRM Institute of Science and Technology**  
Tiruchirappalli Campus

**Department:** Computer Science and Engineering

---

*Developed as part of the Advanced Programming Practice course (21CSC203P) at SRM Institute of Science and Technology, Tiruchirappalli Campus.*

**Project: AirLink – Airplane Ticket Booking Management System**
