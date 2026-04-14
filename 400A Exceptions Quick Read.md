# 🔥 Basics of Exception Handling — 30 SECOND INTERVIEW NOTES

## Source condensed from your detailed notes

## ✅ Definition

* Exception handling = managing runtime failures gracefully without crashing application
* Used for recovery, rollback, logging, controlled failure

> 💡 Memory shortcut:
> “Detect → Propagate → Handle → Recover”

## 🔹 Must-Remember Points

### Throwable = root parent

* Error → JVM/system failure
* Exception → application issues
* RuntimeException → unchecked programming bugs

### Flow

* issue occurs
* JVM creates exception object
* stack unwinding starts
* nearest matching catch runs

> 🔥 Important
> Checked → compiler forces handling
> Unchecked → mostly coding/validation bugs
> Errors → usually don’t catch

### Best Practices

* catch specific exceptions
* preserve root cause
* log stack trace
* rollback transactions
* use global exception handler in Spring Boot

### Performance

* expensive object creation + stack trace
* 👉 never use for normal flow

## ⚡ Interview Quick Comparison

| Type             | Meaning               | Interview Use          |
| ---------------- | --------------------- | ---------------------- |
| Exception        | Recoverable app issue | Handle properly        |
| RuntimeException | Bug / bad input       | Prevent via validation |
| Error            | JVM critical failure  | Avoid catching         |
| Throwable        | Root superclass       | Generic parent         |

## ⚠️ Common Traps

* Catching generic Exception everywhere
* Silent catch block
* Using exceptions in loops / normal logic
* Losing original stack trace during rethrow
* Catching Throwable or Error
* ❗ In banking: missing rollback = money inconsistency bug

> 👉 Interview Tip: Mention rollback + retries + idempotency

## 🌍 Real-World Example

### 🏦 Fund Transfer

* debit success
* credit DB fails
* exception triggers transaction rollback
* prevents double deduction / inconsistent balance

## 🎯 30-Second Spoken Answer

> Exception handling in Java is used to manage runtime abnormal conditions so the application can recover gracefully or fail safely. Throwable is the root class, where Exception handles recoverable issues and Error represents serious JVM failures. In production, good exception handling means specific catch blocks, proper logging, transaction rollback, and retry support. In banking systems, this is critical for balance consistency and customer trust.

---
# 🔥 Checked vs Unchecked Exceptions — 30 SECOND INTERVIEW NOTES

> Source condensed from your detailed notes

## ✅ Definition

* **Checked** = compiler **forces handle or declare**
* **Unchecked** = compiler **does not force**, happens at runtime
* 💡 **Memory shortcut:** *Recoverable = Checked, Coding bug = Unchecked*

---

## 🔹 Must-Remember Points

* **Checked Exception**

  * child of `Exception`
  * excludes `RuntimeException`
  * used for **IO / DB / network / external failures**
  * caller can often **retry / fallback**
* **Unchecked Exception**

  * child of `RuntimeException`
  * used for **validation bugs / bad state / coding mistakes**
  * fail fast, no forced try-catch
* **🔥 Important**

  * Spring Boot usually prefers **custom unchecked exceptions**
  * default transaction rollback works for unchecked
  * always preserve root cause when wrapping

---

## ⚡ Interview Quick Comparison

| Feature        | Checked                    | Unchecked                  |
| -------------- | -------------------------- | -------------------------- |
| Compiler check | ✅ Yes                      | ❌ No                       |
| Parent         | `Exception`                | `RuntimeException`         |
| Cause          | External recoverable issue | Bug / invalid input        |
| Spring usage   | Less preferred             | Highly preferred           |
| Example        | `IOException`              | `IllegalArgumentException` |

---

## ⚠️ Common Traps

* Saying checked is always better ❌
* Using checked exception for simple validation ❌
* Wrapping everything in `RuntimeException` blindly ❌
* Losing original cause ❌
* `throws Exception` in service layer ❌

👉 **Interview Tip:**
Use **checked for infra failures**, **unchecked for domain/business rule violations**

---

## 🌍 Real-World Example

🏦 **Bank Fund Transfer**

* **Checked:** payment gateway timeout → retry possible
* **Unchecked:** negative transfer amount → `IllegalArgumentException`
* prevents invalid debit flow

---

## 🎯 30-Second Spoken Answer

Checked exceptions are compile-time enforced and used for recoverable external failures like file, DB, or network issues. Unchecked exceptions extend `RuntimeException` and usually represent coding bugs, invalid input, or illegal business states. In Spring Boot, unchecked exceptions are preferred because they keep APIs clean and support automatic transaction rollback. In banking systems, checked exceptions suit gateway or IO failures, while unchecked exceptions fit validation and domain rule violations.

---
---

# 🔥 try, catch, finally — 30 SECOND INTERVIEW NOTES

> Source condensed from your detailed notes

## ✅ Definition

* `try` = risky code
* `catch` = handles exception
* `finally` = **always cleanup**
* 💡 **Memory shortcut:** *Try = execute, Catch = recover, Finally = cleanup*

---

## 🔹 Must-Remember Points

* **Valid combinations**

  * `try + catch`
  * `try + finally`
  * `try + catch + finally`
