
# 🔥 JAVA MULTITHREADING — COMPLETE INTERVIEW QUESTION BANK

---

# 1️⃣ Basics of Multithreading
* Q1. What is a thread?
  
* Follow-ups
* Difference between process and thread
* Why threads are called lightweight processes?
* Can a Java program run without threads?
* Is main() a thread?
* Who creates the main thread?
  
* Q2. Why do we need multithreading?

* Follow-ups
* Difference between multitasking and multithreading
* CPU-bound vs IO-bound tasks
* Real-world examples (browser, server, IDE)
* When should we avoid multithreading?
  
* Q3. Advantages and disadvantages of multithreading

* Follow-ups
* Context switching cost
* Memory overhead
* Debugging complexity
* Deadlocks & race conditions

---

# 2️⃣ Thread Creation

* Q4. Ways to create a thread in Java
* Extending Thread
* Implementing Runnable
* Using Callable
* Using ExecutorService

* Follow-ups
* Difference between Thread vs Runnable
* Why Runnable is preferred?
* Can we extend multiple threads?
* Which approach is best and why?
  
* Q5. Difference between Runnable and Callable
* Follow-ups

* Return value
* Exception handling
* How to get result from Callable?
* What is Future?
* Can Runnable throw checked exception?
  
* Q6. What happens internally when we call start()?

* Follow-ups

* Difference between start() and run()
* Can we call start() twice?
* What exception is thrown?
* Who allocates stack memory?

---

# 3️⃣ Thread Lifecycle

* Q7. Explain thread lifecycle in Java

* States:

* NEW
* RUNNABLE
* BLOCKED
* WAITING
* TIMED_WAITING
* TERMINATED

* Follow-ups

* Difference between BLOCKED and WAITING
* When does thread go to BLOCKED?
* Which state occurs during sleep()?
* Which state occurs during wait()?
* Q8. Difference between sleep() and wait()

Follow-ups

Lock release behavior
Which class they belong to?
Can wait() be called without synchronized?
Why wait() exists in Object class?

---

# 4️⃣ Thread Scheduling & Priority

Q9. What is thread scheduling?

Follow-ups

Is scheduling guaranteed?
Who schedules threads?
OS dependent or JVM dependent?
Can we control thread scheduling?
Q10. What is thread priority?

Follow-ups

Priority range in Java
Default priority
Does priority guarantee execution order?
Why priority is unreliable?

---

# 5️⃣ Synchronization
Q11. What is synchronization?

Follow-ups

Why synchronization is needed?
What is critical section?
What happens if synchronization is removed?
Q12. Different ways to achieve synchronization
Synchronized method
Synchronized block
Static synchronization

Follow-ups

Method vs block synchronization
Object lock vs class lock
When static synchronization is needed?
Q13. What is intrinsic lock (monitor lock)?

Follow-ups

Which object owns the lock?
What happens if two threads use different objects?
Is this a lock?
Q14. Can we synchronize a constructor?

Follow-ups

Why not?
Alternative approaches

---

# 6️⃣ Inter-Thread Communication
Q15. What is inter-thread communication?

Methods:

wait()
notify()
notifyAll()

Follow-ups

Why these methods are in Object class?
Difference between notify and notifyAll
What is spurious wakeup?
Why wait is called inside loop?
Q16. Producer-Consumer problem

Follow-ups

Implementation using wait/notify
Using BlockingQueue
Real-world analogy
Problems if notify is used incorrectly

---

# 7️⃣ Deadlock, Livelock, Starvation
Q17. What is deadlock?

Follow-ups

Conditions for deadlock (4 Coffman conditions)
How to detect deadlock?
How to prevent deadlock?
Example code
Q18. What is livelock?

Follow-ups

Difference between deadlock and livelock
Real-world example
Q19. What is starvation?

Follow-ups

Difference between starvation and deadlock
How thread priority causes starvation
How to prevent starvation?

---

# 8️⃣ Volatile Keyword & Memory Model
Q20. What is volatile?

Follow-ups

Visibility vs atomicity
Can volatile replace synchronization?
Happens-before relationship
Use cases of volatile
Q21. Java Memory Model (JMM)

