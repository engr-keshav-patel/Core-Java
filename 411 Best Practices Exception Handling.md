# 🔥 Exception Handling Best Practices — INTERVIEW NOTES (IN-DEPTH)

---

## ✅ Definition

* Exception handling best practices are production-safe principles used to make code readable, recoverable, debuggable, and scalable.
* The goal is not just to catch exceptions, but to handle them at the correct layer with the correct recovery strategy.

### 📌 Simple 1-Line Explanation

* Good exception handling means catching the right exception at the right place with the right action.

> 👉 **Interview Tip:**
> A senior answer should always mention:
>
> * layer responsibility
> * centralized handling
> * logging strategy
> * domain exceptions
> * API consistency

---

## 🧠 Why It Is Important

* Prevents hidden bugs
* Improves debugging
* Keeps business code clean
* Avoids duplicate try-catch blocks
* Ensures consistent API responses
* Protects transaction integrity
* Critical for banking-grade backend systems

### 🏦 Banking Domain Relevance

#### Fund transfer

* rollback on DB failure
* return 409 for duplicate transaction

#### Loan service

* validation errors → 400
* fraud errors → 403

#### Payment gateway

* timeout → retry queue
* downstream failure → fallback response

> 🔥 **Important:**
> Bad exception handling in banking can cause:
>
> * duplicate debit
> * inconsistent ledger
> * hidden transaction failures
> * wrong customer status messages

---

## 🔹 Core Concepts

### 1) Should We Catch Generic Exception?

#### ❌ Usually No

Avoid:

```java
catch (Exception e)
```

#### Why Bad?

* hides real exception type
* poor recovery logic
* hard debugging
* mixes infra + business failures
* may accidentally swallow programming bugs

#### ❌ Bad Example

```java
try {
    transfer();
} catch (Exception e) {
    return "failed";
}
```

#### ✅ Better

```java
catch (InsufficientBalanceException e) { }
catch (SQLException e) { }
```

> 👉 **Interview Tip:**
> Catch generic Exception only in:
>
> * global handler
> * thread boundary
> * scheduler boundary
> * application startup boundary

### 2) Should We Log Or Rethrow Exception?

#### ✅ Golden Rule

* Handle + recover → log
* Cannot recover → rethrow
* Boundary layer → log once

#### ❌ Bad Practice

Logging in every layer:

* DAO → Service → Controller

This creates:

* duplicate logs
* noisy Kibana dashboards
* confusing traces

#### ✅ Best Practice

* DAO → translate exception
* Service → business recovery
* Global handler → final logging + response

> 🔥 **Important:**
> Log once at the boundary, not in every layer.

### 3) Should Business Logic Handle Exceptions?

#### ✅ Yes, But Only Business Exceptions

Business layer should handle:

* rollback
* compensation
* retry decision
* fallback
* status update

### 🏦 Banking Example

* DAO throws DB timeout
* Service decides:

  * rollback transfer
  * move to retry queue
  * notify reconciliation job

#### ❌ Business Layer Should NOT

* build HTTP response

* expose stack trace

* convert to JSON directly

* That belongs to controller/global handler.

---

## 🔍 Interview Follow-Up Questions

### ❓ Exception Handling In Controller vs Service vs DAO

* This is a very important layered architecture question.

| Layer          | Responsibility                    |
| -------------- | --------------------------------- |
| Controller     | Request validation + HTTP mapping |
| Service        | Business recovery + rollback      |
| DAO            | Persistence exception translation |
| Global Handler | Unified API response              |

#### ✅ Best Layering Flow

```text
DAO → Service → ControllerAdvice
```

### 🏦 Banking Example

* repository → throws SQLException

* service → rollback debit

* @ControllerAdvice → return 500 / 409 / 400

* This centralized approach is the most common Spring production pattern.

### ❓ Global Exception Handling in Spring (@ControllerAdvice)

* In Spring Boot, best practice is to use:

```java
@ControllerAdvice
```

