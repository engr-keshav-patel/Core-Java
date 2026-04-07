# 🔥 throw vs throws — INTERVIEW NOTES (IN-DEPTH)

---

## ✅ Definition

* In Java, throw and throws are both related to exception handling, but they serve completely different purposes.
* throw is used to explicitly create and raise an exception object, while throws is used in the method signature to declare that a method may propagate exceptions.

### 📌 Simple 1-Line Explanation

* throw = actually raise exception, throws = declare possible exception.

> 👉 **Interview Tip:**
> The fastest interview answer:
> “throw is used inside method body, throws is used in method signature.”

---

## 🧠 Why It Is Important

* Very common Java interview topic
* Critical for:

  * API contract design
  * checked exception propagation
  * custom business exceptions
  * service-layer validation
* Frequently used in:

  * Spring service methods
  * constructors
  * utility methods
  * DAO layer

### 🏦 Banking Domain Relevance

#### throw

* insufficient balance
* duplicate transaction
* invalid account state

#### throws

* file-based statement generation
* external gateway timeout
* encryption key loading issue

> 🔥 **Important:**
> A strong engineer knows:
>
> * where to create exception
> * where to declare propagation
> * where to convert infra exceptions to domain exceptions

---

## 🔹 Core Concepts

### 1) Difference Between throw and throws

| Feature        | throw                     | throws                         |
| -------------- | ------------------------- | ------------------------------ |
| Purpose        | Actually throws exception | Declares exception possibility |
| Location       | Inside method/block       | Method signature               |
| Followed by    | Exception object          | Exception class name           |
| Count          | One object at a time      | Multiple exception types       |
| Runtime action | Creates exception flow    | Compiler/API contract          |
| Common use     | validation/business rules | checked propagation            |

> 👉 **Interview Tip:**
> Say:
> “throw is runtime action, throws is compile-time declaration.”

### 2) When Do We Use throw?

Use when:

* validation fails
* business rule breaks
* illegal state detected
* custom domain exception needed
* fail-fast logic required

#### Example

```java
if (amount <= 0) {
    throw new IllegalArgumentException("Invalid amount");
}
```

#### Banking Example

* transfer amount negative
* sender == receiver
* duplicate UPI request
* account frozen
* All should use throw.

### 3) When Do We Use throws?

Use when method:

* does not want to handle checked exception
* wants caller to decide recovery
* forms public API contract
* utility method delegates failure

#### Example

```java
public void generateStatement() throws IOException
```

This means:

* file operation may fail
* caller must handle or propagate

---

## 🔍 Interview Follow-Up Questions

### ❓ Can We Throw Checked Exception Without throws?

#### ⚡ Yes, but only if handled locally

```java
try {
    throw new IOException("File missing");
} catch (IOException e) {
    System.out.println("Recovered");
}
```

#### ❌ No, if propagating outside method

Then method must declare:

```java
throws IOException
```

> 👉 **Interview Tip:**
> Compiler rule applies only when checked exception can escape method boundary.

### ❓ Can Constructor Throw Exception?

#### ✅ Yes

* Very common and important.

```java
public AccountService() throws IOException {
    loadEncryptionKeys();
}
```

#### Real Use Cases

* config loading
* certificate loading
* DB pool init
* API key validation
* invalid constructor arguments

#### Banking Example

* startup loads HSM encryption key
* if key missing → constructor throws

### ❓ Can Overridden Method Throw Exception?

#### ✅ Yes, but with rules

* Very common tricky question.

#### Rules

Child overridden method:

* can throw same exception
* can throw child exception
* can throw unchecked exception
* cannot throw broader checked parent exception

#### ✅ Valid Example

```java
class Parent {
    void read() throws IOException {}
}

class Child extends Parent {
    void read() throws FileNotFoundException {}
}
```

Because:

* child exception is narrower

#### ❌ Invalid Example

```java
class Parent {
    void read() throws IOException {}
}

class Child extends Parent {
    void read() throws Exception {}
}
```

* Compile error because child broadened checked exception.

> 🔥 **Important:**
> This protects Liskov Substitution Principle + polymorphism safety.

---

## 💻 Code Example

### ✅ throw Example

```java
public class TransferService {

    public void validate(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Transfer amount must be positive");
        }
    }
}
```

#### Why Correct

* validation failure
* fail fast
* unchecked business misuse

### ✅ throws Example

```java
import java.io.IOException;

public class StatementService {

    public void generatePdf() throws IOException {
        throw new IOException("Storage unavailable");
    }
}
```

#### Why Correct

* external recoverable issue
* caller decides retry/fallback

---

## 🌍 Real-World Examples

### 🏦 Example 1: Fund Transfer Validation

#### throw

* insufficient balance
* invalid IFSC
* self-transfer blocked

### 💳 Example 2: Statement File Export

#### throws IOException

* storage unavailable
* disk write failed
* permission issue

### 🔐 Example 3: Constructor Failure

#### throws SecurityException

* HSM key invalid
* certificate load failed

---

## ⚠️ Common Interview Traps

### ❌ Trap 1: Using throws Instead of throw

Wrong:

```java
throws new Exception()
```

### ❌ Trap 2: Throwing Generic Exception

```java
throw new Exception("error");
```

* Bad design.
* Prefer:

  * IllegalArgumentException
  * custom domain exception
  * infra-specific exception

### ❌ Trap 3: throws Exception

* Method signature pollution.
* Avoid in enterprise code.

### ❌ Trap 4: Broadening Checked Exception In Override

* Common inheritance trap.

---

## 🚀 Best Practices

### Use throw for:

* validation
* business rule failure
* illegal state

### Use throws for:

* checked propagation

* public API contracts

* Prefer custom domain exceptions

* Avoid throws Exception

* Keep constructor exceptions meaningful

* Preserve original cause when wrapping

### In Spring:

* use unchecked custom exceptions
* global handler maps response

### 🏦 Banking Best Practice

```java
throw new InsufficientBalanceException(...)
```

* Much better than generic exception.

---

## 🎯 Interview-Ready Final Answer

* throw is used to explicitly create and raise an exception object inside method logic, usually for validation or business rule failures.
* throws is used in the method signature to declare that a method may propagate checked exceptions to the caller.
* Checked exceptions escaping a method require throws, while local handling does not.
* Constructors can throw exceptions, and overridden methods can only throw same, narrower, or unchecked exceptions.
* In banking systems, throw is commonly used for domain validation, while throws is used for infrastructure failures like file and IO operations.

> 👉 **Interview Tip (2+ Years Experience):**
> Always connect this topic with:
>
> * API contract design
> * Spring service exceptions
> * constructor initialization safety
> * inheritance override rules
> * custom domain exceptions
>
> This makes your answer sound senior and architecture-aware.
