# 🔥 JAVA MULTITHREADING — COMPLETABLEFUTURE (IN-DEPTH)

---

## ✅ Q32. What is CompletableFuture?

### Interview-Level Definition

* CompletableFuture (Java 8) is an asynchronous, non-blocking abstraction that represents a future result of a computation and allows you to build async pipelines using functional-style callbacks.
* It solves the limitations of Future by allowing:

  * Non-blocking callbacks
  * Chaining async tasks
  * Combining multiple async results
  * Explicit exception handling
  * Manual completion

---

## 🔹 Why Future Was Not Enough

| Future                | CompletableFuture      |
| --------------------- | ---------------------- |
| Blocking `get()`      | Non-blocking callbacks |
| No chaining           | Fluent pipeline        |
| No exception handling | Built-in handling      |
| No composition        | Combine/compose        |

> 👉 **Interview line:**
> CompletableFuture brings reactive-style programming to core Java.

---

## 🔹 Simple Example

```java
CompletableFuture<Integer> future =
    CompletableFuture.supplyAsync(() -> 10)
                     .thenApply(x -> x * 2);

future.thenAccept(System.out::println); // 20
```

---

## 🔍 Follow-ups (Q32)

### ➤ supplyAsync() vs runAsync()

#### Core Difference

| Aspect          | supplyAsync    | runAsync        |
| --------------- | -------------- | --------------- |
| Return value    | ✅ Yes          | ❌ No            |
| Functional type | `Supplier<T>`  | `Runnable`      |
| Use case        | Compute result | Fire-and-forget |

#### Example

```java
CompletableFuture<Integer> f1 =
    CompletableFuture.supplyAsync(() -> 100);

CompletableFuture<Void> f2 =
    CompletableFuture.runAsync(() -> {
        System.out.println("Task executed");
    });
```

> 👉 **Interview tip:**
> Use `runAsync()` when result is not needed.

---

### ➤ thenApply() vs thenCompose() (🔥 VERY IMPORTANT)

#### Core Difference

* `thenApply()` → Transformation
* `thenCompose()` → Flattening async calls

#### ❌ Problem with thenApply()

```java
CompletableFuture<CompletableFuture<Integer>> future =
    CompletableFuture.supplyAsync(() -> 10)
        .thenApply(x ->
            CompletableFuture.supplyAsync(() -> x * 2)
        );
```

* Result → Nested future ❌

#### ✅ Solution with thenCompose()

```java
CompletableFuture<Integer> future =
    CompletableFuture.supplyAsync(() -> 10)
        .thenCompose(x ->
            CompletableFuture.supplyAsync(() -> x * 2)
        );
```

> 👉 **Interview one-liner:**
> `thenCompose()` is used when one async task depends on the result of another async task.

#### Quick Rule

| Method        | Use When            |
| ------------- | ------------------- |
| `thenApply`   | Sync transformation |
| `thenCompose` | Async dependency    |

---

### ➤ thenCombine()

#### Purpose

* Combine results of two independent CompletableFutures.

#### Example

```java
CompletableFuture<Integer> f1 =
    CompletableFuture.supplyAsync(() -> 10);

CompletableFuture<Integer> f2 =
    CompletableFuture.supplyAsync(() -> 20);

CompletableFuture<Integer> result =
    f1.thenCombine(f2, (a, b) -> a + b);

result.thenAccept(System.out::println); // 30
```

#### Use Case

* Fetch user details
* Fetch user orders
* Combine both into response

> 👉 **Interview insight:**
> `thenCombine()` runs tasks in parallel, not sequentially.

---

### ➤ Exception Handling in CompletableFuture (🔥 CRITICAL)

#### 1️⃣ exceptionally()

* Handles exception
* Provides fallback value

```java
future.exceptionally(ex -> {
    return -1;
});
```

#### 2️⃣ handle()

* Handles both success and failure

```java
future.handle((result, ex) -> {
    if (ex != null) return 0;
    return result;
});
```

#### 3️⃣ whenComplete()

* Side-effect only
* Does NOT change result

```java
future.whenComplete((res, ex) -> {
    log.info("Completed");
});
```

#### Interview Comparison

| Method          | Can modify result | Handles exception |
| --------------- | ----------------- | ----------------- |
| `exceptionally` | ✅                 | ✅                 |
| `handle`        | ✅                 | ✅                 |
| `whenComplete`  | ❌                 | ✅                 |

---

### ➤ Async vs Non-Async Methods (🔥 OFTEN ASKED)

#### Rule

* Methods without `Async` run in same thread
* Methods with `Async` run in thread pool

#### Example

```java
thenApply()        // same thread
thenApplyAsync()   // different thread
```

#### Thread Pool Used

* Default → `ForkJoinPool.commonPool`
* Can supply custom executor

```java
Executor executor = Executors.newFixedThreadPool(5);

CompletableFuture.supplyAsync(task, executor);
```

> 👉 **Interview insight:**
> Async variants give control over threading, not logic.

---

## 🧠 Real-World Microservice Example

```java
CompletableFuture<User> user =
    CompletableFuture.supplyAsync(() -> getUser());

CompletableFuture<Order> order =
    CompletableFuture.supplyAsync(() -> getOrders());

CompletableFuture<Response> response =
    user.thenCombine(order, (u, o) -> new Response(u, o));
```

* ✔ Non-blocking
* ✔ Parallel
* ✔ Scalable

---

## 🧠 COMMON INTERVIEW TRAPS

* ❌ Thinking CompletableFuture is multithreading by default
* ❌ Blocking with `get()` everywhere
* ❌ Using parallel streams inside CompletableFuture
* ❌ Forgetting custom executor in servers
* ❌ Confusing `thenApply` with `thenCompose`

---

## 🎯 STRONG INTERVIEW CLOSING STATEMENT

* CompletableFuture enables non-blocking, asynchronous programming by allowing tasks to be composed, combined, and handled declaratively. It is ideal for modern microservices where scalability, responsiveness, and parallel IO are critical.

---
