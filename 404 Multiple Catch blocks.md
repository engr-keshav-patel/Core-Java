# 🔥 Multiple catch Blocks — INTERVIEW NOTES (IN-DEPTH)

---

## ✅ Definition

* Java allows multiple catch blocks after a single try block so different exception types can be handled differently.
* This is useful when different recovery actions are needed for different failures.

### 📌 Simple 1-Line Explanation

* Use multiple catch blocks when different exceptions need different handling logic.

> 👉 **Interview Tip:**
> Say this in interviews:
> “Multiple catch blocks improve exception specificity, readability, and recovery strategy.”

---

## 🧠 Why It Is Important

* Enables exception-specific recovery
* Improves debugging clarity
* Prevents generic error handling
* Makes logs meaningful
* Helps return correct API responses
* Very important in banking transaction flows

### 🏦 Banking Domain Relevance

* A fund transfer API may fail due to:

  * SQLException

    * rollback DB transaction
  * IOException

    * payment gateway response failure
  * IllegalArgumentException

    * invalid transfer amount
* Each needs different handling behavior, so multiple catch blocks are ideal.

> 🔥 **Important:**
> Different exception types usually need:
>
> * different rollback logic
> * different retry policies
> * different alert severity
> * different HTTP status codes

---

## 🔹 Core Concepts

### 1) Can We Have Multiple catch Blocks?

#### ✅ Yes

* One try block can have multiple catch blocks.

```java
try {
    processTransfer();
} catch (IOException e) {
    System.out.println("Gateway communication failed");
} catch (SQLException e) {
    System.out.println("Database failed");
}
```

#### Why Useful

* file issue → retry
* DB issue → rollback
* validation issue → return 400
* This makes code production-friendly.

### 2) Order of catch Blocks — Why Important?

* JVM checks catch blocks top to bottom
* First matching block executes
* More specific exceptions must come first
* Parent/general exceptions must come later

#### Correct Order

```java
try {
    riskyCode();
} catch (FileNotFoundException e) {
    System.out.println("Specific file issue");
} catch (IOException e) {
    System.out.println("General IO issue");
}
```

#### Why?

Because:

* FileNotFoundException is child of IOException

> 👉 JVM should get chance to match more specific handler first.

### 3) What Happens If Order Is Wrong?

* Compiler gives unreachable catch block error
* Because parent catch already covers child

#### ❌ Wrong:

```java
try {
    riskyCode();
} catch (IOException e) {
    System.out.println("IO issue");
} catch (FileNotFoundException e) {
    System.out.println("Specific file issue");
}
```

#### Compile Error Reason

* Second block can never be reached.

> 🔥 **Important:**
> This is one of the most common Java interview trap questions.

---

## 🔍 Interview Follow-Up Questions

### ❓ Can We Catch Parent Exception Before Child?

#### ❌ No (not if child catch comes later)

* This makes child block unreachable.

#### Why Internally?

Because JVM checks:

* first catch
* if parent matches
* child catch never gets control
* This violates deterministic catch resolution.

### ❓ What Is Unreachable Catch Block?

* A catch block that can never execute
* Usually happens when:

  * parent placed before child
  * duplicate exception types
  * overly generic Exception before specific catch

#### Example

```java
catch (Exception e)
catch (NullPointerException e)
```

* Second block unreachable.

### ❓ Can We Use Multiple Exceptions In One Catch?

#### ✅ Yes — Multi-Catch (Java 7+)

* Very common and interview-important.

```java
catch (IOException | SQLException e) {
    log.error("Infra failure", e);
}
```

#### When To Use

Use when recovery logic is same:

* log error
* rollback
* return same response
* alert same support team

---

## 💻 Code Example

### ✅ Banking Transfer Example

```java
public class TransferService {

    public void transfer() {
        try {
            processPaymentGateway();
            saveTransaction();
        } catch (java.io.IOException e) {
            System.out.println("Gateway failed → retry later");
        } catch (RuntimeException e) {
            System.out.println("Validation or business failure");
        } finally {
            System.out.println("Release transaction lock");
        }
    }

    private void processPaymentGateway() throws java.io.IOException {
        throw new java.io.IOException("Timeout");
    }

    private void saveTransaction() {
        System.out.println("Saved");
    }
}
```

#### Why This Is Correct

* different failure categories separated
* retry-friendly infra exception
* business exception isolated
* cleanup guaranteed

---

## 🌍 Real-World Examples

### 🏦 Example 1: NEFT Transfer

* SQLException
* rollback DB
* IOException
* retry bank gateway
* IllegalStateException
* duplicate transfer block

### 💳 Example 2: Credit Card Payment

* SocketTimeoutException
* retry
* ValidationException
* reject request
* DuplicatePaymentException
* idempotency response

### 📁 Example 3: Statement Processing

* FileNotFoundException
* fetch backup file
* ParseException
* mark statement corrupted

---

## ⚠️ Common Interview Traps

### ❌ Trap 1: Catching Exception First

```java
catch (Exception e)
catch (IOException e)
```

* Second catch unreachable.

### ❌ Trap 2: Too Many Similar Catch Blocks

* If recovery logic same, use multi-catch.

### ❌ Trap 3: Copy-Paste Logging

* Repeated logging blocks create noisy code.
* Use centralized utility/global handler.

### ❌ Trap 4: Catching Throwable

* Bad production design.
* May catch:

  * Error
  * JVM critical failures
* Never do this in business code.

---

## 🚀 Best Practices

* Catch specific exceptions first
* Keep parent catch last
* Use multi-catch when handling same way
* Avoid duplicate catch logic
* Preserve root cause
* Convert infra exceptions to domain exceptions

### In Spring Boot:

* prefer global exception mapping
* avoid controller-level repeated catches

### Banking Production Best Practice

Map:

* SQLException → TransactionFailedException

* TimeoutException → retry queue

* ValidationException → HTTP 400

* This keeps service clean.

---

## 🔍 Tricky Interview Questions

### ❓ What About This?

```java
catch (IOException | SQLException e) { }
```

#### ✅ Valid

* This is called multi-catch block.

#### Rules

* both exceptions should not have parent-child relation
* variable e is implicitly final
* same recovery logic should apply

#### Example Use Case

* both DB and file failures
* same action = log + retry

### ❓ Can We Reassign e In Multi-Catch?

#### ❌ No

```java
e = new IOException(); // compile error
```

* Because e is effectively final.

---

## 🎯 Interview-Ready Final Answer

* Yes, Java supports multiple catch blocks so different exceptions can be handled differently.
* Catch block order is critical because JVM checks from top to bottom, so child exceptions must always come before parent exceptions.
* If parent comes first, child catch becomes unreachable and compilation fails.
* Java 7 introduced multi-catch, allowing multiple unrelated exceptions in one catch block when handling logic is same.
* In banking systems, this helps separate retryable gateway failures, DB rollbacks, and validation errors cleanly.

> 👉 **Interview Tip (2+ Years Experience):**
> Always connect this topic with:
>
> * retry vs rollback strategy
> * HTTP error mapping
> * idempotency
> * infra vs business exceptions
> * Spring global handlers
>
> This makes your answer sound senior-level and production-focused.
