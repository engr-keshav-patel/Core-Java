# 🔥 COMMON EXCEPTION HANDLING SCENARIO QUESTIONS (INTERVIEW FAVORITES) — MASTER REVISION NOTES

---

## ✅ Golden Interview Formula (Must Follow for Every Scenario)

* Whenever interviewer asks any exception handling scenario, answer in this order:

### 🎯 4-Step Formula

1. Where exception occurs
2. Which layer should handle it
3. Why that layer is correct
4. What final response / client impact will be

> 👉 **Interview Tip:**
> This makes your answer sound structured, senior, and architecture-aware.

---

## 1️⃣ Null Value Scenario

### ❓ Question

* A method receives an object parameter. Sometimes it is null.
* How will you handle this?

### ✅ Expected Answer

* Validate input at method boundary
* Fail fast
* Throw meaningful unchecked exception
* Prefer IllegalArgumentException for invalid caller input

```java
if (user == null) {
    throw new IllegalArgumentException("User cannot be null");
}
```

### 💡 Why This Is Best

* boundary validation
* clear API contract
* easier debugging
* avoids deep NPE later
* fail-fast design

### 🏥 Healthcare Example

```java
if (patient == null) {
    throw new IllegalArgumentException("Patient request cannot be null");
}
```

### 🔍 Follow-Up

#### ❓ Why not allow NullPointerException?

* NPE happens later
* poor business meaning
* harder debugging
* wrong abstraction level

#### ❓ Why IllegalArgumentException and not checked?

* caller misuse
* validation failure
* no need to force boilerplate handling

---

## 2️⃣ Service → DAO → DB Exception

### ❓ Question

* An exception occurs in DAO layer while accessing DB.
* Where should it be handled?

### ✅ Expected Answer

* DAO → throw persistence exception
* Service → translate / wrap
* Controller → return response
* Global handler → final HTTP mapping

```java
catch (SQLException e) {
    throw new DataAccessException("DB failure", e);
}
```

### 🏦 Banking Layer Flow

* DAO → SQLException
* Service → TransactionFailedException
* ControllerAdvice → 500 / 409

### 💡 Follow-Up

#### ❓ Why not handle in DAO?

* DAO should stay persistence-focused
* no business rollback knowledge
* no API context

#### ❓ Why wrap low-level exception?

* hide SQL details
* preserve domain language
* cleaner API contracts

---

## 3️⃣ Exception in finally Block

### ❓ Question

* What happens if an exception occurs in finally?

### ✅ Expected Answer

* original exception gets hidden
* finally exception becomes final visible exception

```java
try {
    throw new RuntimeException("Original");
} finally {
    throw new IllegalStateException("Cleanup failed");
}
```

### ✅ Output

```java
IllegalStateException
```

### 💡 Best Practice

* never throw from finally
* cleanup code must be safe
* use try-with-resources where possible

> 🔥 **Important:**
> This is a top output-based interview trap.

---

## 4️⃣ Return in try and finally

### ❓ Question

```java
try {
    return 1;
} finally {
    return 2;
}
```

### ✅ Expected Answer

#### ✅ Output

```java
2
```

#### Why?

* try stores 1
* finally executes before method exit
* return 2 overrides stored value

### 💡 Follow-Up

#### ❓ Should we return from finally?

* ❌ NO
* Major production anti-pattern.

---

## 5️⃣ Checked vs Unchecked Choice

### ❓ Question

* User enters wrong password. Checked or unchecked?

### ✅ Expected Answer

* ✅ Unchecked Exception

#### Because:

* business validation failure
* expected invalid input
* clean service APIs
* Spring rollback friendly

```java
throw new InvalidCredentialsException();
```

### 💡 Follow-Up

#### ❓ Why not checked?

* controller/global handler already manages response
* caller should not be forced to try-catch for validation failure

#### ❓ Should client be forced to handle it?

* ❌ No

---

## 6️⃣ Resource Handling Scenario

### ❓ Question

* File opened, exception occurs before closing.
* How to handle?

### ✅ Expected Answer

* ✅ Use Try-With-Resources

```java
try (FileInputStream fis = new FileInputStream(file)) {
    // read file
}
```

### 💡 Follow-Up

#### ❓ Why better than finally?

