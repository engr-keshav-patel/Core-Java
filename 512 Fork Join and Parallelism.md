# 🔥 JAVA MULTITHREADING — FORK/JOIN & PARALLELISM (IN-DEPTH)

---

## ✅ Q30. What is the Fork/Join Framework?

### Interview-Level Definition

* The Fork/Join framework (introduced in Java 7) is a specialized framework for parallel execution of divide-and-conquer tasks, optimized for CPU-intensive workloads.
* A big task is forked into smaller subtasks, executed in parallel, and then joined to produce the final result.
* It is built on top of `ForkJoinPool`.

---

## 🔹 Core Idea (Divide & Conquer)

1. Split the task into subtasks
2. Execute subtasks in parallel
3. Combine results

---

## 🔹 Simple Example

```java
class SumTask extends RecursiveTask<Integer> {
    private final int start, end;

    SumTask(int start, int end) {
        this.start = start;
        this.end = end;
    }

    protected Integer compute() {
        if (end - start <= 10) {
            int sum = 0;
            for (int i = start; i <= end; i++) sum += i;
            return sum;
        }

        int mid = (start + end) / 2;
        SumTask left = new SumTask(start, mid);
        SumTask right = new SumTask(mid + 1, end);

        left.fork();
        int rightResult = right.compute();
        int leftResult = left.join();

        return leftResult + rightResult;
    }
}
```

---

## 🔍 Follow-ups (Q30)

### ➤ Work-Stealing Algorithm (🔥 VERY IMPORTANT)

#### What Is Work Stealing?

* Each worker thread maintains its own deque (double-ended queue).
* Thread pushes tasks at one end
* Executes from its own queue
* If idle → steals tasks from other threads

> 👉 This keeps all CPU cores busy.

### ➤ Why Work Stealing Is Efficient?

* Reduces contention
* Improves load balancing
* Avoids centralized queue bottleneck

> 👉 **Interview one-liner:**
> Work stealing maximizes CPU utilization by redistributing unfinished tasks dynamically.

---

### ➤ RecursiveTask vs RecursiveAction

| Aspect       | RecursiveTask | RecursiveAction  |
| ------------ | ------------- | ---------------- |
| Return value | ✅ Yes         | ❌ No             |
| Use case     | Computation   | Side-effect task |
| Example      | Sum, search   | File processing  |

```java
class MyTask extends RecursiveTask<Integer> { }
class MyAction extends RecursiveAction { }
```

---

### ➤ When Should You Use Fork/Join?

* ✅ CPU-bound tasks
* ✅ Large computations
* ✅ Recursive problems
* ✅ Independent subtasks
* ❌ IO-bound tasks
* ❌ Blocking operations
* ❌ Tasks with shared mutable state

> 👉 **Interview rule:**
> Fork/Join is for computation, not coordination.

---

## ✅ Q31. Parallel Streams

### Interview-Level Definition

* Parallel streams allow a stream pipeline to be executed in parallel using the `ForkJoinPool` (common pool), without explicit thread management.

```java
list.parallelStream()
    .map(x -> x * x)
    .forEach(System.out::println);
```

---

## 🔍 Follow-ups (Q31)

### ➤ Difference Between stream() and parallelStream()

| Aspect    | stream()     | parallelStream() |
| --------- | ------------ | ---------------- |
| Execution | Sequential   | Parallel         |
| Threads   | Single       | Multiple         |
| Control   | High         | Low              |
| Overhead  | Low          | Higher           |
| Use case  | Simple tasks | CPU-intensive    |

> 👉 **Interview insight:**
> Parallel streams trade control for convenience.

---

### ➤ When NOT to Use Parallel Streams? (🔥 VERY IMPORTANT)

* ❌ IO-bound tasks
* ❌ Blocking calls (DB, REST)
* ❌ Small datasets
* ❌ Stateful lambdas
* ❌ Shared mutable data
* ❌ Need custom thread pool

```java
// BAD
list.parallelStream().forEach(x -> sharedList.add(x));
```

> 👉 This causes race conditions.

---

### ➤ Common ForkJoinPool (CRITICAL TOPIC)

* Parallel streams use `ForkJoinPool.commonPool()`
* Shared globally across JVM
* Default size = CPU cores - 1

```java
ForkJoinPool.commonPool();
```

* ⚠️ Blocking a parallel stream task blocks other unrelated parallel operations.

### ➤ Can We Change Pool Size?

* Yes (JVM-wide):

```text
-Djava.util.concurrent.ForkJoinPool.common.parallelism=8
```

* ❌ Not recommended in servers
* ❌ Affects entire JVM

---

## 🧠 Fork/Join vs ExecutorService

| Aspect    | Fork/Join  | ExecutorService |
| --------- | ---------- | --------------- |
| Task type | Recursive  | Independent     |
| Blocking  | ❌ Avoid    | ✅ Allowed       |
| Best for  | CPU-bound  | IO-bound        |
| Control   | Low-medium | High            |

---

## 🧠 COMMON INTERVIEW TRAPS

* ❌ Fork/Join is faster for everything
* ❌ Parallel streams always improve performance
* ❌ Safe to do IO in parallel streams
* ❌ Parallel streams use separate thread pool

---

## 🎯 STRONG INTERVIEW CLOSING STATEMENT

* The Fork/Join framework enables efficient parallel execution of recursive, CPU-intensive tasks using work-stealing, while parallel streams provide a convenient abstraction built on the common ForkJoinPool. Both must be used carefully to avoid blocking, contention, and unintended JVM-wide side effects.

---
