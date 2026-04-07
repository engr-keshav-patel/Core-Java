

# 🔥 JAVA MULTITHREADING — THREAD LIFECYCLE (IN-DEPTH)
### ✅ Q7. Explain Thread Lifecycle in Java


> [!NOTE]
>  The thread lifecycle represents the different states a thread goes through from creation to termination, as defined by the JVM and exposed via Thread.State.

> A thread does not execute continuously—it transitions between states based on CPU scheduling, locks, and coordination methods.

#### 🧠 Thread Lifecycle States (Java)
#### 1️⃣ NEW

```java
Thread object is created
start() is not yet called
Thread t = new Thread(() -> {});
// State: NEW
```

> [!NOTE]
>  👉 Key Point:
> Memory exists, but no execution stack is created yet.

#### 2️⃣ RUNNABLE


*start() is called

> Thread is ready to run OR running
> JVM uses RUNNABLE for both states

> t.start();
// State: RUNNABLE

> [!NOTE]
>  👉 Java does NOT have a separate RUNNING > state, **RUNNABLE = ready + running**

#### 3️⃣ BLOCKED
>Thread is waiting to acquire a monitor lock.

Happens due to synchronized

```java

synchronized(lock) {
    // thread holds lock
}
```

* Another thread tries to enter the same block → BLOCKED

> [!NOTE]
> 👉 Thread is not sleeping, it is waiting for a lock

#### 4️⃣ WAITING
> Thread waits indefinitely
Requires explicit notification

* Caused by:

* obj.wait();
* thread.join();
* LockSupport.park();

> [!NOTE]
> 👉 Thread releases lock and waits until someone wakes it up

#### 5️⃣ TIMED_WAITING
> Thread waits for a fixed time

* Caused by:

* Thread.sleep(1000);
* obj.wait(1000);
* thread.join(1000);

> [!NOTE]
>👉 After timeout → automatically moves back to RUNNABLE

#### 6️⃣ TERMINATED
* run() method finishes
* Thread execution ends permanently

> [!NOTE]
>👉 A terminated thread cannot be restarted

🔄 Lifecycle Flow (Narrative)
* NEW → start() → RUNNABLE
* RUNNABLE → synchronized lock unavailable → BLOCKED
* RUNNABLE → wait() → WAITING
* RUNNABLE → sleep()/wait(time) → TIMED_WAITING
*RUNNABLE → run() ends → TERMINATED

#### 🔍 Follow-ups (Q7)

#### ➤ Difference Between BLOCKED and WAITING

| Aspect | `BLOCKED` | `WAITING` |
|---|---|---|
| Cause | Waiting for lock | Waiting for notification |
| Lock Released? | ❌ No | ✅ Yes |
| Example | `synchronized` lock contention | `wait()`, `join()` |
| Timeout | ❌ No | ❌ No (unless timed) |

> [!NOTE]
>  👉 BLOCKED is about lock acquisition, WAITING is about coordination.

#### ➤ When does a thread go to BLOCKED?

> When it tries to enter a synchronized block/method
> Another thread already holds the lock

```java
synchronized(lock) {
    // Thread A holds lock
}
// Thread B → BLOCKED
```

#### ➤ Which state occurs during sleep()?

✅ TIMED_WAITING

* Thread.sleep(1000);
* Thread does NOT release lock
* Automatically wakes after time expires

#### ➤ Which state occurs during wait()?

✅ WAITING or TIMED_WAITING

* obj.wait();      // WAITING
* obj.wait(1000); // TIMED_WAITING
* Thread releases lock
* Waits for notify() or timeout

#### ✅ Q8. Difference Between sleep() and wait()

🔥 MOST ASKED MULTITHREADING QUESTION


> sleep() is a thread pausing mechanism,
> wait() is an inter-thread communication mechanism.

#### 🔍 Comparison Table

| Aspect | `sleep()` | `wait()` |
|---|---|---|
| Class | `Thread` | `Object` |
| Lock Release | ❌ No | ✅ Yes |
| Purpose | Pause execution | Inter-thread coordination |
| Needs `synchronized` | ❌ No | ✅ Yes |
| Wake-up | Time-based | `notify()` / `notifyAll()` |
| Exception | `InterruptedException` | `InterruptedException` |

> [!NOTE]
> ➤ Lock Release Behavior (VERY IMPORTANT)

```java
synchronized(lock) {
    Thread.sleep(1000); // lock NOT released
}
synchronized(lock) {
    lock.wait(); // lock released
}
```

> [!NOTE]
>sleep holds the lock, wait hands it over.

#### ➤ Which class they belong to?
* sleep() → Thread
* wait() → Object

#### ➤ Can wait() be called without synchronized?

❌ No
```java
obj.wait(); // ❌ IllegalMonitorStateException
```

👉 Reason:

> [!NOTE]
>Thread must own the object's monitor

#### ➤ Why does wait() exist in Object class?

* Because:

> [!NOTE]
> * Every object can act as a monitor
> * Lock is associated with object, not thread
> * wait/notify operate on monitor ownership

> [!NOTE]
> 👉 If wait() were in Thread:
> Thread wouldn’t know which object lock to release

#### 🧠 COMMON TRAPS

* ❌ “sleep releases lock” → WRONG
* ❌ “wait pauses thread” → Incomplete
* ❌ “BLOCKED and WAITING are same” → WRONG
* ❌ “run() puts thread in RUNNABLE” → WRONG

> [!NOTE]
>  Thread lifecycle understanding is critical because performance issues, deadlocks, and hangs are all caused by incorrect state transitions and lock handling.

---
---
