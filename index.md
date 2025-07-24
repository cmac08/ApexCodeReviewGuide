# 1. Formatting & Readability

- **Indentation and spacing are consistent.**
    
    Consistent formatting makes the code easier to read, understand, and maintain over time. It also reduces merge conflicts because everyone follows the same layout.
    
- **Line length is reasonable**
    
    Limiting line length helps prevent horizontal scrolling and makes code easier to read in code editors and diff tools.
    
    Writing an entire app or method on a single line is fine for computers but bad for humans. ***Don’t do it.*** 
    
- **Blank lines are used to separate logical sections.**
    
    Grouping related blocks of code with blank lines improves clarity and helps the reviewer quickly see where one part of the logic ends and another begins.
    
- **Comments explain why the code exists, not just what it does.**
    
    The code itself often shows *what* is happening. Comments should describe *why* a particular approach was used, especially if it is not apparent. 
    
    No code is universally readable to everyone. Even our own code can become difficult to understand as our domain knowledge fades over time. While developers can implement many techniques to improve code readability, one essential practice is adding clear, quality comments. 
    
- **Consistent bracket placement.**
    
    Following a standard bracket style reduces cognitive load and makes it easier to scan code for control structures and method boundaries. Brackets should start on the same line.
    
- **No commented-out or debugs left behind.**
    
    Leaving dead code behind creates confusion and increases the chance of reintroducing outdated or broken logic.
    
    **Debug Statements increase your CPU limits and effect performance**
    

---

# 2. Naming Conventions

- **Classes use PascalCase (e.g., `OpportunityService`).**
    
    This makes it clear what a class is versus a variable or method.
    
- **Methods use camelCase and start with a verb (e.g., `calculateDiscount`).**
    
    Verbs indicate an action, making code more expressive and readable.
    
- **Variables have meaningful, descriptive names. *Most of the time.***
    
    Clear names make the purpose of each variable immediately obvious. Avoid vague names like `temp`, `x`, or `data`.
    
    However, there are some legitimate uses for single-letter variables. For example, in for loops and nested for loops, using index variables like `i`, `j`, `k`, and `l` for counters is a common and acceptable practice. In fact, more verbose variables could be confusing. 
    
    ```java
    var names = []
    for (var i = 0; i < peope.length; i++) {
            names.push(people[i]).name
    }
    ```
    
    Common single-letter variable conventions include `x`, `y`, and `z` for coordinates, `n` and `m` for generic numeric values, and `e` for errors in `catch()` blocks. Similarly, `k` and `v` often represent 'key' and 'value' respectively. These conventions span multiple programming languages and become familiar through regular exposure to code.
    
    - Avoid using verbs for variables that aren't functions
    - Avoid using nouns for functions
    - Use plural names for collections and singular names for individual items
    - Avoid generic names like `object`, `array`, or `number`
    - Be careful with similar singular and plural forms that differ by only one letter, as they can cause readability issues and bugs:
    
    ```jsx
    var totalPages = 0
    for (var book of books) {
      totalPages += books.pages
    }
    ```
    
- **Constants use ALL_CAPS_WITH_UNDERSCORES.**
    
    Also known as **Screaming Snake Case**, this convention *visually* indicates that the value is fixed and should not be altered.
    
- **Abbreviations are used only when they are clear and commonly understood.**
    
    Obscure abbreviations can make the code harder to understand for others.
    

*Why this matters:*

Consistent naming reduces confusion, improves readability, and helps maintain a shared vocabulary among team members.

---

# 3. Use of Utility and Helper Methods

- **Reusable logic is extracted into service, helper, or utility classes.**
    
    This prevents duplication and keeps code DRY (Don’t Repeat Yourself). For example, if you have logic to calculate a discount in multiple places, extract it to a shared method.
    
- **Methods are small and focused.**
    
    Smaller methods are easier to test, read, and maintain. Each method should ideally do just one thing.
    
- **Single Responsibility Principle is followed.**
    
    Each method or class should have one clear purpose. This makes it easier to modify without unintended side effects.
    

*Why this matters:*

Encapsulating logic in helpers makes your code modular, promotes reuse, and reduces the likelihood of errors during maintenance.

---

# 4. Bulkification

- **Code is designed to handle collections of records, not just single records.**
    
    Salesforce processes data in batches, and your code must be able to handle scenarios where multiple records are inserted or updated simultaneously.
    
- **Utilize sets and maps to efficiently process records.**
    
    Maps and sets enable constant-time lookups, thereby avoiding performance bottlenecks when processing large datasets.
    
- **No SOQL or DML statements inside loops.**
    
    Queries and DML statements in loops can quickly exceed governor limits, resulting in your code failing in production.
    

*Why this matters:*

Bulkification is critical for scalability. It ensures that your logic works for both single-record transactions and large data operations without exceeding Salesforce governor limits.

---

# 5. DML and SOQL Best Practices

- **Minimize the number of SOQL queries (Always <=100 per transaction).**
    
    Exceeding this limit causes runtime errors. Consolidate queries whenever possible.
    
