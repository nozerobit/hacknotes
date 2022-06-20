# Application Hardening

## Application Configuration Hardening

Application configuration options can be used to restrict an application's permissions or disable specific susceptible features.

Hardening an application's setup entails looking for potential vulnerabilities in both the application and the environment in which it runs.

## Dead Code Elimination

Code that is considered inaccessible by regular program execution is known as dead code. By adding code to a condition that never evaluates to true, dead code can be formed. Dead code should be eliminated because if forced to execute mistakenly or deliberately, it can create unexpected outcomes.

Algorithms that analyze program flows for unreachable code are commonly used to identify dead code. Dead code is removed by asking compilers to remove it using compiler flags, such as '-fdce' for Dead Code Elimination.

## Exception Handler Pointer Validation

When a process encounters an exception, it invokes an exception handler to handle it. The operating system uses a different approach to determine this exception handler. Even if it is the default exception handler, the program is terminated and a notification saying the program has stopped working is displayed. If no proper exception handler is identified, the program will fail to continue normally and may be designed to terminate.

The address of the exception registration record is kept at the very beginning of the Thread Information Block in Windows, and the GS register points to it.

