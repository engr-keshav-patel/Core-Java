🔥 Tricky / Output-Based Questions — Exception Handling — INTERVIEW NOTES (IN-DEPTH)

🔹 Core Concepts
1) Exception in catch Block
✅ New Exception Starts New Propagation

If a catch block throws another exception:

old exception is considered handled
new exception starts propagation
Example
public class Demo {
    public static void main(String[] args) {
        try {
            int x = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Catch block");
            throw new RuntimeException("New exception");
        }
    }
}
✅ Output
Catch block
Exception in thread "main" java.lang.RuntimeException: New exception
Why?
original ArithmeticException handled
catch throws new runtime exception
new propagation begins

👉 Interview Tip:
Say:

“Catch block can become a new exception source.”

2) Exception in finally Block
⚠️ finally Exception Overrides Previous Exception

This is a classic tricky question.

public class Demo {
    public static void main(String[] args) {
        try {
            throw new RuntimeException("Try failed");
        } finally {
            throw new IllegalStateException("Finally failed");
        }
    }
}
✅ Final Output
Exception in thread "main" java.lang.IllegalStateException: Finally failed
Why?
finally executes before method exits
new exception replaces old one
original exception gets hidden

🔥 Important:
This is why finally code must be extremely safe.

3) Nested try-catch
✅ Inner Catch Gets First Chance

JVM always resolves nearest matching catch first.

public class Demo {
    public static void main(String[] args) {
        try {
            try {
                int x = 10 / 0;
            } catch (ArithmeticException e) {
                System.out.println("Inner catch");
            }
        } catch (Exception e) {
            System.out.println("Outer catch");
        }
    }
}
✅ Output
Inner catch
Why?
inner block handles it fully
outer never receives exception
4) Exception in Constructor
✅ Object Creation Fails

If constructor throws exception:

object creation remains incomplete
reference not assigned usable object
partially initialized object discarded
public class Account {
    Account() {
        throw new RuntimeException("Constructor failed");
    }

    public static void main(String[] args) {
        Account a = new Account();
    }
}
✅ Output
Exception in thread "main" java.lang.RuntimeException: Constructor failed

👉 Interview Tip:
Best answer:

“Object is not created successfully if constructor throws.”

5) Exception During Object Creation
🔥 Very Important

This is deeper than constructor.

Flow
memory allocated
fields default initialized
constructor starts
exception occurs
object reference never safely escapes
Example
class PaymentService {
    PaymentService() {
        int x = 10 / 0;
    }

    public static void main(String[] args) {
        PaymentService p = new PaymentService();
    }
}
Result
heap memory allocated
constructor failed
object unusable
GC cleans orphan object later
🏦 Banking Example

Spring bean:

HSM key loading in constructor fails
bean creation aborts
app startup may fail

🔥 Important:
This is very relevant in Spring bean lifecycle interviews.

🔍 Interview Follow-Up Questions
❓ What If Constructor Has try-finally?
class Demo {
    Demo() {
        try {
            System.out.println("Init");
            throw new RuntimeException();
        } finally {
            System.out.println("Cleanup");
        }
    }
}
✅ Output
Init
Cleanup
Exception...
Why?
constructor exception still follows normal try-finally rules
❓ Can this Escape Before Constructor Exception?
⚠️ Dangerous Advanced Topic

Yes, if constructor publishes this before failing:

partially initialized object may leak
thread safety issue
invalid state possible

This is a senior-level concurrency + object initialization trap.

💻 Code Example
🏦 Banking Initialization Example
class PaymentGatewayClient {

    PaymentGatewayClient() {
        try {
            System.out.println("Load certificates");
            throw new RuntimeException("Certificate invalid");
        } finally {
            System.out.println("Close temp file");
        }
    }
}
Why This Is Good Interview Example
constructor failure
finally cleanup
startup abort scenario
real banking SSL init use case
🌍 Real-World Examples
🏦 Banking
HSM key load failure in constructor
nested transfer rollback try-catch
finally lock release failure
🏥 Healthcare
patient report generator bean init fails
nested EMR parsing try-catch
💳 Payments
SDK certificate init exception
finally distributed lock cleanup issue
⚠️ Common Interview Traps
❌ Trap 1: Thinking Outer Catch Always Runs

False in nested blocks.

❌ Trap 2: Assuming Object Exists After Constructor Failure

Wrong.

❌ Trap 3: Ignoring Finally Exception Override

Very common miss.

❌ Trap 4: Publishing this From Constructor

Advanced design bug.

🚀 Best Practices
keep finally exception-safe
avoid complex logic in constructors
use factory methods for risky initialization
avoid nested try complexity unless needed
never publish partially initialized object
prefer dependency injection lifecycle hooks
use lazy init for expensive external setup
🏦 Banking Production Insight

For payment clients:

use startup health checks
avoid constructor-heavy network calls
use retryable bean initialization
🎯 Interview-Ready Final Answer
If an exception occurs inside catch, it starts a new propagation flow.
If finally throws, it overrides previous exceptions and becomes the final visible failure.
In nested try-catch, the nearest matching catch handles the exception first.
If a constructor throws, object creation fails and the reference never receives a usable object.
During object creation, memory may be allocated, but failed construction means the object is eventually garbage collected.
