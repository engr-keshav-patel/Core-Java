# 🔥 throw vs throws  
> Source condensed from your detailed notes

## ✅ Definition

* `throw` = **actually raises exception object** inside method
* `throws` = **declares exception propagation** in method signature
* 💡 **Memory shortcut:** *throw = action, throws = declaration*

---

## 🔹 Must-Remember Points

* `throw` used for

  * validation failures
  * business rule violations
  * fail-fast logic
* `throws` used for

  * checked exception propagation
  * API contract clarity
  * caller-driven retry/fallback
* **🔥 Important**

  * `throw` → followed by **exception object**
  * `throws` → followed by **exception class name(s)**
* Override rule

  * child method can throw **same / narrower / unchecked only**

---

## ⚡   Quick Comparison

| Feature      | `throw`         | `throws`              |
| ------------ | --------------- | --------------------- |
| Use place    | method body     | method signature      |
| Purpose      | raise exception | declare propagation   |
| Common use   | validation      | checked exceptions    |
| Runtime role | starts flow     | compile-time contract |

---

## ⚠️ Common Traps

* Using `throws new Exception()` ❌
* Throwing generic `Exception` ❌
* `throws Exception` in service layer ❌
* Broadening checked exception in override ❌

👉 **  Tip:** `throw` = runtime action, `throws` = compile-time declaration

---

## 🌍 Real-World Example

🏦 **Fund Transfer**

* `throw` → `InsufficientBalanceException`
* `throws` → `IOException` for statement PDF export
* cleanly separates **domain vs infra failures**

---

## 🎯 30-Second Spoken Answer

`throw` is used inside method logic to explicitly raise an exception, usually for validation or business rule failures. `throws` is used in the method signature to declare that checked exceptions may propagate to the caller. In enterprise systems, `throw` is common for domain exceptions like insufficient balance, while `throws` is useful for recoverable IO or external system failures.

---
---

# 🔥 Custom / User-Defined Exceptions  
> Source condensed from your detailed notes

## ✅ Definition

* Custom exceptions = **developer-created domain-specific exceptions**
* Used to represent **business rule violations clearly**
* 💡 **Memory shortcut:** *Business meaning + Exception*

---

## 🔹 Must-Remember Points

* Usually extend **`RuntimeException`** in Spring Boot 🔥
* Best for

  * validation failures
  * domain rules
  * illegal business state
  * workflow violations
* Better than generic `RuntimeException`
* Naming rule

  * `InsufficientBalanceException`
  * `PatientAlreadyExistsException`
* **🔥 Important**

  * improves API readability
  * easier HTTP status mapping
  * supports rollback + global handler

---

## ⚡   Quick Comparison

| Generic                             | Custom                         |
| ----------------------------------- | ------------------------------ |
| `RuntimeException("Balance issue")` | `InsufficientBalanceException` |
| poor readability                    | domain clear                   |
| weak API mapping                    | easy `409/400` mapping         |
| bad logs                            | meaningful logs                |

---

## ⚠️ Common Traps

* Using generic `RuntimeException` ❌
* Extending `Throwable` ❌
* Poor naming like `BalanceIssue` ❌
* Missing constructor with cause ❌

👉 **  Tip:** In Spring, prefer **unchecked custom exceptions + `@ControllerAdvice`**

---

## 🌍 Real-World Example

🏦 **Bank Transfer**

* `InsufficientBalanceException`
* `DuplicateTransactionException`
* `AccountFrozenException`

These make **rollback + HTTP mapping + logs** much cleaner.

---

## 🎯 30-Second Spoken Answer

Custom exceptions are user-defined exception classes used to represent domain-specific business failures clearly. In Spring Boot, they usually extend `RuntimeException` because this keeps service APIs clean and supports automatic rollback. Examples include `InsufficientBalanceException` or `PatientAlreadyExistsException`. They improve readability, logging, and HTTP error mapping compared to generic exceptions.

---
---

# 🔥 Exception Handling with Method Overriding  
> 🎯 **  Focus:** Override rules + LSP + common traps
>
> 💡 **Golden Rule:** **Checked = same / narrower / none** | **Unchecked = any**

---

## ✅ Definition

* In **method overriding**, child method must **respect parent exception contract**
* Rule exists to protect **polymorphism + LSP (safe substitution)**
* 👉 Parent reference users should never get a **broader checked exception surprise**

---

## 🔹 Must-Remember Rules

### ✅ Checked Exceptions

* Same exception → ✅ Allowed
* Narrower child exception → ✅ Allowed
* No exception → ✅ Allowed
* Broader checked exception → ❌ Not allowed

### ✅ Unchecked Exceptions

* Any `RuntimeException` → ✅ Always allowed
* No compiler restriction

### 🔥 Important Memory Line

* **Child can reduce exception burden**
* **Child cannot increase checked exception burden**

---

## ⚡   Quick Comparison

| Child Override Case            | Result | Reason          |
| ------------------------------ | ------ | --------------- |
| `throws IOException`           | ✅      | same contract   |
| `throws FileNotFoundException` | ✅      | narrower        |
| no exception                   | ✅      | safer contract  |
| `throws Exception`             | ❌      | broader checked |
| `throws IllegalStateException` | ✅      | unchecked       |

---

## ⚠️ Common Traps

