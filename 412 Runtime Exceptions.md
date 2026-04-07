🔥 RuntimeException Deep Dive — INTERVIEW NOTES (IN-DEPTH)
✅ Definition
RuntimeException is the superclass of all unchecked exceptions in Java. It extends Exception, but unlike checked exceptions, the compiler does not force handling or declaration.
These exceptions usually indicate:
programming bugs
invalid assumptions
bad API usage
missing validations
illegal state
📌 Simple 1-Line Explanation
Runtime exceptions happen due to coding mistakes or invalid runtime conditions, and Java does not force try-catch for them.

👉 Interview Tip:
Best one-liner:

“RuntimeException represents unchecked failures that usually indicate programming defects or invalid input.”

🧠 Why It Is Important
Very common Java + Spring Boot interview topic
Core to:
fail-fast design
validation strategy
clean service APIs
transaction rollback
global exception handling
Widely used in:
Spring services
REST APIs
domain validation
DDD exceptions
🏦 Banking Domain Relevance
negative transfer amount
invalid account state
duplicate payment request
null beneficiary details
double debit protection failure

These are usually modeled as runtime exceptions.

🔥 Important:
Spring transaction rollback naturally works best with unchecked exceptions.

🔹 Core Concepts
1) What Is RuntimeException?
Direct child of Exception
Parent of all unchecked exceptions
Compiler does not force
try-catch
throws
Usually caused by:
coding bugs
missing validation
illegal arguments
invalid object state
Hierarchy
Throwable
 └── Exception
      └── RuntimeException
2) Common Runtime Exceptions

Most asked interview examples:

Exception	Cause
NullPointerException	null object access
ArithmeticException	divide by zero
ArrayIndexOutOfBoundsException	invalid array index
IllegalArgumentException	invalid method input
IllegalStateException	invalid object state
ClassCastException	wrong type casting

Oracle directly lists many of these as subclasses of RuntimeException.

3) Why Not Force Handling Of Runtime Exceptions?

Java intentionally does not force handling because these usually indicate:

developer mistakes
logic bugs
invalid assumptions
broken contracts

Oracle explicitly classifies them as unchecked exceptions.

Why This Design Is Good

If compiler forced:

NullPointerException
ArrayIndexOutOfBoundsException

everywhere, code would become:

noisy
unreadable
full of unnecessary try-catch

👉 Interview Tip:
Best answer:

“Runtime exceptions are generally non-recoverable coding issues, so Java avoids forcing boilerplate handling.”

🔍 Interview Follow-Up Questions
❓ Is ArithmeticException Checked?
❌ No — Unchecked

It is a subclass of RuntimeException, so it is unchecked. Oracle documents it under runtime exception subclasses.

Example
int result = 10 / 0;
Banking Example
EMI calculation divisor becomes zero
invalid interest formula config
repayment tenure bug
❓ Is ArrayIndexOutOfBoundsException Checked?
❌ No — Also Unchecked

This also extends RuntimeException, so compiler does not force handling.

Example
int[] tx = {100, 200};
System.out.println(tx[5]);
Banking Example
invalid transaction batch index
wrong reconciliation file parsing loop
bad pagination logic
❓ Can We Create Custom Runtime Exception?
✅ Yes (Very Common)

This is a best practice in Spring and DDD.

public class InsufficientBalanceException extends RuntimeException {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

Oracle explicitly supports creating programmer-defined exception classes.

🔥 Important:
Custom runtime exceptions are preferred for:

domain validation
business invariant violations
API contract failures
💻 Code Example
🏦 Banking Example — Custom Runtime Exception
class InsufficientBalanceException extends RuntimeException {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

public class TransferService {

    public void transfer(double amount, double balance) {
        if (amount > balance) {
            throw new InsufficientBalanceException("Insufficient balance");
        }

        System.out.println("Transfer successful");
    }
}
Why This Is Correct
fail-fast
domain-specific
clean API
Spring rollback friendly
no checked exception clutter
🌍 Real-World Examples
🏦 Banking
InsufficientBalanceException
DuplicateTransactionException
AccountFrozenException
🏥 Healthcare
PatientAlreadyExistsException
InvalidPrescriptionStateException
AppointmentConflictException
💳 Payments
DuplicatePaymentException
FraudRuleViolationException
InvalidCardStateException
⚠️ Common Interview Traps
❌ Trap 1: “Runtime means only JVM issue”

Wrong.

Most runtime exceptions are application bugs or validation failures.

❌ Trap 2: Catching RuntimeException Everywhere

Bad design:

catch (RuntimeException e)

Only catch when:

boundary layer
scheduler
global handler
async thread boundary
❌ Trap 3: Using Checked Exception For Validation

Bad:

throws Exception

Use runtime exception for:

invalid amount
null input
illegal state
🚀 Best Practices
Use runtime exceptions for:
validation
illegal state
business invariants
Prefer custom runtime exceptions in Spring
Map using @ControllerAdvice
Preserve root cause when wrapping
Fail fast
Avoid generic RuntimeException
Prefer domain names:
InsufficientBalanceException
DuplicatePaymentException
🏦 Banking Production Insight

Runtime exceptions are ideal for:

transfer validation
duplicate request detection
fraud workflow invariant protection
account freeze checks
🎯 Interview-Ready Final Answer
RuntimeException is the parent class of all unchecked exceptions in Java, and the compiler does not force developers to catch or declare it.
It is mainly used for programming mistakes, invalid input, illegal state, and business invariant violations.
Common examples include NullPointerException, ArithmeticException, and ArrayIndexOutOfBoundsException, all of which are unchecked.
Java does not force handling because these usually indicate bugs that should be fixed, not boilerplate-caught.
In Spring Boot and banking systems, custom runtime exceptions are preferred for validation and transaction-safe fail-fast design.
