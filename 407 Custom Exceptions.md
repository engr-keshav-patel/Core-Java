🔥 Custom / User-Defined Exceptions — INTERVIEW NOTES (IN-DEPTH)
✅ Definition
A custom (user-defined) exception is an exception class created by developers to represent a specific business rule violation or domain failure.
It gives meaningful, domain-specific error handling instead of generic Java exceptions.
📌 Simple 1-Line Explanation
Custom exceptions represent business-specific problems in a clean and readable way.
👉 Interview Tip:
Best senior-level line:
“Custom exceptions improve domain clarity, API readability, and recovery strategy.”
🧠 Why It Is Important
Makes code self-explanatory
Improves business readability
Avoids generic Exception
Helps global exception handling
Better API error responses
Easy mapping to HTTP status codes
Strongly used in banking, healthcare, and Spring Boot
🏦 Banking Domain Relevance
Instead of:
Java
throw new RuntimeException("Balance issue");
Use:
Java
throw new InsufficientBalanceException("Insufficient funds");
This clearly tells:
business meaning
retry possibility
customer response
audit severity
🏥 Healthcare Domain Relevance
PatientAlreadyExistsException
AppointmentSlotUnavailableException
InvalidPrescriptionException
DoctorNotAvailableException
🔥 Important:
Custom exceptions are a key part of Domain-Driven Design (DDD).
🔹 Core Concepts
1) What Is a Custom Exception?
Developer-created class
Extends:
Exception → checked
OR RuntimeException → unchecked
Used for:
domain validation
business rules
illegal state
workflow violations
Examples
banking
InsufficientBalanceException
DuplicateTransactionException
healthcare
PatientAlreadyExistsException
InvalidMedicalRecordException
2) Why Do We Need Custom Exceptions?
domain-specific meaning
better readability
cleaner logs
easier recovery strategy
API consistency
better UI/user messages
centralized exception mapping
❌ Generic Bad Example
Java
throw new RuntimeException("Patient issue");
✅ Better
Java
throw new PatientAlreadyExistsException("Patient already registered");
👉 This instantly improves debugging.
3) How To Create A Custom Exception?
✅ Runtime (Most Common)
Java
public class PatientAlreadyExistsException extends RuntimeException {

    public PatientAlreadyExistsException(String message) {
        super(message);
    }
}
✅ Checked Version
Java
public class StatementGenerationException extends Exception {

    public StatementGenerationException(String message) {
        super(message);
    }
}
🔍 Interview Follow-Up Questions
❓ Should Custom Exception Extend Exception or RuntimeException?
✅ Best Practical Rule
Extend RuntimeException when:
validation failure
business rule violation
illegal state
Spring Boot service exceptions
global exception handling
transaction rollback needed
Extend Exception when:
caller can realistically recover
file export
external integration issue
batch retry workflow
public library API
👉 Senior-Level Answer:
“In Spring enterprise applications, custom exceptions usually extend RuntimeException for cleaner APIs and natural transaction rollback.”
❓ Best Practices For Naming Custom Exceptions
✅ Naming Rules
Always end with Exception
Good Names
PatientAlreadyExistsException
InsufficientBalanceException
DuplicatePaymentException
AppointmentConflictException
❌ Bad Names
PatientError
BalanceIssue
TransferFailure
Naming Formula
Plain text
<BusinessMeaning> + Exception
👉 This improves readability instantly.
💻 Code Example
🏥 Healthcare Scenario
Patient already registered → throw PatientAlreadyExistsException
Java
class PatientAlreadyExistsException extends RuntimeException {
    public PatientAlreadyExistsException(String message) {
        super(message);
    }
}

class PatientService {

    public void registerPatient(String email) {
        if ("john@hospital.com".equals(email)) {
            throw new PatientAlreadyExistsException(
                "Patient already registered with email: " + email
            );
        }

        System.out.println("Patient registered successfully");
    }
}
Why This Is Correct
domain-specific
easy to debug
easy HTTP mapping
clear healthcare business meaning
reusable across service layer
🌍 Real-World Examples
🏦 Banking
InsufficientBalanceException
transfer amount > balance
DuplicateTransactionException
same UPI request replay
AccountFrozenException
debit blocked
🏥 Healthcare
PatientAlreadyExistsException
duplicate registration
AppointmentSlotUnavailableException
doctor busy
MedicalRecordNotFoundException
invalid patient ID
💳 Payment Systems
PaymentAlreadyProcessedException
InvalidCardStateException
FraudCheckFailedException
⚠️ Common Interview Traps
❌ Trap 1: Using Generic RuntimeException
Loses domain meaning.
❌ Trap 2: Extending Throwable
Never do this.
Always extend:
Exception
OR RuntimeException
❌ Trap 3: Wrong Naming
Java
class ErrorInPatient extends RuntimeException
Poor readability.
❌ Trap 4: No Constructor With Cause
Bad for debugging.
Better:
Java
public MyException(String msg, Throwable cause) {
    super(msg, cause);
}
🚀 Best Practices
Prefer unchecked custom exceptions in Spring
Name by business meaning
Keep message user-friendly
Preserve root cause
Use with global handler
Map to:
HTTP 400
HTTP 404
HTTP 409
Add error code for APIs
Keep reusable across layers
Spring Boot Production Pattern
Java
throw new PatientAlreadyExistsException("...");
Mapped in @ControllerAdvice:
Java
409 CONFLICT
Perfect enterprise design.
🎯 Scenario-Based Interview Answer
❓ Scenario
Patient already registered → what should you do?
✅ Best Interview Answer
Create:
Java
PatientAlreadyExistsException
Extend RuntimeException
Throw from service layer
Catch globally using Spring @ControllerAdvice
Return:
Java
409 CONFLICT
Why Best?
domain clarity
clean API
duplicate registration prevention
production-safe healthcare workflow
🎯 Interview-Ready Final Answer
A custom exception is a user-defined exception class created to represent domain-specific business failures clearly.
It improves readability, debugging, API consistency, and global error handling compared to generic exceptions.
In Spring Boot applications, custom exceptions usually extend RuntimeException for clean service APIs and automatic transaction rollback.
Good naming should always reflect business meaning, like PatientAlreadyExistsException.
In healthcare or banking systems, custom exceptions make workflows safe, expressive, and production-ready.
👉 Interview Tip (2+ Years Experience):
Always connect this topic with:
DDD
Spring @ControllerAdvice
HTTP status mapping
transaction rollback
business invariant protection
clean service APIs
This makes your answer sound senior, domain-driven, and production-focused.
