🔥 Exception Handling with Method Overriding — INTERVIEW NOTES (IN-DEPTH)
✅ Definition
In Java, when a child class overrides a parent method, exception rules are controlled to preserve polymorphism safety and API compatibility.
The child method cannot widen checked exceptions, because parent reference users should not be forced to handle new broader failures.
📌 Simple 1-Line Explanation
In overriding, checked exceptions can stay same or become narrower, but cannot become broader.
👉 Interview Tip:
Best interview-ready one-liner:
“Java protects polymorphism by restricting checked exceptions during method overriding.”
🧠 Why It Is Important
Very common Java OOP + exception handling interview question
Tests:
inheritance
polymorphism
API contract safety
Liskov Substitution Principle (LSP)
Very important in:
Spring service inheritance
template pattern
abstract classes
framework extension points
🏦 Banking Domain Relevance
Base payment processor:
processPayment() throws IOException
Child:
card payment
UPI payment
NEFT payment
Each child must respect exception contract so caller code remains safe.
🔥 Important:
This topic is really about safe polymorphism contracts.
🔹 Core Concepts
1) Can Overridden Method Throw Exception?
✅ Yes
But Java applies strict rules.
Rules Summary
✅ Checked Exceptions
same exception → allowed
child/narrower exception → allowed
no exception → allowed
broader checked exception → ❌ not allowed
✅ Unchecked Exceptions
any unchecked exception → allowed
👉 Golden Rule
Checked → Same or narrower
Unchecked → Any
🔥 Important:
This is one of the most repeated Java interview rules.
🔍 Interview Follow-Up Questions
❓ Can Overridden Method Throw Broader Exception?
❌ No (for checked exceptions)
❌ Invalid Example
Java
import java.io.*;

class Parent {
    void read() throws IOException {}
}

class Child extends Parent {
    void read() throws Exception {}
}
Why Compile Error?
Parent contract promises:
Java
IOException
Child cannot suddenly force callers to handle:
Java
Exception
This breaks:
substitutability
API expectations
polymorphism
👉 This is pure LSP protection.
❓ Can It Throw Unchecked Exception?
✅ Yes — Always Allowed
Because compiler does not enforce unchecked exceptions.
Java
class Parent {
    void process() {}
}

class Child extends Parent {
    void process() throws RuntimeException {}
}
Why Allowed?
unchecked exceptions are runtime failures
caller contract unchanged
compiler safety unaffected
Banking Example
parent:
generic payment process
child:
DuplicatePaymentException extends RuntimeException
Perfectly valid.
❓ Can Child Class Avoid Exception Completely?
✅ Yes
Very common and valid.
Java
import java.io.*;

class Parent {
    void read() throws IOException {}
}

class Child extends Parent {
    void read() {
        System.out.println("No exception");
    }
}
Why Allowed?
Child provides safer implementation
fewer failure possibilities
easier for callers
contract becomes stronger
👉 Strong senior-level point:
“Child class can always reduce exception burden.”
💻 Code Example
✅ Same Checked Exception
Java
import java.io.IOException;

class PaymentProcessor {
    void process() throws IOException {
        System.out.println("Base processor");
    }
}

class CardPaymentProcessor extends PaymentProcessor {
    @Override
    void process() throws IOException {
        System.out.println("Card payment");
    }
}
✅ Narrower Checked Exception
Java
import java.io.*;

class PaymentProcessor {
    void process() throws IOException {}
}

class UpiPaymentProcessor extends PaymentProcessor {
    @Override
    void process() throws FileNotFoundException {}
}
Why Correct
FileNotFoundException is child of IOException
✅ Unchecked Exception Allowed
Java
class PaymentProcessor {
    void process() {}
}

class FraudPaymentProcessor extends PaymentProcessor {
    @Override
    void process() throws IllegalStateException {
        throw new IllegalStateException("Fraud detected");
    }
}
🌍 Real-World Examples
🏦 Banking Example
Base:
Plain text
PaymentProcessor
Children:
CardPaymentProcessor
UpiPaymentProcessor
FraudPaymentProcessor
Use Cases
checked:
gateway file failure
unchecked:
fraud detected
duplicate request
invalid payment state
🏥 Healthcare Example
Base:
Plain text
PatientImporter
Child:
Plain text
CsvPatientImporter
Can throw narrower:
Java
FileNotFoundException
instead of broader:
Java
IOException
⚠️ Common Interview Traps
❌ Trap 1: Broadening Checked Exception
Most common mistake.
❌ Trap 2: Thinking Runtime Exceptions Follow Same Rule
No. Unchecked = any allowed.
❌ Trap 3: Forgetting No Exception Is Also Valid
Very common miss.
❌ Trap 4: Ignoring Polymorphism Reason
Always explain why Java restricts it.
Best reason:
LSP
safe substitution
caller contract stability
🚀 Best Practices
Keep child exceptions:
same
narrower
unchecked
Prefer unchecked custom exceptions in Spring services
Never broaden checked exceptions
Document business unchecked exceptions
Use meaningful domain names
Preserve parent API contract
Keep inheritance extension safe
Production Spring Example
Base:
Java
PaymentService
Child:
Java
UpiPaymentService
Throw:
Java
DuplicateTransactionException
unchecked → valid and clean
🎯 Interview-Ready Final Answer
Yes, an overridden method can throw exceptions, but checked exceptions must remain the same, narrower, or be removed completely.
A child method cannot throw a broader checked exception because it would break the parent method’s contract and violate polymorphism safety.
Unchecked exceptions are always allowed because they are not compiler-enforced.
Child classes may also avoid throwing exceptions entirely, which strengthens the contract.
The golden rule is: Checked → same or narrower, Unchecked → any.
👉 Interview Tip (2+ Years Experience):
Always connect this topic with:
LSP
polymorphism
framework extension points
Spring service inheritance
template method pattern
API backward compatibility
This makes your answer sound senior OOP-focused and architecture-aware.