Follow-ups

Heap vs Stack
Main memory vs working memory
Instruction reordering
Happens-before rules

---

# 9️⃣ Atomic Variables
Q22. What are atomic classes?

Follow-ups

AtomicInteger vs synchronized
CAS (Compare And Swap)
ABA problem
Performance comparison

---

# 🔟 Locks Framework
Q23. Difference between synchronized and Lock

Follow-ups

ReentrantLock features
Fair vs unfair lock
tryLock()
Lock interruptibility
Q24. What is ReentrantLock?

Follow-ups

Why reentrant?
Reentrancy in synchronized
Real-world example
Q25. What is ReadWriteLock?

Follow-ups

When to use?
Performance benefits
Example scenario

---

# 1️⃣1️⃣ Executors & Thread Pool
Q26. What is Executor framework?

Follow-ups

Why thread pool?
Problems with creating threads manually
Executor vs ExecutorService
Q27. Types of thread pools
Fixed
Cached
Single
Scheduled
WorkStealingPool

Follow-ups

Which pool for CPU-bound?
Which pool for IO-bound?
What happens when pool is full?
Q28. How thread pool works internally?

Follow-ups

Core pool size
Max pool size
Queue types
Rejection policies
Q29. ThreadPoolExecutor parameters

Follow-ups

CallerRunsPolicy
AbortPolicy
DiscardPolicy
DiscardOldestPolicy

---

# 1️⃣2️⃣ Fork/Join & Parallelism
Q30. What is Fork/Join framework?

Follow-ups

Work stealing algorithm
RecursiveTask vs RecursiveAction
When to use fork/join?
Q31. Parallel streams

Follow-ups

Difference between stream() and parallelStream()
When not to use parallel streams?
Common fork join pool

---

# 1️⃣3️⃣ CompletableFuture
Q32. What is CompletableFuture?

Follow-ups

supplyAsync vs runAsync
thenApply vs thenCompose
thenCombine
Exception handling
Async vs non-async methods

---

# 1️⃣4️⃣ Thread Safety & Immutability
Q33. What is thread safety?

Follow-ups

Ways to achieve thread safety
Immutable objects
Stateless classes
Q34. Why String is immutable?

Follow-ups

Thread safety benefits
String pool
Security implications

---

# 1️⃣5️⃣ Daemon Threads
Q35. What is daemon thread?

Follow-ups

Difference between user and daemon thread
JVM shutdown behavior
Examples (GC)

---

# 1️⃣6️⃣ Common Scenario-Based Questions
Q36. Two threads updating same variable — issue?

Follow-ups

Race condition
How to fix?
Best approach?
Q37. Why wait/notify inside synchronized?

Follow-ups

IllegalMonitorStateException
Monitor ownership
Q38. Web application concurrency issues

Follow-ups

Singleton beans
Controller thread safety
Session vs request scope
Q39. How does Spring handle multithreading?

Follow-ups

@Async
Thread pools in Spring
Thread safety of beans
Q40. Debugging multithreading issues

Follow-ups

Logging issues
Thread dump
Deadlock detection

---


# 🔥 COMMON MULTITHREADING SCENARIO QUESTIONS (INTERVIEW GOLD)

# 1️⃣ Multiple Threads Updating Same Variable
❓ Scenario

Two threads are incrementing a shared counter.

class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
❓ Interviewer Asks
What is the problem here?
Will final count always be correct?
How to fix?
✅ Expected Answer
Problem: Race condition
count++ is not atomic
Multiple threads can read same value
✅ Fix
synchronized method
AtomicInteger
Lock

---

# 2️⃣ Singleton Class in Multithreaded Environment
❓ Scenario

Singleton instance created lazily.

