🔥 Exception Handling in Multithreading — INTERVIEW NOTES (IN-DEPTH)
✅ Definition
In multithreading, exceptions are isolated to the thread where they occur.
If a thread throws an uncaught exception, that thread terminates, but other threads (including main) continue unless explicitly coordinated.
📌 Simple 1-Line Explanation
A thread exception kills only that thread unless you handle it.

👉 Interview Tip:
Best senior line:

“Exception propagation is thread-bound, not process-wide.”

🧠 Why It Is Important
Prevents silent worker thread failures
Critical for:
ExecutorService
batch jobs
schedulers
async payment retries
reconciliation systems
Very important in banking systems where background threads process:
settlements
fraud checks
notifications
statement generation
🏦 Banking Domain Relevance
Fraud detection thread fails
payment API may still work
Settlement worker crashes
batch item may remain pending
Notification thread exception
transaction succeeds but SMS fails

🔥 Important:
In multithreading, silent exception loss is a major production risk.

🔹 Core Concepts
1) What Happens If Exception Occurs in a Thread?
Exception stays inside that thread’s call stack
If not caught:
thread terminates
JVM invokes UncaughtExceptionHandler
stack trace printed by default
Thread t = new Thread(() -> {
    int x = 10 / 0;
});
t.start();
Result
child thread dies
main thread continues
app may partially continue
2) Does Main Thread Catch Child Thread Exception?
❌ No

This is one of the most asked multithreading questions.

public static void main(String[] args) {
    try {
        new Thread(() -> {
            throw new RuntimeException("Child failed");
        }).start();
    } catch (Exception e) {
        System.out.println("Will never execute");
    }
}
Why?
try-catch in main covers main thread stack only
child thread has its own stack
exception never jumps threads

👉 Interview Tip:
Say:

“Parent thread cannot catch child thread exception because stacks are independent.”

This sounds strong.

🔍 Interview Follow-Up Questions
❓ What Is UncaughtExceptionHandler?
Java provides:

Thread.UncaughtExceptionHandler

It is called when a thread dies due to uncaught exception.
Example
Thread t = new Thread(() -> {
    throw new RuntimeException("Payment retry failed");
});

t.setUncaughtExceptionHandler((thread, ex) -> {
    System.out.println(thread.getName() + " failed: " + ex.getMessage());
});

t.start();
Real Use Cases
log fatal worker failure
send alert
push failed job to DLQ
restart scheduler
increment monitoring metric
🏦 Banking Example
settlement worker dies
handler logs batch ID
failed batch resumes from checkpoint

🔥 Important:
Best for:

background workers
schedulers
async batch jobs
notification services
❓ How To Handle Exception in ExecutorService?
✅ Most Important Senior Topic

With ExecutorService, exception handling depends on:

execute()
submit()
✅ Using submit() + Future

Best and most reliable.

import java.util.concurrent.*;

ExecutorService pool = Executors.newFixedThreadPool(1);

Future<?> future = pool.submit(() -> {
    throw new RuntimeException("Gateway timeout");
});

try {
    future.get();
} catch (ExecutionException e) {
    System.out.println("Handled: " + e.getCause().getMessage());
} catch (InterruptedException e) {
    Thread.currentThread().interrupt();
}
Why Best?
exception captured in Future
caller decides retry
perfect for async workflows
works well in batch jobs

Future.get() rethrows task failure as ExecutionException.

✅ Using execute()

If task throws unchecked exception:

worker thread may terminate
uncaught handler may run
no Future returned

Use when fire-and-forget logging is enough.

💻 Code Example
🏦 Banking Retry Worker Example
import java.util.concurrent.*;

public class RetryService {

    public static void main(String[] args) {
        ExecutorService pool = Executors.newFixedThreadPool(1);

        Future<?> future = pool.submit(() -> {
            throw new RuntimeException("Payment gateway failed");
        });

        try {
            future.get();
        } catch (ExecutionException e) {
            System.out.println("Move transaction to retry queue");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        pool.shutdown();
    }
}
Why This Is Correct
async-safe
failure visible
retry-friendly
production batch compatible
🌍 Real-World Examples
🏦 Banking
settlement batch thread fails
move failed item to retry queue
continue other batch chunks
🏥 Healthcare
async report generation thread crashes
patient API unaffected
failed task retried later
💳 Payments
webhook callback worker fails
retry using DLQ
alert support team
⚠️ Common Interview Traps
❌ Trap 1: Main Thread Can Catch Worker Exception

Wrong.

❌ Trap 2: Ignoring Future.get()

If you never call get(), task exceptions may remain hidden.

❌ Trap 3: Swallowing Exception in Runnable
catch (Exception e) {}

Silent async failure = huge production issue.

❌ Trap 4: Forgetting InterruptedException

Always restore interrupt:

Thread.currentThread().interrupt();
🚀 Best Practices
Use Future.get() for task failures
Prefer submit() over execute() when result/error matters
Use UncaughtExceptionHandler for raw threads
Add task IDs in logs
push failed jobs to retry queue / DLQ
never swallow worker exceptions
preserve interrupt status
use monitoring counters
shut down executors cleanly
🏦 Banking Production Insight

For payment retries:

task failure → DLQ
retry scheduler reprocesses
do not block entire pool
isolate per transaction
🎯 Interview-Ready Final Answer
If an exception occurs in a thread, only that thread is affected; it terminates if the exception is uncaught.
The main thread cannot catch child thread exceptions because each thread has its own call stack.
Java provides UncaughtExceptionHandler to centrally handle uncaught worker thread failures.
In ExecutorService, the best approach is to use submit() and handle failures through Future.get(), which wraps task exceptions in ExecutionException.
In banking systems, this is essential for retry queues, batch resilience, and preventing silent worker failures.
