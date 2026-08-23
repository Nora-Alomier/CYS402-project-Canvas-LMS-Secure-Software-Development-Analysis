# Canvas LMS — Secure Software Development Analysis

An end-to-end security analysis of Canvas, the open-source learning management system used by universities and schools worldwide, done across four phases for the CYS402 (Secure Software Development) course at Prince Sultan University. The project moves from identifying what's worth protecting, through modeling how the system could be attacked, to reviewing real functions in Canvas's own codebase for how well they hold up.

## Why Canvas

Canvas runs coursework for a large number of institutions: assignments, grades, quizzes, communication between students and instructors, and everything an academic administrator needs to keep the platform running. That combination of scale and sensitive data — personal information, academic records, authentication credentials — makes it a realistic target to study, and its source is public on GitHub, which meant the project could go past theory and actually inspect production code.

## What this project covers

**Phase 1 — Domain and risk identification.** What Canvas does, who its stakeholders are (students, instructors, administrators, third-party integrations), and the four headline risk categories: data breaches, unauthorized access, service outages, and data integrity attacks. Each risk is traced back to its likely root cause and the damage it could realistically do to a school's operations.

**Phase 2 — Assets, threat actors, and STRIDE.** A full asset inventory (student data, academic records, authentication credentials, API tokens, audit logs, servers, databases) prioritized by sensitivity and impact, followed by a profile of who might attack the system — external hackers, insider students misusing legitimate access, malicious administrators, and organized cybercriminal groups — and a STRIDE-based breakdown of how each threat category (spoofing, tampering, repudiation, information disclosure, denial of service, elevation of privilege) maps to a break in confidentiality, integrity, or availability.

**Phase 3 — Architecture and threat modeling.** The system broken into subsystems (UI, application logic, authentication and access control, database management, API integration, logging and monitoring), each mapped against its security-specific counterpart (encryption services, access control, audit logging). This phase also defines the system's machine and trust boundaries and traces out where external interactions introduce the most risk.

**Phase 4 — Secure code review.** Each team member picked one security-relevant function from the live Canvas repository and reviewed it manually against the OWASP Top 10 and OWASP ASVS, then ran the same repository through SonarCloud for automated static analysis. The functions reviewed:

- `authenticate_one_time_password` — the MFA/OTP verification logic, checked for race conditions and correct atomic handling
- `session_token` (login controller) — session token generation and `return_to` URL validation, flagged for token exposure in the URL rather than an HTTP-only cookie
- `otp_secret_key=` — the encryption setter for MFA secrets, checked against OWASP's cryptographic-failures category
- `clear_file_session` — session invalidation on logout, using `SecureRandom` for regeneration

## Repository structure

```
cys402-canvas-lms-secure-design/
├── README.md
├── diagrams/
│   ├── system-architecture.png        # Subsystem-level architecture (Phase 3)
│   ├── trust-boundaries-dfd.png       # Machine/trust boundary data-flow diagram
│   └── class-diagram.png              # Core entity relationships (User, Course, Submission, AuditLog...)
├── secure-code-review/
│   ├── 01-otp-authentication.png
│   ├── 02-session-token-login-controller.png
│   ├── 03-otp-secret-encryption.png
│   ├── 04-session-invalidation.png
│   └── 04b-session-invalidation-sonarcloud.png
└── docs/
    └── CYS402_Canvas_LMS_Full_Report.pdf   # All four phases, complete
```

## Source

Canvas LMS is instructure's open-source project: [github.com/instructure/canvas-lms](https://github.com/instructure/canvas-lms). This project analyzes and critiques the public codebase; it does not modify or redistribute it.

## Course context

Prepared for **CYS402: Secure Software Development**, Term 252, Section 1407, Prince Sultan University.
Instructor: **Dr. Zainab Masood**.

## Team

Joud AlNitaifi, Haifa Alangari, Nora Alomier, Haya Albakr