class Singleton {
    private static Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
❓ Follow-ups
Is this thread safe?
How to fix?
Best implementation?
✅ Expected Answer
NOT thread-safe
Multiple threads can create multiple instances
✅ Fix
Synchronized method
Double-checked locking with volatile
Enum singleton (best)

---

# 3️⃣ Deadlock Scenario
❓ Scenario

Two threads, two locks.

synchronized(lock1) {
    synchronized(lock2) { }
}

and

synchronized(lock2) {
    synchronized(lock1) { }
}
❓ Follow-ups
Why deadlock happens?
Conditions of deadlock?
How to prevent?
✅ Expected Answer
Circular wait
Mutual exclusion
Hold and wait
No preemption
✅ Prevention
Lock ordering
Timeout locks
Avoid nested locks

---

# 4️⃣ wait() vs sleep() Confusion
❓ Scenario

Thread calls wait() without synchronized block.

❓ Follow-ups
What happens?
Why?
Correct usage?
✅ Expected Answer
IllegalMonitorStateException
Thread doesn’t own monitor
wait() must be inside synchronized block

---

# 5️⃣ Producer–Consumer Problem
❓ Scenario

One thread produces data, another consumes.

❓ Follow-ups
How will you implement?
Why wait/notify needed?
Alternative?
✅ Expected Answer
Use shared buffer
Producer waits when buffer full
Consumer waits when buffer empty
✅ Better Solution
BlockingQueue (ArrayBlockingQueue, LinkedBlockingQueue)

---

# 6️⃣ Thread Safety in Web Applications
❓ Scenario

Multiple users hitting same REST API.

❓ Follow-ups
Are controllers thread-safe?
What about singleton beans?
How to handle shared data?
✅ Expected Answer
Controllers are singleton
Local variables are thread-safe
Shared fields are NOT
Use stateless design

---

# 7️⃣ Volatile Visibility Issue
❓ Scenario
boolean flag = true;

while(flag) {
}

Another thread sets flag = false.

❓ Follow-ups
Will loop stop?
Why or why not?
Fix?
✅ Expected Answer
May never stop
Visibility issue
Fix using volatile

---

# 8️⃣ notify() vs notifyAll()
❓ Scenario

Multiple threads waiting on same object.

❓ Follow-ups
Difference?
When to use which?
Risk of notify()?
✅ Expected Answer
notify() wakes one random thread
notifyAll() wakes all
notify can cause thread starvation

---

# 9️⃣ Thread Pool Exhaustion
❓ Scenario

ExecutorService handling heavy traffic.

❓ Follow-ups
What happens when pool is full?
How to handle rejected tasks?
Which rejection policy is safest?
✅ Expected Answer
Task goes to queue
If queue full → rejection
CallerRunsPolicy prevents loss

---

# 🔟 Blocking vs Non-Blocking
❓ Scenario

API call taking long time.

❓ Follow-ups
What is blocking?
How to make it non-blocking?
Java support?
✅ Expected Answer
Blocking → thread waits
Non-blocking → async execution
CompletableFuture, Reactive APIs

---

# 1️⃣1️⃣ Synchronized Performance Issue
❓ Scenario

Application slows down after adding synchronized.

❓ Follow-ups
Why?
How to optimize?
✅ Expected Answer
Thread contention
Reduce critical section
Use concurrent collections
Use ReadWriteLock

---

# 1️⃣2️⃣ Thread Interruption Scenario
❓ Scenario

Need to stop long-running thread safely.

❓ Follow-ups
How to stop thread?
Why stop() deprecated?
Correct approach?
✅ Expected Answer
Use interrupt()
Check interrupted flag
Graceful shutdown

---

# 1️⃣3️⃣ Parallel Stream Issue
❓ Scenario

Parallel stream gives wrong result.

❓ Follow-ups
Why?
When not to use parallel streams?
✅ Expected Answer
Shared mutable state
Non-thread-safe operations
Avoid side effects

---

# 1️⃣4️⃣ Daemon Thread Scenario
❓ Scenario

Background logging thread stops abruptly.

❓ Follow-ups
Why?
What is daemon thread?
✅ Expected Answer
JVM exits when only daemon threads remain
Logging should not be daemon
🎯 MOST IMPORTANT INTERVIEW SCENARIO (🔥🔥🔥)
❓ “Why wait() releases lock but sleep() does not?”
✅ Expected Answer
wait() → coordination mechanism
sleep() → timing mechanism
wait allows other threads to acquire lock
🧠 ONE-LINE INTERVIEW CHEAT RULE

Every multithreading bug = shared mutable state without proper synchronization

---
