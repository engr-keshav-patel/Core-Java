

# 🔥 JAVA MULTITHREADING — SYNCHRONIZATION (IN-DEPTH)
## ✅ Q11. What is Synchronization?
Interview-Level Definition

> [!NOTE]
> Synchronization is a mechanism in Java that controls access to shared resources so that only one thread can execute a critical section at a time, thereby maintaining data consistency and thread safety.

> In simple terms:
> Synchronization prevents multiple threads from corrupting shared data.

* ✅ Example (Problem Without Synchronization)

```java
class Counter {
    int count = 0;

    void increment() {
        count++; // NOT atomic
    }
}
```

* If two threads execute increment() simultaneously:

* Both read the same value
* Both update it
* One update is lost

> 👉 This is a race condition.

#### 🔍 Follow-ups (Q11)

#### ➤ Why is Synchronization Needed?

* Because:

> Threads share heap memory
Operations like count++ are not atomic
CPU may interleave execution

* Without synchronization:

* Inconsistent data
* Corrupted state
* Hard-to-reproduce bugs

> [!NOTE] 👉 Synchronization ensures mutual exclusion and visibility guarantees.

#### ➤ What is a Critical Section?

* A critical section is the part of code that:

* Accesses shared mutable data
Must be executed by only one thread at a time

```java

synchronized(lock) {
    // critical section
}
```

> [!NOTE]
> 👉 Identifying the correct critical section is key to performance.

#### ➤ What Happens if Synchronization is Removed?

* ❌ Data inconsistency
* ❌ Race conditions
* ❌ Lost updates
* ❌ Visibility issues

> 👉 In production, this leads to random failures that are extremely hard to debug.

#### ✅ Q12. Different Ways to Achieve Synchronization

##### Java provides three intrinsic locking mechanisms.

##### 1️⃣ Synchronized Method

Example

```java
class Counter {
    synchronized void increment() {
        count++;
    }
}
```

> Explanation : 
> Lock is acquired on current object (this)
Only one thread can execute any synchronized method of that object

##### 2️⃣ Synchronized Block (Preferred)

Example
```java
void increment() {
    synchronized(this) {
        count++;
    }
}
}
```

* Why Better?
* Smaller critical section
* Better performance
* More control

> [!TIP]
>👉 Synchronize the minimum required code, not the whole method.

##### 3️⃣ Static Synchronization

Example
```java
class Counter {
    static synchronized void increment() {
        count++;
    }
}
```

> Explanation : 
> Lock is acquired on Class object
Shared across all instances

#### 🔍 Follow-ups (Q12)

#### ➤ Method vs Block Synchronization

| Aspect | Synchronized Method | Synchronized Block |
|---|---|---|
| Scope | Whole method | Selected code block |
| Flexibility | Low | High |
| Performance | Lower | Better |
| Preferred | ❌ No | ✅ Yes |

#### ➤ Object Lock vs Class Lock

| Lock Type | Locked On | Used When |
|---|---|---|
| Object Lock | `this` / object instance | Instance-level data |
| Class Lock | `ClassName.class` | Static / shared data |

#### ➤ When Is Static Synchronization Needed?
* When shared data is static
* When consistency must be maintained across all objects
* static int totalUsers;

> [!NOTE]
> 👉 Instance lock won’t protect static data.

> ✅ Q13. What is Intrinsic Lock (Monitor Lock)?

> [!NOTE]
> An intrinsic lock (also called monitor lock) is an internal lock associated with every Java object, used by synchronized.

> Every object in Java can act as a monitor.

Example
```java
synchronized(obj) {
    // obj’s monitor lock acquired
}
```

#### 🔍 Follow-ups (Q13)

#### ➤ Which Object Owns the Lock?
* The object used in synchronized(obj)
* For instance method → this
* For static method → ClassName.class

#### ➤ What If Two Threads Use Different Objects?
```java
Counter c1 = new Counter();
Counter c2 = new Counter();
```

* If threads synchronize on c1 and c2:

* ❌ No mutual exclusion
* Threads run concurrently
* Data corruption possible

> [!NOTE]
> 👉 Locks work per object, not per class unless static.

➤ Is this a Lock?

✅ Yes
```java
synchronized(this) {
}
```

* Current object’s monitor is used
* Common but must be used carefully
  
#### ✅ Q14. Can We Synchronize a Constructor?

❌ NO

#### ➤ Why Not?
> [!NOTE]
> Constructor is used to create the object
Lock exists only after object is created
Other threads cannot access object before construction completes

👉 Therefore, constructor synchronization is meaningless.

➤ Alternative Approaches

#### 1️⃣ Factory Method

```java
public static synchronized MyClass createInstance() {
    return new MyClass();
}
```

#### 2️⃣ Initialization Block

```java
{
    synchronized(lock) {
        // init logic
    }
}
```

#### 3️⃣ Proper Object Publication

* Use final fields
* Use volatile references
* Safe publication through constructors
  
🧠 COMMON TRAPS

* ❌ Synchronizing everything
* ❌ Synchronizing wrong object
* ❌ Using this blindly
* ❌ Forgetting static synchronization
* ❌ Assuming synchronization solves performance issues

> [!NOTE]
> 🎯 Synchronization guarantees correctness but reduces concurrency. A good design balances thread safety with performance by minimizing critical sections and using higher-level concurrency utilities when possible.

---
---
