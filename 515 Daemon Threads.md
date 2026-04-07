

🔥 JAVA MULTITHREADING — DAEMON THREADS (IN-DEPTH)
✅ Q35. What is a Daemon Thread?
Interview-Level Definition

A daemon thread is a background service thread that runs in support of user threads and does not prevent the JVM from shutting down.

When all user (non-daemon) threads finish execution, the JVM exits, even if daemon threads are still running.

🔹 Key Characteristics
Low-priority background tasks
No guarantee of completion
Automatically terminated on JVM shutdown
🔹 Simple Example
Thread daemon = new Thread(() -> {
    while (true) {
        System.out.println("Running in background...");
    }
});

daemon.setDaemon(true);
daemon.start();

If main() ends → JVM exits → daemon stops immediately.

🔍 Follow-ups (Q35)
➤ Difference Between User Thread and Daemon Thread
Aspect	User Thread	Daemon Thread
Purpose	Core application logic	Background services
JVM exit	Waits to finish	Ignored
Completion guarantee	✅ Yes	❌ No
Examples	main thread, request threads	GC, cleanup threads

👉 Interview one-liner:

User threads keep the JVM alive; daemon threads do not.

➤ JVM Shutdown Behavior (🔥 IMPORTANT)

JVM exits when:

All user threads finish execution
OR System.exit() is called

JVM does not wait for:

Daemon threads
finally blocks in daemon threads

👉 Critical implication:

Never perform critical work in daemon threads.

➤ Can We Convert a User Thread to Daemon Thread?

✅ Yes, but only before start()

Thread t = new Thread(task);
t.setDaemon(true);
t.start();

❌ After start() → IllegalThreadStateException

➤ Inheritance Rule
Child thread inherits daemon status from parent
🔍 Examples of Daemon Threads
➤ Garbage Collector (GC)
Runs in background
Frees unused memory
JVM doesn’t wait for GC before exiting

👉 Most cited interview example

➤ Other Examples
JIT compiler threads
Reference handler threads
Finalizer (legacy)
Cache cleanup threads
Monitoring/housekeeping threads
🧠 Real-World Scenario (Interview Favorite)

❓ “Why did my background logging thread stop unexpectedly?”

Answer:
It was a daemon thread
JVM shut down after user threads finished
Daemon thread terminated abruptly
🧠 COMMON INTERVIEW TRAPS

❌ Daemon threads are faster
❌ Daemon threads are low priority (not guaranteed)
❌ JVM waits for daemon threads
❌ Daemon threads should handle business logic

🎯 STRONG INTERVIEW CLOSING STATEMENT

Daemon threads are designed for background support tasks and do not block JVM shutdown. They must never be used for critical business operations because their execution is not guaranteed.

---
---
