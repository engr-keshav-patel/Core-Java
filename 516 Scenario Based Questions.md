# 🔥 JAVA MULTITHREADING — SCENARIO-BASED QUESTIONS (MASTER INTERVIEW NOTES)

---

## 🔥 SCENARIO 1: Multiple Threads Updating Same Variable

### ❓ Scenario

Two threads are incrementing a shared counter.

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

### ❓ Interviewer Asks

* What is the problem here?
* Will the final count always be correct?
* Why does this happen internally?
* How do you fix it?
* What is the best approach?

### ✅ Interview-Grade Answer

#### 1️⃣ What Is the Problem Here?

* The problem is a race condition.
* A race condition occurs when multiple threads access and modify shared data concurrently without proper synchronization, leading to inconsistent results.

#### 2️⃣ Why Is `count++` Problematic?

* Because `count++` is NOT atomic.

* Internally, `count++` breaks into three CPU-level steps:

  1. Read count from memory
  2. Increment value
  3. Write updated value back to memory

* If two threads interleave like this:

  * Thread A reads count = 5
  * Thread B reads count = 5
  * Thread A writes count = 6
  * Thread B writes count = 6

➡️ One increment is lost.

#### 3️⃣ Will the Final Count Always Be Correct?

* ❌ NO
* Sometimes it will be correct
* Sometimes it will be lower than expected
* The bug is non-deterministic

> 👉 Interview keyword: Heisenbug (appears/disappears unpredictably)

#### 4️⃣ How to Fix It?

##### ✅ Fix #1: synchronized (Correct but Heavy)

```java
class Counter {
    int count = 0;

    synchronized void increment() {
        count++;
    }
}
```

##### Why This Works

* Only one thread at a time can execute increment()
* Guarantees:

  * Mutual exclusion
  * Visibility
  * Atomicity

##### Downside

* Thread contention
* Performance degrades under high concurrency

##### ✅ Fix #2: AtomicInteger (BEST for Simple Counters)

```java
class Counter {
    AtomicInteger count = new AtomicInteger(0);

    void increment() {
        count.incrementAndGet();
    }
}
```

##### Why This Is Better

* Lock-free
* Uses CAS (Compare-And-Swap)
* High performance under low/moderate contention

> 👉 Strong interview line:
> AtomicInteger provides atomicity and visibility without blocking.

##### ✅ Fix #3: Lock (When You Need Control)

```java
class Counter {
    int count = 0;
    Lock lock = new ReentrantLock();

    void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
}
```

##### When to Use

* Interruptible locking
* Timeouts (`tryLock`)
* Fairness requirements

#### 5️⃣ What Is the Best Approach?

| Scenario                   | Best Choice   |
| -------------------------- | ------------- |
| Simple counter             | AtomicInteger |
| Multiple related variables | synchronized  |
| High contention counter    | LongAdder     |
| Complex locking rules      | ReentrantLock |

### 🎯 Interview-Ready Final Answer (2–3 Lines)

* This code has a race condition because `count++` is not atomic. Multiple threads can read the same value and overwrite each other’s updates. The fix is to ensure atomicity using synchronization or atomic variables. For a simple counter, `AtomicInteger` is the most efficient and clean solution.

### 🧠 Interview Traps (Avoid These ❌)

* ❌ “volatile will fix this” → WRONG (visibility ≠ atomicity)
* ❌ “It works most of the time” → WRONG
* ❌ “count++ is thread-safe” → WRONG

---

## 🔥 SCENARIO 2: Singleton Class in Multithreaded Environment

### ❓ Scenario

Lazy-loaded Singleton implementation:

```java
class Singleton {
    private static Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### ❓ Interviewer Asks

* Is this Singleton thread-safe?
* What exactly can go wrong?
* Why does it happen internally?
* How can you fix it?
* What is the best Singleton implementation in Java?

### ✅ Interview-Grade Answer

#### 1️⃣ Is This Singleton Thread-Safe?

* ❌ NO, it is NOT thread-safe
* Multiple threads can create multiple instances.

#### 2️⃣ What Can Go Wrong? (Exact Failure Scenario)

* Assume two threads: T1 and T2
* T1 enters `getInstance()`

  * → `instance == null` ✔
* (context switch)
* T2 enters `getInstance()`

  * → `instance == null` ✔
  * → creates new `Singleton()`
  * → assigns instance
* (context switch)
* T1 resumes

  * → creates new `Singleton()`
  * → assigns instance again

➡️ Two different Singleton objects are created

#### 3️⃣ Why Does This Happen Internally?

* Because:

  * `if (instance == null)` is not atomic
  * There is no synchronization
  * Threads can interleave execution
  * JVM allows instruction reordering

> 👉 Interview keyword: Race condition during lazy initialization

#### 4️⃣ How to Fix It?

##### ✅ Fix #1: Synchronized Method (Correct but Slow)

```java
class Singleton {
    private static Singleton instance;

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

##### ✅ Fix #2: Double-Checked Locking (MOST ASKED)

```java
class Singleton {
    private static volatile Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

##### ✅ Fix #3: Bill Pugh Singleton (BEST LAZY APPROACH)

```java
class Singleton {

    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```

##### ✅ Fix #4: Enum Singleton (🔥🔥 BEST ANSWER)

```java
enum Singleton {
    INSTANCE;
}
```

#### 5️⃣ What Is the BEST Implementation?

| Rank           | Implementation         |
| -------------- | ---------------------- |
| 🥇 Best        | Enum Singleton         |
| 🥈 Best (Lazy) | Bill Pugh              |
| 🥉 Acceptable  | Double-Checked Locking |
| ❌ Avoid        | Simple lazy singleton  |

---

## 🔥 SCENARIO 3: Deadlock Scenario

### ❓ Scenario

Two threads, two locks, acquired in different order.

```java
synchronized (lock1) {
    synchronized (lock2) {
        // critical section
    }
}
```

and

```java
synchronized (lock2) {
    synchronized (lock1) {
        // critical section
    }
}
```

### ✅ Interview-Grade Answers

#### 1️⃣ What Is Deadlock?

* A deadlock is a situation where two or more threads are permanently blocked, each waiting for a resource held by another thread, so none of them can proceed.

#### 2️⃣ Conditions for Deadlock (🔥 VERY IMPORTANT)

* 1️⃣ Mutual Exclusion
* 2️⃣ Hold and Wait
* 3️⃣ No Preemption
* 4️⃣ Circular Wait

> 👉 Interview rule: Break any one of these → deadlock is prevented.

#### 3️⃣ How to Prevent Deadlock? (🔥 INTERVIEW GOLD)

* ✅ Prevention #1: Lock Ordering (BEST PRACTICE)
* ✅ Prevention #2: Avoid Nested Locks
* ✅ Prevention #3: Timeout Locks
* ✅ Prevention #4: Use High-Level Concurrency APIs

---

## 🔥 SCENARIO 4: wait() vs sleep() Confusion

### ❓ Scenario

```java
Object lock = new Object();
lock.wait();   // ❌
```

### ✅ Interview-Grade Answers

#### 1️⃣ What Happens at Runtime?

* ❌ The program throws an exception immediately.
* Exception: `IllegalMonitorStateException`

#### 2️⃣ Correct Usage of `wait()`

```java
synchronized (lock) {
    while (!condition) {
        lock.wait();
    }
}
```

#### 3️⃣ `wait()` vs `sleep()`

| Aspect             | wait()              | sleep()         |
| ------------------ | ------------------- | --------------- |
| Belongs to         | Object              | Thread          |
| Releases lock      | ✅ Yes               | ❌ No            |
| Needs synchronized | ✅ Yes               | ❌ No            |
| Purpose            | Thread coordination | Pause execution |

---

## 🔥 SCENARIO 5: Producer–Consumer Problem

### ✅ Correct & Interview-Approved Implementation

```java
class Buffer {
    private int data;
    private boolean hasData = false;

    public synchronized void produce(int value) throws InterruptedException {
        while (hasData) {
            wait();
        }
        data = value;
        hasData = true;
        notifyAll();
    }

