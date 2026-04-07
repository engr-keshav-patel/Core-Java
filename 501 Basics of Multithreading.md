# 🔥 JAVA MULTITHREADING

## 1️⃣ Basics of Multithreading

### ✅ Q1. What is a Thread?

>  A thread is the smallest unit of execution inside a process.
> In Java, a thread represents an independent path of execution that shares the same memory space (heap) with other threads > of the same process but has its own stack.

#### Example
```java
class MyTask extends Thread {
    public void run() {
        System.out.println("Task running in thread: " + Thread.currentThread().getName());
    }
}

public class Test {
    public static void main(String[] args) {
        new MyTask().start();
        new MyTask().start();
    }
}
```
Here:

One process → JVM
Two threads → executing run() concurrently

#### 🔍 Follow-ups

#### ➤ Difference between Process and Thread
| Aspect | Process | Thread |
|---|---|---|
| Definition | Program in execution | Lightweight execution unit |
| Memory | Separate memory | Shared memory |
| Communication | Expensive (IPC) | Cheap (shared heap) |
| Creation Cost | High | Low |
| Failure Impact | Process crash | Thread crash |


> [!TIP]
> Threads are faster because they share memory, but this also introduces concurrency bugs.

#### ➤ Why are threads called lightweight processes?
* Threads don’t need separate memory
* Creation and destruction is cheap
* Context switching is faster
* They reuse process resources

* Hence → lightweight

#### ➤ Can a Java program run without threads?
❌ No
Even the simplest Java program runs with:
* Main thread
* GC thread
* JIT/compiler threads

> [!TIP]
> 👉 Every Java program is multithreaded by default.

#### ➤ Is main() a thread?

❌ main() is not a thread
* ✅ It is a method executed by the main thread

#### ➤ Who creates the main thread?
* JVM
* When JVM starts execution, it creates the main thread
* That thread calls main()

### ✅ Q2. Why Do We Need Multithreading?

Multithreading is used to:
* Improve application responsiveness
* Utilize CPU efficiently
* Perform multiple tasks concurrently
* Handle high-traffic systems (servers, APIs)
  
#### 🔍 Follow-ups
#### ➤ Multitasking vs Multithreading
| Aspect | Multitasking | Multithreading |
|---|---|---|
| Execution Model | Multiple processes | Multiple threads |
| Level | OS level | Application level |
| Resource Cost | Heavy | Lightweight |

* Example: Chrome + Spotify	One app doing many tasks
  
#### ➤ CPU-Bound vs IO-Bound Tasks
| Aspect | CPU-Bound | IO-Bound |
|---|---|---|
| Work Type | Heavy computation | Waiting on external resources |
| Limiting Factor | CPU speed | IO latency (DB, network, file) |
| Example | Encryption, image processing | Database calls, REST APIs, file read/write |
| Best Thread Strategy | Few threads | More threads |

👉 More threads = better throughput

#### ➤ Real-World Examples
* Browser
* UI thread
* Network thread
* Rendering thread
* JavaScript execution thread
* Web Server (Spring Boot)
* One request = one thread
* Thread pool handles thousands of users
* IDE
* Code typing
* Background indexing
* Error analysis

#### ➤ When Should We Avoid Multithreading?
* Simple sequential tasks
* Heavy shared mutable state
* Low-resource systems
* When synchronization overhead > benefit

> [!NOTE]
> 👉 Golden rule:
> Don’t add multithreading unless you need concurrency.

### ✅ Q3. Advantages and Disadvantages of Multithreading

#### ✅ Advantages
* 1️⃣ Better CPU Utilization
* CPU doesn’t stay idle during IO wait
* UI remains responsive
* Background tasks run asynchronously
* 3️⃣ High Throughput
* More requests processed simultaneously
* 4️⃣ Resource Sharing
* Threads share heap → less memory usage
#### ❌ Disadvantages
* ➤ Context Switching Cost
* CPU switches between threads
* Too many threads → performance drop
  
> [!NOTE]
> Excessive threads can make application slower, not faster.

* ➤ Memory Overhead
* Each thread has its own stack
* Thousands of threads → OutOfMemoryError
* ➤ Debugging Complexity
* Bugs are non-deterministic
* Issues may not reproduce consistently
* ➤ Deadlocks & Race Conditions
* Race Condition
* count++;
* Not atomic
* Leads to inconsistent results
* Deadlock
* Threads waiting forever on each other

> [!NOTE]
>👉 Most production bugs in multithreading come from improper synchronization

> [!NOTE]
> * Multithreading improves performance and responsiveness, but it must be used carefully because shared memory introduces
> concurrency issues like race conditions, deadlocks, and visibility problems.

---
---
