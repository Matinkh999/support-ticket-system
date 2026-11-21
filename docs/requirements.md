# Software Requirements Specification
## Support Request Tracking System

### 1. Introduction

The Support Request Tracking System is designed to centralize customer support requests and improve the process of handling, assigning, responding to, and tracking them.

This document describes the functional and non-functional requirements of the system, their priorities, and acceptance criteria.

### 2. User Roles

The system has three main user roles:

- **Customer:** Registers, submits support requests, and tracks responses.
- **Agent:** Handles assigned requests and provides responses.
- **Manager:** Manages agents, assigns requests, monitors responses, and can directly handle support requests.

### 3. Functional Requirements

| ID | Requirement | Priority |
|---|---|---|
| FR-01 | Customers shall be able to register using an email address and password. | Must |
| FR-02 | The system shall send an email verification link after registration. | Must |
| FR-03 | Customers shall not be able to access the system until their email is verified. | Must |
| FR-04 | Verified users shall be able to log in using their email and password. | Must |
| FR-05 | Users shall be able to reset forgotten passwords through a secure email link. | Must |
| FR-06 | Customers shall be able to create support requests with a title, description, category, and urgency level. | Must |
| FR-07 | Customers shall be able to attach relevant files and images to their requests. | Could |
| FR-08 | Customers shall be able to view their submitted requests and their statuses. | Must |
| FR-09 | Customers shall be able to view responses and add additional information to open requests. | Must |
| FR-10 | Managers shall be able to create accounts for support agents. | Must |
| FR-11 | Managers shall be able to promote existing customers to support agents. | Must |
| FR-12 | Customers shall not be able to change their own access roles. | Must |
| FR-13 | Managers shall be able to assign requests to support agents. | Must |
| FR-14 | Each request shall have only one active assigned person at a time. | Must |
| FR-15 | The system shall record changes in request assignments. | Must |
| FR-16 | Agents shall be able to view their assigned open requests in priority order. | Must |
| FR-17 | Agents shall be able to add response notes to requests. | Must |
| FR-18 | Agents and managers shall be able to update request statuses according to the defined workflow. | Must |
| FR-19 | Managers shall be able to view and monitor all support requests. | Must |
| FR-20 | Managers shall be able to assign requests to themselves and respond directly. | Must |
| FR-21 | The system shall generate reports of open, closed, and answered requests within a selected time period. | Must |
| FR-22 | The system shall provide a way to resend email verification links. | Should |

### 4. Business Rules

- Each request must have only one active responsible person.
- Changes in request assignments must be recorded.
- Closed requests cannot be edited. They can only be reopened through a reopening event.
- High-urgency requests must appear at the top of an agent's request queue.
- If all requests in an agent's queue are high urgency, the queue must display only high-urgency requests.
- A request cannot transition from "In Progress" to "Answered" without at least one response note.
- Closed requests must be displayed separately from the other request lists.

### 5. Request Status Workflow

The system shall support the following statuses:

1. **Open:** The request has been submitted.
2. **In Progress:** The request is being handled.
3. **Answered:** A response has been provided.
4. **Closed:** The request has been completed and closed.

Invalid status transitions must be prevented by the system.

### 6. Non-Functional Requirements

| ID | Requirement | Priority |
|---|---|---|
| NFR-01 | User passwords must be stored securely using password hashing. | Must |
| NFR-02 | Access to system features must be controlled according to user roles. | Must |
| NFR-03 | The system must temporarily block login attempts after five consecutive failed attempts for 15 minutes. | Must |
| NFR-04 | Password reset links must be secure, time-limited, and usable only once. | Must |
| NFR-05 | The user interface must be in Persian and support right-to-left (RTL) layout. | Must |
| NFR-06 | The system must support responsive layouts for computers, laptops, tablets, and mobile phones. | Must |
| NFR-07 | The system must provide a simple and understandable user interface. | Should |
| NFR-08 | The system must preserve request and response information reliably. | Must |
| NFR-09 | The application code must be organized and maintainable for future development. | Should |
| NFR-10 | The system should provide acceptable response times for normal operations. | Should |

### 7. Acceptance Criteria

The following criteria define how important requirements will be verified.

**AC-01: User Registration**
- A user can register with a valid email address and password.
- The system sends an email verification link.
- Duplicate email registrations are rejected.

**AC-02: Email Verification**
- A valid verification link activates the account.
- An unverified user cannot access protected system features.
- An expired or invalid verification link is rejected.

**AC-03: Login Security**
- Users can log in with correct credentials.
- Incorrect credentials are rejected.
- After five consecutive failed attempts, login is blocked for 15 minutes.

**AC-04: Password Recovery**
- A user can request a password reset link.
- A valid link allows the user to set a new password.
- An expired or previously used link is rejected.

**AC-05: Request Creation**
- A customer can submit a request with all required information.
- Requests with missing required fields are rejected.
- The newly created request receives the initial Open status.

**AC-06: Request Assignment**
- A manager can assign a request to an agent.
- Each request has only one active assignee.
- Assignment changes are recorded.

**AC-07: Request Responses**
- An assigned agent can add a response note.
- A request cannot be marked as Answered without a response note.

**AC-08: Request Closure**
- A closed request cannot be edited.
- Reopening must be performed through an authorized reopening action.

**AC-09: Role Management**
- A manager can create agent accounts or promote existing customers.
- Customers cannot change their own roles.
- Users can only access features permitted by their roles.

**AC-10: Responsive Interface**
- Main system pages remain usable on desktop, tablet, and mobile screen sizes.
- Important actions and information remain accessible without horizontal overflow.

### 8. Out of Scope

The following features are excluded from the current project:

- Telephone and SMS support services.
- Network and equipment monitoring.
- Employee salary calculation and performance evaluation.
- Knowledge base and user training services.
- Mandatory two-factor authentication (2FA).
- English language interface in the initial version.

### 9. Priority Classification (MoSCoW)

- **Must:** Essential requirements that must be implemented.
- **Should:** Important requirements that should be implemented if resources allow.
- **Could:** Optional features that can improve the system.
- **Won't:** Features excluded from the current project scope.