    public synchronized int consume() throws InterruptedException {
        while (!hasData) {
            wait();
        }
        hasData = false;
        notifyAll();
        return data;
    }
}
```

### ✅ Better / Modern Solution (🔥 VERY IMPORTANT)

```java
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

queue.put(10);
int value = queue.take();
```

---

## 🔥 SCENARIO 6: Thread Safety in Web Applications

### ✅ Key Answer

* Spring controllers are singleton by default.
* Local variables are thread-safe.
* Shared instance fields are NOT thread-safe.
* Best practice:

  * Stateless controllers/services
  * Immutable data
  * `AtomicInteger`
  * `ConcurrentHashMap`

---

## 🔥 SCENARIO 7: Volatile Visibility Issue

### ❓ Scenario

```java
boolean flag = true;

while (flag) {
    // do something
}
```

### ✅ Fix

```java
volatile boolean flag = true;
```

### ✅ Key Insight

* Problem = visibility issue
* `volatile` ensures:

  * reads from main memory
  * writes to main memory
  * prevents reordering

---

## 🔥 SCENARIO 8: notify() vs notifyAll()

### ✅ Core Difference

| Method      | What it does                       |
| ----------- | ---------------------------------- |
| notify()    | Wakes one arbitrary waiting thread |
| notifyAll() | Wakes all waiting threads          |

> 👉 If in doubt, use `notifyAll()`.

---

## 🔥 SCENARIO 9: Thread Pool Exhaustion

### ✅ Safest Rejection Policy

* ✅ `CallerRunsPolicy`
* Provides backpressure
* Prevents task loss
* Stabilizes system under load

### ✅ Safe ThreadPoolExecutor

```java
ExecutorService executor =
    new ThreadPoolExecutor(
        10,
        20,
        60, TimeUnit.SECONDS,
        new ArrayBlockingQueue<>(100),
        new ThreadPoolExecutor.CallerRunsPolicy()
    );
```

---

## 🔥 SCENARIO 10: Blocking vs Non-Blocking

### ✅ Common Non-Blocking Option

```java
CompletableFuture<String> future =
    CompletableFuture.supplyAsync(() -> fetchData());
```

### ✅ Reactive Example

```java
Mono<String> result = webClient.get().retrieve().bodyToMono(String.class);
```

---

## 🔥 SCENARIO 11: Application Slows Down After Adding synchronized

### ✅ Better Alternatives

| Situation           | Better Choice          |
| ------------------- | ---------------------- |
| Simple counter      | AtomicInteger          |
| Read-heavy workload | ReadWriteLock          |
| Complex locking     | ReentrantLock          |
| Shared collections  | Concurrent collections |
| Stateless logic     | No synchronization     |

---

## 🔥 SCENARIO 12: Thread Interruption (Graceful Shutdown)

### ✅ Correct Handling

```java
class Worker implements Runnable {
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            // do work
        }
    }
}
```

### ✅ Rule

> Always restore interrupt status when catching `InterruptedException`.

```java
Thread.currentThread().interrupt();
```

---

## 🔥 SCENARIO 13: Parallel Stream Gives Wrong Result

### ❌ Wrong

```java
numbers.parallelStream().forEach(n -> sum += n);
```

### ✅ Correct

```java
int sum = numbers.parallelStream()
                 .mapToInt(Integer::intValue)
                 .sum();
```

---

## 🔥 SCENARIO 14: Daemon Thread Stops Abruptly

### ✅ Correct Design Approach

* Use user threads for critical work
* Use daemon threads only for:

  * GC
  * Monitoring
  * Cache cleanup
* Use graceful shutdown hooks

```java
Runtime.getRuntime().addShutdownHook(new Thread(() -> {
    flushLogs();
}));
```

---

## 🔥 PRODUCER–CONSUMER — SCENARIO-BASED INTERVIEW QUESTIONS

### 🔹 SCENARIO 1: Basic Producer–Consumer Coordination

* Producer waits if buffer is full
* Consumer waits if buffer is empty
* Use `wait()` + `notify()`

### 🔹 SCENARIO 2: Using if Instead of while

```java
while (bufferFull) {
    wait();
}
```

### 🔹 SCENARIO 3: notify() vs notifyAll() Bug

* `notify()` may wake wrong thread
* `notifyAll()` is safer

### 🔹 SCENARIO 4: Multiple Producers and Consumers

* Prefer `notifyAll()`
* Better → `BlockingQueue`

### 🔹 SCENARIO 5: Interviewer Asks for Code (Classic)

```java
class Buffer {
    private int data;
    private boolean hasData = false;

    public synchronized void produce(int value) throws InterruptedException {
        while (hasData) {
            wait();
        }
        data = value;
        hasData = true;
        notifyAll();
    }

    public synchronized int consume() throws InterruptedException {
        while (!hasData) {
            wait();
        }
        hasData = false;
        notifyAll();
        return data;
    }
}
```

### 🔹 SCENARIO 6: Modern Java Design Question

```java
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
```

### 🎯 INTERVIEWER’S FAVORITE FINAL QUESTION 🔥

* Why is Producer–Consumer hard to implement correctly?
* Because it requires correct coordination, condition checking, and notification ordering. Small mistakes like using `if` instead of `while` or `notify` instead of `notifyAll` can lead to subtle deadlocks.

---