* for centralized exception handling across controllers.

#### ✅ Why Best?

* removes repeated try-catch in controllers
* standardizes error response
* maps HTTP status cleanly
* keeps controller clean
* easier monitoring
* works well with custom exceptions

#### Example

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(InsufficientBalanceException.class)
    public ResponseEntity<String> handleBalance(InsufficientBalanceException ex) {
        return ResponseEntity.badRequest().body(ex.getMessage());
    }
}
```

> 👉 **Interview Tip:**
> Say:
> “Controllers should stay thin; global exception handling keeps API contracts consistent.”

### ❓ Wrapping Low-Level Exceptions

#### ✅ Yes, This Is Best Practice

* Convert infra exceptions into domain-friendly exceptions.

#### Example

```java
try {
    repository.save(tx);
} catch (SQLException e) {
    throw new TransactionFailedException("DB failure during transfer", e);
}
```

#### Why Best?

* hides infra details
* preserves business meaning
* improves API response mapping
* keeps service language domain-driven

### 🏦 Banking Example

* SQL deadlock
* wrap into:

  * DuplicateTransactionException

> 🔥 **Important:**
> Always preserve original cause:

```java
new MyException(msg, e)
```

---

## 💻 Code Example

### ✅ Production-Ready Spring Layered Example

```java
@Service
public class TransferService {

    public void transfer(double amount, double balance) {
        if (amount > balance) {
            throw new InsufficientBalanceException("Insufficient funds");
        }

        try {
            // DAO call
            saveTransaction();
        } catch (RuntimeException e) {
            throw new TransactionFailedException("Transfer failed", e);
        }
    }

    private void saveTransaction() {
        throw new RuntimeException("DB timeout");
    }
}
```

#### Why This Is Correct

* domain exception for validation
* low-level exception wrapped
* root cause preserved
* clean service API
* global handler friendly

---

## 🌍 Real-World Examples

### 🏦 Banking

* duplicate UPI request
* DuplicateTransactionException
* insufficient funds
* InsufficientBalanceException
* settlement file issue
* SettlementFileException

### 🏥 Healthcare

* duplicate patient
* PatientAlreadyExistsException
* doctor unavailable
* DoctorUnavailableException
* EMR save failure
* wrapped persistence exception

### 💳 Payments

* gateway timeout
* retryable domain exception
* fraud rule failure
* custom unchecked exception

---

## ⚠️ Common Interview Traps

### ❌ Trap 1: Swallowing Exceptions

```java
catch (Exception e) {}
```

* Major production anti-pattern.

### ❌ Trap 2: Logging Same Exception 3 Times

* Creates duplicate alerts.

### ❌ Trap 3: Returning HTTP From Service Layer

* Wrong separation of concerns.

### ❌ Trap 4: Exposing DB Error To Client

* Never leak:

  * SQL
  * table names
  * stack traces
  * internal hostnames

---

## 🚀 Best Practices

* catch specific exceptions
* log once at boundary
* wrap low-level exceptions
* use domain exceptions
* use @ControllerAdvice
* keep controller thin
* service owns rollback
* DAO owns translation
* preserve root cause
* map meaningful HTTP codes
* include requestId / traceId in logs
* use RFC 7807 style error body where possible

### 🏦 Banking Production Rule

For transaction systems:

* rollback on service exceptions
* log audit trail once
* avoid duplicate retry logs
* return deterministic error codes

---

## 🎯 Interview-Ready Final Answer

* Best practice is to catch specific exceptions, handle them at the correct architectural layer, and log only once at the boundary.
* Controllers should stay thin, services should handle business recovery and rollback, and DAOs should translate persistence failures.
* In Spring Boot, @ControllerAdvice is the preferred way to centralize API exception handling and keep responses consistent.
* Low-level exceptions should be wrapped into domain-specific exceptions while preserving the root cause.
* In banking systems, this design prevents duplicate logs, protects transaction integrity, and ensures predictable API contracts.