* `try` **cannot exist alone** ❗
* **finally use cases**

  * DB connection close
  * file/socket close
  * Redis lock release
  * thread-local cleanup
* **🔥 Important**

  * `finally` runs even if `return` exists in `try`
  * usually skipped only on `System.exit()` / JVM crash
  * best for **must-run cleanup code**

---

## ⚡ Interview Quick Comparison

| Block     | Purpose              | Banking Use           |
| --------- | -------------------- | --------------------- |
| `try`     | risky business logic | debit + credit        |
| `catch`   | rollback / fallback  | reverse failed txn    |
| `finally` | guaranteed cleanup   | close DB / unlock row |

---

## ⚠️ Common Traps

* Returning from `finally` ❌
* Throwing new exception inside `finally` ❌
* Writing cleanup in `catch` instead of `finally` ❌
* Large business logic inside `finally` ❌

👉 **Interview Tip:** Never `return` from `finally` — it hides real exceptions

---

## 🌍 Real-World Example

🏦 **Fund Transfer**

* `try` → debit sender + credit receiver
* `catch` → rollback on failure
* `finally` → close DB connection + release lock

---

## 🎯 30-Second Spoken Answer

`try` contains risky code, `catch` handles failures, and `finally` is used for cleanup that must run whether exception occurs or not. It is critical for closing DB connections, file streams, and distributed locks. `finally` almost always executes except rare JVM termination scenarios. In banking systems, this prevents connection leaks, deadlocks, and transaction resource issues.

---
---

# 🔥 Multiple catch Blocks — 30 SECOND INTERVIEW NOTES

> Source condensed from your detailed notes

## ✅ Definition

* Use **multiple `catch` blocks** when different exceptions need different recovery
* Improves **specificity + readability + retry strategy**
* 💡 **Memory shortcut:** *Specific catch first, generic catch last*

---

## 🔹 Must-Remember Points

* One `try` can have **multiple catch blocks** ✅
* JVM checks **top to bottom**
* **🔥 Important:** child exception must come **before parent**

  * `FileNotFoundException` → before `IOException`
* Wrong order = **unreachable catch compile error** ❗
* Use **multi-catch (`|`)** when handling logic is same

  * `catch (IOException | SQLException e)`

---

## ⚡ Interview Quick Comparison

| Case                       | Best Practice         |
| -------------------------- | --------------------- |
| Different recovery actions | multiple catch        |
| Same recovery action       | multi-catch           |
| Parent + child types       | child first           |
| Generic fallback           | keep `Exception` last |

---

## ⚠️ Common Traps

* Catching `Exception` first ❌
* Wrong child-parent order ❌
* Duplicate catch logic instead of multi-catch ❌
* Catching `Throwable` in business code ❌

👉 **Interview Tip:** JVM always picks the **first matching catch block**

---

## 🌍 Real-World Example

🏦 **Fund Transfer API**

* `IOException` → gateway timeout → retry
* `SQLException` → rollback DB
* `IllegalArgumentException` → invalid amount → HTTP 400

---

## 🎯 30-Second Spoken Answer

Java supports multiple catch blocks so different exception types can be handled with different recovery logic. Catch order is critical because JVM checks top to bottom, so child exceptions must come before parent exceptions. If the order is wrong, the child catch becomes unreachable and compilation fails. In banking systems, this cleanly separates retryable gateway failures, DB rollback scenarios, and validation errors.

---
---

# 🔥 Exception Propagation — 30 SECOND INTERVIEW NOTES

> Source condensed from your detailed notes

## ✅ Definition

* If current method does **not handle exception**, JVM passes it to **caller method**
* This upward movement is **exception propagation**
* 💡 **Memory shortcut:** *Throw → caller → caller → handler*

---

## 🔹 Must-Remember Points

* Happens via **stack unwinding** 🔥
* JVM checks current method’s `catch`
* If not found → pops stack frame → moves to caller
* **Both checked + unchecked** propagate same at runtime
* Difference is only **compiler enforcement**
* **Best layered handling**

  * DAO → throw
  * Service → rollback / retry
  * ControllerAdvice → HTTP response
* In multithreading → propagation is **thread-bound only**

---

## ⚡ Interview Quick Comparison

| Layer          | Responsibility              |
| -------------- | --------------------------- |
| DAO            | throw persistence exception |
| Service        | rollback + compensation     |
| Controller     | API response                |
| Global Handler | common error mapping        |

---

## ⚠️ Common Traps

* Catching in DAO unnecessarily ❌
* Swallowing exception ❌
* Logging at every layer ❌
* Thinking child thread exception reaches parent ❌

👉 **Interview Tip:** Best place to stop propagation = **service layer**

---

## 🌍 Real-World Example

🏦 **Fund Transfer Flow**

* DAO → DB timeout
* propagates to service
* service → rollback debit
* controller → return failure response

---

## 🎯 30-Second Spoken Answer

Exception propagation means if a method does not handle an exception, Java automatically passes it to the caller through stack unwinding. This continues until a matching catch block is found or JVM terminates the thread. In layered applications, DAO usually throws, service handles rollback and compensation, and global handlers map final API errors. In multithreading, propagation stays within the same thread only.

---
---

