# Wells Fargo – Investment Management Portfolio System

> Task 2 of the Wells Fargo Software Engineering Virtual Experience Program (Forage)

---

## Overview

This project implements the data model for a financial advisor portfolio management system built on the **Spring framework** with **JPA/Hibernate** for ORM and a **relational database** backend.

The system enables financial advisors to manage client portfolios containing various securities — tracking purchases, quantities, and pricing across a structured, normalized data model.

---

## Entity Relationship Diagram

```
FINANCIAL_ADVISOR  1 ──── N  CLIENT  1 ──── 1  PORTFOLIO  1 ──── N  SECURITY
```

| Relationship | Cardinality | Description |
|---|---|---|
| FinancialAdvisor → Client | One-to-Many | An advisor manages multiple clients |
| Client → Portfolio | One-to-One | Each client has exactly one portfolio |
| Portfolio → Security | One-to-Many | A portfolio holds zero or more securities |

---

## Entities

### `FinancialAdvisor`
| Field | Type | Notes |
|---|---|---|
| id | Long | PK, auto-generated |
| firstName | String | Required |
| lastName | String | Required |
| email | String | Required, unique |
| phone | String | Optional |
| clients | List\<Client\> | One-to-many, cascaded |

### `Client`
| Field | Type | Notes |
|---|---|---|
| id | Long | PK, auto-generated |
| firstName | String | Required |
| lastName | String | Required |
| email | String | Required, unique |
| phone | String | Optional |
| financialAdvisor | FinancialAdvisor | FK, many-to-one |
| portfolio | Portfolio | One-to-one, cascaded |

### `Portfolio`
| Field | Type | Notes |
|---|---|---|
| id | Long | PK, auto-generated |
| client | Client | FK, one-to-one |
| securities | List\<Security\> | One-to-many, cascaded |

### `Security`
| Field | Type | Notes |
|---|---|---|
| id | Long | PK, auto-generated |
| name | String | Required |
| category | String | Required |
| purchaseDate | LocalDate | Required |
| purchasePrice | BigDecimal | Required (precision-safe) |
| quantity | Integer | Required |
| portfolio | Portfolio | FK, many-to-one |

---

## Tech Stack

- **Java** — Spring Boot
- **Spring Data JPA** — ORM via Hibernate
- **javax.persistence** — Entity annotations
- **Relational Database** — H2 (dev) / configurable

---

## Project Structure

```
src/
└── main/
    └── java/
        └── com/wellsfargo/counselor/
            └── entity/
                ├── FinancialAdvisor.java
                ├── Client.java
                ├── Portfolio.java
                └── Security.java
```

---

## Setup & Run

```bash
# Clone the repo
git clone https://github.com/<your-username>/wells-fargo-task-2.git
cd wells-fargo-task-2

# Build and run
./mvnw spring-boot:run
```

---

## Design Decisions

- **`BigDecimal` for price** — avoids floating-point precision errors common with `double` for financial data
- **`LocalDate` for purchase date** — timezone-safe for date-only values
- **`CascadeType.ALL` + `orphanRemoval = true`** — deleting an advisor cascades to clients → portfolios → securities automatically
- **`GenerationType.IDENTITY`** — delegates ID generation to the database (auto-increment), compatible with most RDBMS
- **No setter on `id`** — IDs are immutable after creation per JPA best practice

---

## Author

**Abderrahim Nait Ali Mohamed**  
Data Scientist & Energy Engineer  
[LinkedIn](https://linkedin.com/in/nait-ali-mohamed-abderrahim-663a202a5) · [GitHub](https://github.com/Loki-01)
