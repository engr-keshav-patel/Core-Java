

# 🔥 JAVA MULTITHREADING — INTER-THREAD COMMUNICATION (IN-DEPTH)

## ✅ Q15. What is Inter-Thread Communication?

> [!NOTE]
> Inter-thread communication is a mechanism that allows multiple threads to coordinate their execution by temporarily pausing and resuming each other based on shared conditions.

> It is primarily used when:
> * One thread produces data
> * Another thread consumes or depends on that data

* Java provides three core methods for this:
* wait()
* notify()
* notifyAll()
  
#### 🧠 Why Inter-Thread Communication Is Needed

#### Without it:

* Threads either busy-wait (waste CPU)
* Or proceed incorrectly (consume before produce)

#### With it:

* Threads sleep efficiently
* Resume only when condition changes

> [!NOTE]
> 👉Inter-thread communication enables cooperation, not competition, between threads.

🔹 Core Methods

#### 1️⃣ wait()
* Causes the current thread to release the lock
* Thread enters WAITING state
* Wakes up only when:
* notify() / notifyAll() is called OR it is interrupted
  
#### 2️⃣ notify()
* Wakes up one arbitrary waiting thread
* Choice of thread is not guaranteed
  
#### 3️⃣ notifyAll()
* Wakes up all waiting threads
* Threads compete again for the lock

#### 🔍 Follow-ups (Q15)
#### ➤ Why are wait(), notify(), notifyAll() in Object class?

Because:
* These methods work on monitor locks
* Monitor locks belong to objects, not threads
* Any object can act as a lock

> [!NOTE]
>  If these methods were in Thread:
> A thread wouldn’t know which object’s lock to release

> [!NOTE]
> 👉 Locks are object-based, not thread-based — hence these methods belong to Object.

➤ Difference Between notify() and notifyAll()

| Aspect | `notify()` | `notifyAll()` |
|---|---|---|
| Threads Awakened | One | All |
| Thread Selection | Random waiting thread | All waiting threads |
| Risk | Missed signals | Higher contention |
| Safety | Risky | Safer |

> [!NOTE]
> 👉 Advice: Use notifyAll() unless you are 100% sure only one thread should wake.

#### ➤ What is Spurious Wakeup?

> [!NOTE]
> Definition : A spurious wakeup happens when a thread:
> * Wakes up from wait()
> * Without notify() or notifyAll()

* This is allowed by the JVM specification.

#### ➤ Why is wait() Called Inside a Loop?

```java
❌ WRONG
if (!condition) {
    lock.wait();
}
✅ CORRECT
while (!condition) {
    lock.wait();
}
```
#### Reasons:

* 1️⃣ Handle spurious wakeups
* 2️⃣ Re-check condition after waking
* 3️⃣ Multiple threads may wake up
* 4️⃣ Prevents invalid state execution

> [!NOTE]
> 👉 wait() is always used in a loop to re-validate the condition.

#### ✅ Q16. Producer–Consumer Problem

> [!NOTE]
> The Producer–Consumer problem is a classic concurrency problem where:
> Producer thread generates data
> Consumer thread processes data
> Both share a bounded buffer

> [!NOTE]
> The challenge is to ensure:
> Producer doesn’t produce when buffer is full
> Consumer doesn’t consume when buffer is empty

#### 🔹 Implementation Using wait() / notifyAll()

#### Example (Simplified)

```java
class Buffer {
    private int data;
    private boolean hasData = false;

    synchronized void produce(int value) throws InterruptedException {
        while (hasData) {
            wait();
        }
        data = value;
        hasData = true;
        notifyAll();
    }

    synchronized int consume() throws InterruptedException {
        while (!hasData) {
            wait();
        }
        hasData = false;
        notifyAll();
        return data;
    }
}
```

#### Key Observations
* while used, not if
* Lock released during wait()
* notifyAll() avoids missed signals
  
#### 🔍 Follow-ups (Q16)

#### ➤ Using BlockingQueue (BEST PRACTICE)

```java
BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

queue.put(10);  // Producer
queue.take();   // Consumer
```

#### Why Better?
* No manual synchronization
* No wait/notify complexity
* Less error-prone
* Production-ready

> [!NOTE]
> 👉 verdict: Prefer BlockingQueue over wait/notify in real systems.

#### ➤ Real-World Analogy

#### 🏭 Factory Conveyor Belt

* Producer → puts items
* Consumer → picks items
* Conveyor capacity → buffer size
* Full belt → producer waits
* Empty belt → consumer waits
  
#### ➤ Problems If notify() Is Used Incorrectly

* ❌ Missed notification
* ❌ Deadlock-like waiting
* ❌ Thread starvation
* ❌ Only wrong thread wakes up

#### Example:

* Consumer wakes another consumer
* Producer never wakes
* System stalls

> [!NOTE]
> 👉 Insight: Many production deadlocks are caused by incorrect notify usage.

🧠 COMMON TRAPS

* ❌ Calling wait() without synchronized → IllegalMonitorStateException
* ❌ Using if instead of while
* ❌ Using notify blindly
* ❌ Forgetting shared condition variable
* ❌ Mixing sleep with wait

> [!NOTE]
> 🎯 Inter-thread communication enables efficient coordination by suspending threads until conditions are met, but misuse of wait/notify easily leads to subtle and hard-to-debug concurrency bugs. Hence, higher-level concurrency utilities are preferred in modern Java.

---
---