* automatic cleanup
* less boilerplate
* safer
* suppressed exception support

#### ❓ What interface required?

* ✅ AutoCloseable
* Java `AutoCloseable`

---

## 7️⃣ Exception in Constructor

### ❓ Question

* What if exception occurs inside constructor?

### ✅ Expected Answer

* object creation fails
* reference not assigned usable object
* caller must handle or propagate
* partial object discarded

```java
PatientService service = new PatientService();
```

* If constructor fails → no valid object.

### 💡 Follow-Up

#### ❓ Can constructor throw exception?

* ✅ Yes

#### ❓ Can it throw checked exception?

* ✅ Yes, using throws

---

## 8️⃣ Exception in Multithreading

### ❓ Question

* Exception occurs inside a thread. Will main thread catch it?

### ✅ Expected Answer

* ❌ No
* each thread has separate stack
* exception propagation is thread-local
* worker thread dies only

### 💡 Solution

* ✅ Use UncaughtExceptionHandler

```java
thread.setUncaughtExceptionHandler(...)
```

* Perfect for:

  * batch jobs
  * retry workers
  * async notifications

---

## 9️⃣ REST API Exception Scenario

### ❓ Question

* Patient not found in DB.
* What exception and HTTP status?

### ✅ Expected Answer

* throw:

```java
PatientNotFoundException
```

* return:

```java
404 NOT FOUND
```

### Spring Example

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
```

### 💡 Follow-Up

#### ❓ Why not return null?

* ambiguous
* causes NPE risk
* poor API contract

#### ❓ Why not 500?

* resource absence is client/business issue
* server not broken

---

## 🔟 Logging vs Rethrowing

### ❓ Question

* Should you log exception and throw again?

### ✅ Expected Answer

* ✅ Log Once + Rethrow
* log at boundary
* rethrow to higher abstraction layer
* avoid duplicate logs

### 💡 Follow-Up

#### ❓ Where should logging happen?

* Best place:

  * global handler
  * scheduler boundary
  * thread boundary
  * API gateway boundary

---

## 1️⃣1️⃣ Catching Generic Exception

### ❓ Question

* Is this good practice?

```java
catch (Exception e) {}
```

### ❌ Expected Answer

* NO
* hides real issue
* kills debugging
* may swallow business failure
* dangerous in production

### ✅ Better

```java
catch (SQLException e) {}
catch (ValidationException e) {}
```

---

## 1️⃣2️⃣ Validation Failure Scenario

### ❓ Question

* Age < 18 for voting. Exception or if-else?

### ✅ Expected Answer

* validate using if-else
* exception only when flow truly cannot continue

```java
if (age < 18) {
    throw new AgeNotEligibleException();
}
```

### 🎯 Senior-Level Insight

* For bulk validation:

  * prefer error collection
  * avoid exception in loops

---

## 1️⃣3️⃣ Exception vs Return Code

### ❓ Question

* Method fails — return false or throw exception?

### ✅ Expected Answer

#### ✅ Rule

* expected failure → return value
* unexpected failure → exception

### 🏦 Banking Example

* wrong OTP → return business response
* DB timeout → throw exception

---

## 1️⃣4️⃣ Spring Boot Global Exception Handling

### ❓ Question

* How do you avoid try-catch in every controller?

### ✅ Expected Answer

* ✅ Use Spring Boot `@ControllerAdvice`

* This gives:

  * centralized handling
  * consistent error response
  * thin controllers
  * reusable mapping

* Perfect for:

  * healthcare APIs
  * banking services
  * payment microservices

---

## 1️⃣5️⃣ Performance Scenario

### ❓ Question

* Should exceptions be used for normal flow?

### ❌ Expected Answer

* NO
* exceptions are expensive
* stack trace creation cost
* logging overhead
* bad hot-path performance

### ✅ Better

* Use:

  * if-else
  * guard clauses
  * boolean validation
  * error objects in bulk processing

---

## 🎯 Final Interview Master Answer Formula

* Whenever interviewer asks scenario-based exception question, answer like this:

### ✅ Perfect Senior Template

* Exception occurs in: DAO / service / validation / thread
* Handled by: service / controller advice / future / handler
* Why: correct abstraction + rollback + clean API
* Client receives: HTTP code / retry / business message
