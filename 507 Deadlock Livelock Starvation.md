

#### 🔥 JAVA MULTITHREADING — DEADLOCK, LIVELOCK, STARVATION (IN-DEPTH)

#### ✅ Q17. What is Deadlock?

> [!NOTE]
> A deadlock is a situation where two or more threads are permanently blocked, each waiting for a resource held by another, so none of them can proceed.

* In deadlock:
* Threads are alive, but the system is stuck forever.

#### 🔁 Classic Deadlock Scenario
* Thread A holds Lock 1 → waits for Lock 2
* Thread B holds Lock 2 → waits for Lock 1
* ➡ Circular waiting → deadlock

#### 🔍 Follow-ups (Q17)

#### ➤ 4 Coffman Conditions for Deadlock (🔥 VERY IMPORTANT)

##### Deadlock can occur only if all four conditions are true:

##### 1️⃣ Mutual Exclusion

* Resource can be held by only one thread at a time

##### 2️⃣ Hold and Wait

* Thread holds one resource while waiting for another

##### 3️⃣ No Preemption

* Resource cannot be forcibly taken away

##### 4️⃣ Circular Wait

* Threads form a circular chain of waiting

> [!NOTE]
> 👉 Rule: Break any one of these → deadlock is prevented.

##### ➤ Example Code (Deadlock)

```java
class DeadlockExample {
    static final Object lock1 = new Object();
    static final Object lock2 = new Object();

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                sleep();
                synchronized (lock2) {
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock2) {
                sleep();
                synchronized (lock1) {
                }
            }
        });

        t1.start();
        t2.start();
    }

    static void sleep() {
        try { Thread.sleep(100); } catch (Exception e) {}
    }
}

```
#### 👉 Both threads wait forever.

#### ➤ How to Detect Deadlock?

###### 1️⃣ Thread Dump (MOST COMMON)
* jstack <pid>
* Shows “Found one Java-level deadlock”
* Lists locked monitors and waiting threads
  
###### 2️⃣ Monitoring Tools
* VisualVM
* JConsole
* Java Flight Recorder
###### 3️⃣ Logs + Hanging Threads
* Requests stuck
* CPU low but app unresponsive

#### ➤ How to Prevent Deadlock?

###### ✅ 1️⃣ Lock Ordering (BEST PRACTICE)

* Always acquire locks in same order
synchronized(lock1) {
    synchronized(lock2) {
    }
}

###### ✅ 2️⃣ Avoid Nested Locks
* Reduce synchronized blocks
* Keep critical sections small
  
###### ✅ 3️⃣ Use Timeouts
lock.tryLock(1, TimeUnit.SECONDS);

###### ✅ 4️⃣ Use Higher-Level Concurrency APIs
* ExecutorService
* BlockingQueue
* ConcurrentHashMap
  
> [!NOTE]
> 🧠 Deadlocks are design problems, not coding mistakes.

#### ✅ Q18. What is Livelock?

> [!NOTE]
> A livelock occurs when threads are not blocked, but they continuously change state in response to each other, making no progress.

* In livelock:
* Threads are active, but the system is busy doing nothing useful.

#### 🔁 Livelock Example (Conceptual)
* Thread A → “You go first”
* Thread B → “No, you go first”
* Both keep yielding → infinite loop

#### ➤ Difference Between Deadlock and Livelock

| Aspect | Deadlock | Livelock |
|---|---|---|
| Thread State | `BLOCKED` | `RUNNABLE` |
| CPU Usage | Low | High |
| Progress | None | None |
| Thread Behavior | Waiting | Actively retrying |

#### ➤ Real-World Example

#### 🚶 Two people trying to pass in a narrow hallway

* Both step left → block
* Both step right → block
* They keep adjusting → livelock

#### ➤ How to Fix Livelock?
* Add random delay
* Backoff strategy
* Limit retries

#### ✅ Q19. What is Starvation?

> [!NOTE]
> Starvation occurs when a thread never gets CPU or resources because other threads continuously take precedence.

* Thread is ready but never scheduled.

#### 🔍 Follow-ups (Q19)

#### ➤ Difference Between Starvation and Deadlock

| Aspect | Starvation | Deadlock |
|---|---|---|
| Cause | Resource unfairness | Circular wait |
| Thread State | `RUNNABLE` | `BLOCKED` |
| Recovery | Possible | Impossible without intervention |

#### ➤ How Thread Priority Causes Starvation

* highPriorityThread.setPriority(10);
* lowPriorityThread.setPriority(1);
* High-priority thread dominates CPU
* Low-priority thread rarely executes
> [!NOTE]
> 👉 Common in priority-based scheduling

#### ➤ How to Prevent Starvation?
* ✅ 1️⃣ Avoid Priority-Based Logic
* Do not rely on thread priority for correctness
* ✅ 2️⃣ Fair Locks
* new ReentrantLock(true); // fair lock
* ✅ 3️⃣ Use Thread Pools
* Controlled scheduling
* Balanced execution
* ✅ 4️⃣ Reduce Lock Contention
* Smaller critical sections
* Better data partitioning
  
#### 🧠 COMMON TRAPS

* ❌ Deadlock = infinite loop → WRONG
* ❌ Livelock = deadlock → WRONG
* ❌ Priority guarantees execution → WRONG
* ❌ notify() always safe → WRONG

> [!NOTE]
> 🎯 Deadlock blocks threads permanently, livelock keeps them busy without progress, and starvation denies them resources indefinitely. All three are symptoms of poor concurrency design and are best avoided using disciplined locking strategies and higher-level concurrency utilities.

---
---
