# Day 4 — Exception Handling, Custom Exceptions, `finally`, `try-with-resources` Notes


---

# 1) What is Exception Handling

## ✅ Definition

Exception handling is the mechanism to **handle runtime problems gracefully** so program flow can continue or fail meaningfully.

## ✅ Why needed

* prevents abrupt termination
* separates normal flow from error flow
* improves debugging
* supports business error messaging
* mandatory in backend service layers

```java
int a = 10 / 0; // ArithmeticException
```

---

# 2) Exception Hierarchy

```mermaid
flowchart TD
    A[Throwable]
    A --> B[Error]
    A --> C[Exception]
    C --> D[Checked Exceptions]
    C --> E[RuntimeException]
```

## ✅ Hierarchy meaning

* `Throwable` → root of all throwable problems
* `Error` → serious JVM/system issue
* `Exception` → application-level recoverable issues
* `RuntimeException` → unchecked runtime bugs

## 📝 Must remember

* interviewers often ask: **Error vs Exception**

---

# 3) Checked vs Unchecked Exceptions ⭐

## ✅ Checked Exception

Handled at **compile time**.

Examples:

* `IOException`
* `SQLException`
* `FileNotFoundException`

```java
FileReader fr = new FileReader("a.txt");
```

## ✅ Unchecked Exception

Occurs at runtime.

Examples:

* `NullPointerException`
* `ArithmeticException`
* `ArrayIndexOutOfBoundsException`
* `IllegalArgumentException`

## 📌 Difference Table

| Topic               | Checked         | Unchecked          |
| ------------------- | --------------- | ------------------ |
| checked by compiler | yes             | no                 |
| extends             | `Exception`     | `RuntimeException` |
| handling mandatory  | yes             | no                 |
| cause               | external issues | programming bugs   |

## 🎯 Interview line

> Checked exceptions represent recoverable external failures, while unchecked exceptions usually indicate coding mistakes.

---

# 4) `try-catch-finally`

## ✅ Basic flow

```java
try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
} finally {
    System.out.println("Always runs");
}
```

## Mermaid Flow

```mermaid
flowchart TD
    A[try block] --> B{Exception occurs?}
    B -->|yes| C[catch block]
    B -->|no| D[skip catch]
    C --> E[finally]
    D --> E
```

## 📝 Must remember

* `finally` runs almost always
* used for cleanup
* DB connection closing common use case

---

# 5) `throw` vs `throws`

## ✅ `throw`

Used to explicitly throw exception object.

```java
throw new IllegalArgumentException("Invalid age");
```

## ✅ `throws`

Declares that method may throw exception.

```java
void read() throws IOException {}
```

## 📌 Difference Table

| Topic    | `throw`         | `throws`          |
| -------- | --------------- | ----------------- |
| purpose  | throw exception | declare exception |
| location | method body     | method signature  |
| count    | one object      | multiple types    |

---

# 6) Exception Propagation

## ✅ Meaning

When a method does not handle exception, it moves to **caller method**.

```java
void c() {
    int x = 10 / 0;
}

void b() { c(); }
void a() { b(); }
```

## Mermaid Flow

```mermaid
sequenceDiagram
    participant A as methodA
    participant B as methodB
    participant C as methodC

    A->>B: call
    B->>C: call
    C-->>B: exception
    B-->>A: propagate
```

## 📝 Remember

* propagation continues until handled
* if unhandled → JVM default handler

---

# 7) Multiple Catch + Multi Catch

## ✅ Multiple catch

```java
try {
    String s = null;
    s.length();
} catch (NullPointerException e) {
    System.out.println("NPE");
} catch (Exception e) {
    System.out.println("General");
}
```

## ✅ Multi catch

```java
catch (IOException | SQLException e)
```

## 📝 Trap point

* child catch must come **before parent catch**
* else unreachable catch compile error

---

# 8) `finally` Tricky Interview Cases ⭐

## ✅ Case 1: return in try

```java
int test() {
    try {
        return 10;
    } finally {
        System.out.println("finally");
    }
}
```

Output:

```java
finally
```

Return still happens after finally.

## ✅ Case 2: return in finally

```java
int test() {
    try {
        return 10;
    } finally {
        return 20;
    }
}
```

Result = `20`

## 🎯 Interview warning

> Return in `finally` suppresses earlier return/exception.

---

# 9) `try-with-resources` ⭐

## ✅ Why introduced

Automatic resource closing.

```java
try (BufferedReader br = new BufferedReader(new FileReader("a.txt"))) {
    System.out.println(br.readLine());
}
```

## Mermaid Flow

```mermaid
flowchart TD
    A[open resource] --> B[use resource]
    B --> C[auto close]
```

## ✅ Requirements

Resource must implement:

* `AutoCloseable`
* `Closeable`

## 🎯 Benefits

* cleaner code
* no manual finally cleanup
* prevents resource leaks

---

# 10) Custom Exceptions ⭐

## ✅ Why create custom exception

Used for business/domain-specific failures.

Examples:

* invalid user id
* insufficient balance
* duplicate email

```java
class InvalidAgeException extends RuntimeException {
    InvalidAgeException(String msg) {
        super(msg);
    }
}
```

## Usage

```java
if (age < 18) {
    throw new InvalidAgeException("Age must be 18+");
}
```

## 📝 Best practice

* checked → recoverable business flow
* unchecked → validation/programming/business rule failures

---

# 11) Common Runtime Exceptions

## ✅ Must know

* `NullPointerException`
* `ArithmeticException`
* `ClassCastException`
* `IllegalArgumentException`
* `IllegalStateException`
* `IndexOutOfBoundsException`
* `ConcurrentModificationException`

## 🎯 Interview expectation

Know:

* why it happens
* prevention
* real project example

---

# 12) Exception Handling Best Practices

## ✅ Must follow

* catch most specific exception first
* never swallow exceptions silently
* log meaningful message
* wrap lower exceptions in business exception
* avoid generic `Exception`
* use custom exceptions in service layer
* use `try-with-resources`

---

# 13) Most Important Trap Concepts

## ✅ Must remember bullets

* `finally` runs almost always
* return in finally overrides previous return
* checked exceptions are compiler enforced
* unchecked exceptions are runtime
* `throw` object, `throws` declaration
* child catch before parent
* resource must implement `AutoCloseable`
* custom exceptions improve domain clarity

---

# 14) Day 4 Revision Cheatsheet

## ⚡ Fast recall bullets

* checked = compile time
* unchecked = runtime
* `finally` = cleanup
* `throw` = explicit
* `throws` = declaration
* propagation = caller chain
* multi-catch supported
* try-with-resources = auto close
* custom exception = business rule
* return in finally dangerous

---

# 15) Interview Answer Framework

For exception questions answer in this order:

1. definition
2. hierarchy
3. checked vs unchecked
4. code example
5. propagation
6. best practice
7. trap point

---

# ✅ Day 4 Completion Checklist

* [ ] hierarchy clear
* [ ] checked vs unchecked
* [ ] try-catch-finally
* [ ] throw vs throws
* [ ] propagation
* [ ] multi catch
* [ ] tricky finally cases
* [ ] try-with-resources
* [ ] custom exceptions
* [ ] best practices

---

> **Next recommended topic:** Day 5 → Multithreading, Thread Lifecycle, Synchronization, `volatile`, Executor Framework
