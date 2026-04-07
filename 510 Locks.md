# 🔥 JAVA MULTITHREADING — LOCKS FRAMEWORK (IN-DEPTH)

---

## ✅ Q23. Difference between synchronized and Lock

### High-Level Interview Answer

* Both synchronized and Lock provide mutual exclusion, but Lock offers more flexibility, control, and advanced features, making it suitable for high-performance concurrent systems.

---

## 🔍 Comparison Table

| Aspect              | synchronized     | Lock                               |
| ------------------- | ---------------- | ---------------------------------- |
| Type                | Language keyword | API (`java.util.concurrent.locks`) |
| Lock acquisition    | Implicit         | Explicit (`lock()`)                |
| Lock release        | Automatic        | Manual (`unlock()`)                |
| Interruptible       | ❌ No             | ✅ Yes                              |
| tryLock             | ❌ No             | ✅ Yes                              |
| Fairness            | ❌ No             | ✅ Optional                         |
| Multiple conditions | ❌ No             | ✅ Yes                              |
| Deadlock risk       | Lower            | Higher if misused                  |

### 🧠 Interview Insight

* synchronized is simple and safe, but Lock is powerful and flexible.

---

## 🔍 Follow-ups (Q23)

### ➤ ReentrantLock Features (🔥 VERY IMPORTANT)

* ReentrantLock provides:

  1. Reentrancy
  2. Fairness policy
  3. Interruptible locking
  4. `tryLock()` with timeout
  5. Multiple `Condition` objects

---

### ➤ Fair vs Unfair Lock

#### 🔹 Fair Lock

* Threads acquire lock in FIFO order
* Prevents starvation
* Slightly slower

```java
new ReentrantLock(true); // fair
```

#### 🔹 Unfair Lock (Default)

* No ordering guarantee
* Higher throughput
* Possible starvation

```java
new ReentrantLock(); // unfair
```

> 👉 **Interview line:**
> Fair locks trade performance for predictability.

---

### ➤ tryLock()

```java
if (lock.tryLock()) {
    try {
        // critical section
    } finally {
        lock.unlock();
    }
}
```

* Acquires lock only if immediately available
* Avoids blocking
* Useful in deadlock prevention

---

### ➤ Lock Interruptibility

```java
lock.lockInterruptibly();
```

* Thread can be interrupted while waiting
* synchronized threads cannot be interrupted while blocked

> 👉 Real-world use: graceful shutdown, cancellation.

---

## ✅ Q24. What is ReentrantLock?

### Interview-Level Definition

* ReentrantLock is a mutual exclusion lock that allows a thread to acquire the same lock multiple times without blocking itself.
* A thread that already holds the lock can re-enter it.

### ➤ Why Reentrant?

* Without reentrancy:

  * Recursive calls would deadlock
  * One synchronized method calling another would block

### 🔹 Example

```java
ReentrantLock lock = new ReentrantLock();

void outer() {
    lock.lock();
    try {
        inner();
    } finally {
        lock.unlock();
    }
}

void inner() {
    lock.lock();
    try {
        System.out.println("Reentered lock");
    } finally {
        lock.unlock();
    }
}
```

### ➤ Reentrancy in synchronized

```java
synchronized void outer() {
    inner();
}

synchronized void inner() {
}
```

* Java’s intrinsic locks are also reentrant

> 👉 **Interview insight:**
> Reentrancy is a property of the lock, not the thread.

### ➤ Real-World Example

#### 📦 Bank Account

* `withdraw()` calls `validateBalance()`
* Both synchronized on same account
* Reentrancy avoids deadlock

---

## ✅ Q25. What is ReadWriteLock?

### Interview-Level Definition

* ReadWriteLock allows:

  * Multiple threads to read concurrently
  * Only one thread to write exclusively

* Improves performance when reads are frequent and writes are rare.

### 🔹 Interface & Implementation

```java
ReadWriteLock lock = new ReentrantReadWriteLock();
```

* `readLock()`
* `writeLock()`

---

## 🔍 Follow-ups (Q25)

### ➤ When to Use ReadWriteLock?

* Use when:

  * Read operations >> write operations
  * Data is mostly read-only
  * Writes must be exclusive

* Examples:

  * Cache
  * Configuration data
  * In-memory lookup tables

---

### ➤ Performance Benefits

| Scenario     | synchronized | ReadWriteLock |
| ------------ | ------------ | ------------- |
| Many readers | Poor         | Excellent     |
| Few writers  | OK           | Better        |
| Contention   | High         | Reduced       |

> 👉 **Interview one-liner:**
> ReadWriteLock increases concurrency by separating read and write access.

---

### ➤ Example Scenario

```java
class Cache {
    private final ReadWriteLock lock = new ReentrantReadWriteLock();
    private Map<String, String> data = new HashMap<>();

    String read(String key) {
        lock.readLock().lock();
        try {
            return data.get(key);
        } finally {
            lock.readLock().unlock();
        }
    }

    void write(String key, String value) {
        lock.writeLock().lock();
        try {
            data.put(key, value);
        } finally {
            lock.writeLock().unlock();
        }
    }
}
```

---

## 🧠 COMMON INTERVIEW TRAPS

* ❌ Forgetting `unlock()` in finally
* ❌ Assuming Lock is always better
* ❌ Using fair locks everywhere
* ❌ Mixing synchronized and Lock on same resource

---

## 🎯 STRONG INTERVIEW CLOSING STATEMENT

* The Locks framework provides advanc