* ❌ Broadening checked exception
* ❌ Applying same rule to runtime exceptions
* ❌ Forgetting “no exception” is valid
* ❌ Not explaining **LSP / polymorphism reason**

👉 **  Tip:** Always say *“Java prevents breaking parent API contract.”*

---

## 🌍 Real-World Example

🏦 **PaymentProcessor → UpiPaymentProcessor**

* Parent: `process() throws IOException`
* Child: `process() throws FileNotFoundException` → ✅
* Child: `process() throws Exception` → ❌

💡 This keeps **payment service APIs backward-compatible**

---

## 🎯 30-Second Spoken Answer

In method overriding, checked exceptions can stay the same, become narrower, or be removed, but they cannot become broader. This protects polymorphism and Liskov Substitution Principle because parent reference callers should not be forced to handle new wider checked exceptions. Unchecked exceptions are always allowed. The easiest memory rule is: **checked same or narrower, runtime any**.

---
---

# 🔥 Finally Block — Advanced  
> 🎯 **  Focus:** return + exception + `System.exit()` traps
>
> 💡 **Golden Rule:** **finally = must-run cleanup before method exit**

---

## ✅ Definition

* `finally` is the **guaranteed cleanup block** in Java exception handling
* Used for **resource safety + deterministic cleanup**
* Best for locks, thread-local cleanup, MDC, manual resources

---

## 🔹 Must-Remember Rules

### ✅ finally after `return`

* **Yes, executes** ✅
* Flow: **return value stored → finally runs → method exits**
* 👉 return is prepared first, exit happens later

### ✅ finally after exception

* **Yes, executes** ✅
* Runs before stack unwinding completes

### ❌ finally after `System.exit()`

* **Does NOT execute** ❌
* JVM terminates immediately

### 🔥 Dangerous Traps

* `return` inside finally → **overrides original return** ❌
* exception inside finally → **hides original exception** ❌

---

## ⚡   Quick Comparison

| Scenario         | finally Runs? |   Point    |
| ---------------- | ------------- | ------------------ |
| normal execution | ✅             | cleanup            |
| `return` in try  | ✅             | runs before exit   |
| exception in try | ✅             | runs before unwind |
| `System.exit()`  | ❌             | JVM dead           |
| JVM crash / kill | ❌             | no control flow    |

---

## ⚠️ Common Traps

* ❌ Saying finally always runs
* ❌ Returning from finally
* ❌ Throwing cleanup exception from finally
* ❌ Using finally instead of try-with-resources for files

👉 **  Tip:** Say *“finally runs only if JVM remains alive.”*

---

## 🌍 Real-World Example

🏦 **Payment Lock Cleanup**

* acquire Redis lock in `try`
* payment processing
* `finally` → release lock

✅ prevents **deadlock + duplicate payment retries**

---

## 🎯 30-Second Spoken Answer

The advanced purpose of `finally` is guaranteed cleanup before method exit. It executes after normal flow, after return, and even after exceptions, but it does not run if the JVM terminates using `System.exit()` or crashes. Returning or throwing from finally is dangerous because it can hide original return values or exceptions. In modern Java, use try-with-resources for files and finally mainly for locks, MDC, and thread-local cleanup.

---
---

# 🔥 Try-With-Resources (Java 7+)  
> 🎯 **  Focus:** auto-close + `AutoCloseable` + reverse close order
>
> 💡 **Golden Rule:** **compiler handles cleanup, no manual finally needed**

---

## ✅ Definition

* Java 7 feature for **automatic resource cleanup**
* Best for files, JDBC, sockets, streams, Kafka consumers
* Works even if **exception / return happens**

---

## 🔹 Must-Remember Rules

### ✅ Core Rule

* Resource must implement **`AutoCloseable`**
* JVM auto-calls `close()` after block
* Compiler internally creates **hidden finally logic**

### ✅ Multiple Resources

* Supported ✅
* Closed in **reverse order** 🔥
* Example: `PreparedStatement` closes before `Connection`

### 🔥 Why Preferred

* no null checks
* less boilerplate
* preserves original exceptions better
* supports **suppressed exceptions**

---

## ⚡   Quick Comparison

| Feature               | `finally` | Try-With-Resources |
| --------------------- | --------- | ------------------ |
| cleanup               | manual    | automatic          |
| null checks           | needed    | no                 |
| boilerplate           | high      | low                |
| suppressed exceptions | weak      | strong             |
| modern best practice  | ❌         | ✅                  |

---

## ⚠️ Common Traps

* ❌ resource not implementing `AutoCloseable`
* ❌ forgetting reverse close order
* ❌ using old `finally` style for streams
* ❌ ignoring suppressed exceptions

👉 **  Tip:** Say *“try-with-resources is compiler-generated finally with safer exception preservation.”*

---

## 🌍 Real-World Example

🏦 **Bank Statement CSV Import**

* open `BufferedReader`
* parse transactions
* auto-close file even if parse fails

✅ prevents **file lock + connection leak issues**

---

## 🎯 30-Second Spoken Answer

Try-with-resources is a Java 7 feature that automatically closes resources after use, even if exceptions occur. Any resource used must implement `AutoCloseable`, and multiple resources are closed in reverse order. It is preferred over manual finally blocks because it reduces boilerplate, prevents leaks, and preserves suppressed exceptions better. In production, it is heavily used for JDBC, file streams, sockets, and batch processing.
