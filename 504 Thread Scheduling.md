

# 🔥 JAVA MULTITHREADING — THREAD SCHEDULING & PRIORITY (IN-DEPTH)

## ✅ Q9. What is Thread Scheduling?


> [!NOTE]
> Thread scheduling is the mechanism by which the CPU decides which runnable thread gets execution time and for how long.

> [!NOTE]
>  In Java:
> Threads are scheduled preemptively
Scheduling is not deterministic
JVM delegates scheduling largely to the underlying OS

> [!NOTE]
> 👉 Key Idea:
> * Java decides what is runnable,
> * OS decides what actually runs.

#### 🔄 How Scheduling Works (Conceptually)

* Multiple threads are in RUNNABLE state
* OS scheduler selects one thread
* Thread executes for a time slice
* Context switch occurs
* Another thread gets CPU
  
#### 🔍 Follow-ups (Q9)
#### ➤ Is Thread Scheduling Guaranteed?

❌ NO

* Execution order is never guaranteed
* JVM spec explicitly avoids guarantees


> [!NOTE]
> Java provides concurrency, not parallelism guarantees.

#### ➤ Who Schedules Threads?

✅ Primarily the Operating System

* JVM maps Java threads to native OS threads
* OS kernel scheduler performs:
* CPU allocation
* Time slicing
* Preemption

* JVM only provides hints (priority, yield)

#### ➤ Is Scheduling OS-dependent or JVM-dependent?

✅ OS-dependent

* Linux, Windows, macOS → different schedulers
* Same Java code behaves differently on different OS

> [!NOTE]
> 👉 Java threading behavior can vary across platforms.

#### ➤ Can We Control Thread Scheduling?

❌ Not directly

* We can only influence, not control:

* setPriority()
* yield()
* sleep()

* But JVM/OS may ignore them

#### ➤ What About Thread.yield()?

* Hints scheduler to give CPU to another thread
* No guarantee
* Rarely useful in production

#### ✅ Q10. What is Thread Priority?

> [!NOTE]
>  Thread priority is a hint to the thread scheduler about the relative importance of a thread.

> Java priority influences CPU preference, not execution order.

#### 🔢 Priority Range in Java
* Thread.MIN_PRIORITY  = 1
* Thread.NORM_PRIORITY = 5 (default)
* Thread.MAX_PRIORITY  = 10
  
#### ➤ Default Priority
5
* Child thread inherits parent’s priority
➤ Example
```java
Thread t1 = new Thread(task);
t1.setPriority(Thread.MAX_PRIORITY);

Thread t2 = new Thread(task);
t2.setPriority(Thread.MIN_PRIORITY);
```

* 👉 Even here, execution order is not guaranteed.

#### 🔍 Follow-ups (Q10)
#### ➤ Does Priority Guarantee Execution Order?

❌ NO

* Higher priority thread may not run first
* Lower priority thread may starve or may run

* Priority is a suggestion, not a command.

#### ➤ Why Is Thread Priority Unreliable?

* 1️⃣ OS Scheduler Overrides
* OS may ignore Java priority
* Especially true on modern OS
* 2️⃣ Platform Differences
* Windows honors priority more than Linux
* Linux often ignores JVM priority mapping
* 3️⃣ JVM Optimizations
* JVM may reorder or batch execution

#### ➤ Can Priority Cause Starvation?

✅ YES

* High-priority thread monopolizes CPU
* Low-priority threads may not get CPU time

> [!NOTE]
> This is why priority-based design is discouraged.

🧠 Real-World Insight

* ❌ Using priority for business logic
* ❌ Assuming execution order
* ❌ Relying on yield/sleep for correctness

✅ Use:

* Proper synchronization
* Executor framework
* Fair locks if ordering matters
  
> [!NOTE]
> 🎯 Java thread scheduling is intentionally non-deterministic to allow JVM and OS optimizations. Correct multithreaded programs must never rely on execution order or priority.

---
---
