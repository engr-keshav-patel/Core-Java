

# 🔥 JAVA MULTITHREADING — THREAD CREATION (IN-DEPTH)
## 2️⃣ Thread Creation
### ✅ Q4. Ways to Create a Thread in Java

> [!NOTE]
>  Java provides four standard ways to create threads, evolving from low-level control → high-level concurrency management.


#### 1️⃣ Extending Thread class

#### Example

```java

class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

public class Test {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();
    }
}
```

#### Explanation
* You override run()
* Call start() → JVM creates a new thread
* run() executes in new call stack
  
#### ❌ Limitations
* Java supports single inheritance
* You lose chance to extend another class
* Thread logic tightly coupled with thread management

> 👉 Verdict:
> ❌ Not recommended for real projects

#### 2️⃣ Implementing Runnable (MOST COMMON)

#### Example

```java
class MyTask implements Runnable {
    public void run() {
        System.out.println("Task running in: " + Thread.currentThread().getName());
    }
}

public class Test {
    public static void main(String[] args) {
        Thread t = new Thread(new MyTask());
        t.start();
    }
}
```

#### Explanation
* Runnable represents task
* Thread represents execution
* Separation of responsibility
  
#### ✅ Advantages
* Supports inheritance
* Clean design
* Better for testing
* Preferred in frameworks (Spring, Executors)

> [!NOTE]
> 👉 Runnable decouples what to do from how to execute.

#### 3️⃣ Using Callable (Return Value + Exception)

#### Example

```java

Callable<Integer> task = () -> {
    return 10 + 20;
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(task);

System.out.println(future.get()); // 30
executor.shutdown();
```

#### Explanation
* Introduced in Java 5
* Used when:
* You need result
* You need to throw checked exceptions
  
#### 4️⃣ Using ExecutorService (BEST PRACTICE)

#### Example

```java
ExecutorService executor = Executors.newFixedThreadPool(3);

executor.execute(() -> {
    System.out.println("Running in thread pool");
});

executor.shutdown();
```

#### Why This Is Best?
* Thread reuse
* Controlled concurrency
* Better performance
* Prevents thread explosion

> [!NOTE]
>  👉 Never create threads manually in production systems.

#### 🔍 Follow-ups (Q4)

#### ➤ Difference between Thread vs Runnable

| Aspect | `Thread` | `Runnable` |
|---|---|---|
| Inheritance | Extends `Thread` | Implements `Runnable` |
| Flexibility | Low | High |
| Design | Tight coupling | Loose coupling |
| Multiple Inheritance Support | ❌ No | ✅ Yes |
| Preferred | ❌ No | ✅ Yes |

#### ➤ Why is Runnable preferred?

* Supports inheritance
* Better OO design
* Thread pooling compatibility
* Cleaner separation of concerns

> [!NOTE]
> 👉 Runnable follows composition over inheritance.

#### ➤ Can we extend multiple threads?

❌ No

* Java does not support multiple inheritance of classes.

#### ➤ Which approach is best and why?

* ✅ ExecutorService + Runnable/Callable

Reason:

* Thread reuse
* Resource control
* Better scalability
* Production-grade concurrency
  
#### ✅ Q5. Difference Between Runnable and Callable

| Feature | `Runnable` | `Callable` |
|---|---|---|
| Return Value | ❌ No | ✅ Yes |
| Checked Exception | ❌ No | ✅ Yes |
| Introduced In | Java 1.0 | Java 5 |
| `submit()` Result | `null` | `Future<T>` |
| Used with Executor | ✅ Yes | ✅ Yes |


#### ➤ How to get result from Callable?

* Using Future

```java
Future<Integer> future = executor.submit(callable);
Integer result = future.get();
```

#### ➤ What is Future?

> [!NOTE] A placeholder for result of async computation.

* Capabilities:

* get() → blocks until result
* isDone() → check completion
* cancel() → cancel task

> [!NOTE]
>  👉 Future represents the promise of a future result.

#### ➤ Can Runnable throw checked exception?

❌ No
> It must handle checked exceptions internally.

### ✅ Q6. What Happens Internally When We Call start()?
> [!TIP] MOST IMPORTANT QUESTION 🔥
Step-by-Step Internal Flow

* 1️⃣ start() is called
* 2️⃣ JVM requests OS to create new thread
* 3️⃣ New call stack is allocated
* 4️⃣ Thread state → RUNNABLE
* 5️⃣ JVM calls run() internally
* 6️⃣ run() executes concurrently

#### ➤ Difference Between start() and run()
| Aspect | `start()` | `run()` |
|---|---|---|
| Thread Creation | ✅ Creates new thread | ❌ No new thread |
| Execution | Parallel / concurrent | Sequential |
| Internal Behavior | JVM internally calls `run()` | Normal method call |
| Multithreading | ✅ Yes | ❌ No |
| Example | `t.start();` | `t.run();` |

> [!NOTE]
>  👉 Calling run() directly **does NOT** start a new thread.

#### ➤ Can we call start() twice?

❌ No

```java
Thread t = new Thread();
t.start();
t.start(); // ❌

Exception:
IllegalThreadStateException
```

#### ➤ Why start() only once?
Thread lifecycle is one-time
Once terminated → cannot restart

#### ➤ Who allocates stack memory?

* ✅ JVM + OS

* JVM requests OS
* OS allocates native thread
* Each thread gets:
* Separate stack
* Program counter
* Heap is shared

> [!NOTE]
>  🎯 Thread creation evolved from low-level Thread API to high-level Executor framework to improve performance, scalability, and safety in concurrent applications.

---
---