- **Minimize the number of DML statements**
    
    Group DML operations to reduce the total number of statements.
    
- **Queries are selective and optimized.**
    
    Use `WHERE` clauses to avoid pulling unnecessary records and to improve performance.
    
- **Governor limits are respected throughout the code.**
    
    Apex has strict limits designed to maintain platform performance. Always consider how close your code is to hitting these limits.
    

*Why this matters:*

Code that ignores these best practices may work in testing but fail in production when processing real data volumes.

---

# 6. Trigger Frameworks and Patterns

- **Only one trigger exists per object.**
    
    Having multiple triggers makes it hard to predict execution order, leading to inconsistent results.
    
- **Business logic is delegated to helper classes.**
    
    Triggers should only coordinate logic and never contain business rules directly. This separation improves testability and clarity.
    
- **A framework or pattern manages before/after logic and recursion.**
    
    Structured frameworks (like the Trigger Handler Pattern) ensure that different trigger contexts are handled cleanly.
    
- **Recursion prevention is implemented.**
    
    Without recursion guards, triggers can re-fire and cause infinite loops or duplicate processing.
    

*Why this matters:*

A consistent trigger pattern improves maintainability, makes logic easier to extend, and avoids unintended side effects.

---

# 7. Testing and Code Coverage

- **Tests cover positive, negative, and bulk scenarios.**
    
    This ensures your code behaves correctly in all situations.
    
- **Assertions validate expected behavior, not just code coverage.**
    
    Tests without meaningful assertions don’t prove correctness.
    
- **Reusable test data is created in `@testSetup` methods.**
    
    This keeps test code DRY and easier to maintain.
    
- **Tests are independent and don’t rely on execution order.**
    
    Each test should be able to run in isolation without relying on side effects from other tests.
    

*Why this matters:*

High-quality tests ensure your code is reliable, catches regressions early, and meets deployment requirements.

---

# 8. Security and Sharing

- **CRUD and Field-Level Security checks are enforced in code accessing data.**
    
    For example, `Schema.sObjectType.Account.fields.Name.isAccessible()` should be used to confirm a user has access to fields before reading them.
    
- **Use `with sharing` where appropriate to enforce record visibility.**
    
    This prevents users from accessing records they shouldn’t see.
    
- **Avoid hardcoded IDs, profiles, or record types.**
    
    Hardcoded values break when moving between orgs and make maintenance harder.
    

*Why this matters:*

Security mistakes can expose sensitive data or cause compliance issues. Following these practices keeps your application secure.

---

# 9. Performance Considerations

- **Limit creating new objects inside loops where possible.**
    
    Creating objects repeatedly in loops increases CPU time and heap usage.
    
- **Leverage maps and sets to avoid nested loops and expensive lookups.**
    
    Maps are essential for efficiently relating data.
    
- **Batch processing is used for large volumes of data.**
    
    Use Batch Apex or Queueable Apex if processing large datasets.
    
- **Remove redundant queries and DML statements.**
    
    Consolidate logic wherever possible to improve performance.
    

*Why this matters:*

Performance bottlenecks can lead to slow processing, limit errors, and a poor user experience.

---

# 10. Maintainability and Extensibility

- **Code is self-documenting with clear, expressive names.**
    
    Good names reduce the need for excessive comments.
    
- **Classes are modular and easy to test.**
    
    Small, focused classes make it easier to extend functionality without unintended consequences.
    
- **Clear separation of concerns between layers.**
    
    Triggers, services, helpers, and utilities each serve distinct roles.
    
- **Code is structured so it can evolve over time without significant rewrites.**

*Why this matters:*

Maintainable code reduces technical debt, enables easier onboarding, and helps future developers understand and extend your work.

---

# 11. Additional Considerations

### Error Handling

- Use `try-catch` blocks when appropriate to handle exceptions gracefully.
- Avoid catching exceptions without logging or rethrowing.
- Provide clear error messages that help identify the cause of the issue.

### Recursion Handling

- Use static variables to prevent triggers from running multiple times on the same records.
- Ensure logic is idempotent if it must run more than once.

### Version Control Practices

- Write clear commit messages that explain the purpose of changes.
- Review changes to avoid introducing merge conflicts.
- No rubber stamping pull-requests.

### Consistent Documentation

- Include class and method headers as required
- Use inline comments sparingly to clarify complex logic.

### API Versioning

- Ensure Apex classes and triggers are set to the appropriate API version.

---

## Quick Reference Checklist

Use this checklist during your review:

- [ ]  Formatting and readability
- [ ]  Naming conventions
- [ ]  No DML or SOQL in loops
- [ ]  Proper bulkification
- [ ]  Trigger uses the handler pattern
- [ ]  Reuse of helpers and utilities
- [ ]  Positive, negative, and bulk test coverage
- [ ]  CRUD and field-level security enforced
- [ ]  Recursion prevention
- [ ]  Performance considerations
- [ ]  Clear error handling
- [ ]  Maintainability and clear separation of concerns
