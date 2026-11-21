# 🎫 Support Request Tracking System
### Group E | Software Engineering Laboratory | Fall 1405

A Python 3-based project for analyzing, designing, and optionally implementing a support request tracking system.

## 📌 Project Overview

The Support Request Tracking System is designed to help support teams manage user requests from submission to closure.

Currently, support requests are received through different communication channels, such as email and messaging applications. As a result, some requests may remain unanswered or become difficult to track.

This project aims to provide an organized solution for registering requests, assigning support agents, managing priorities, tracking request statuses, and generating reports.

The system ensures that every request has a clear lifecycle and an assigned person responsible for handling it.

## 🎯 Project Objectives

- Centralize support request management.
- Ensure every request has an assigned support agent.
- Make request priorities clear and manageable.
- Track requests throughout their lifecycle.
- Prevent invalid status transitions.
- Maintain response and assignment history.
- Provide reports for monitoring request statuses.

## ✨ Required Features

The system must support the following six core functionalities:

### 1. Request Registration
Create a support request with:
- Title
- Description
- Category
- Urgency level

### 2. Support Agent Assignment
- Assign each request to a support agent.
- Maintain only one active assigned agent per request.
- Record every assignment change.

### 3. Request Status Management

Requests must follow the defined status workflow:

| Status | Description |
|---|---|
| Open | Newly registered request |
| Under Review | Request is being investigated |
| Answered | A response has been provided |
| Closed | Request has been completed and closed |

### 4. Response Notes
- Register response notes for each request.
- Maintain response history.
- Require at least one response note before moving a request from Under Review to Answered.

### 5. Agent Request Queue
- Display open requests assigned to a specific support agent.
- Sort requests according to urgency.
- Display only high-priority requests when high-priority requests exist in the agent's queue.

### 6. Request Reports
Generate reports for a specified date range, including:
- Number of open requests.
- Number of closed requests.
- Number of answered requests.

## 📋 Business Rules & Acceptance Criteria

The system must comply with the following rules:

1. Each request can have only one active assigned support agent.
2. Any change of the assigned agent must be recorded.
3. Closed requests cannot be edited.
4. A closed request can only be reopened through an explicit reopening event.
5. High-urgency requests must appear at the beginning of the assigned agent's queue.
6. If high-urgency requests exist in an agent's queue, only those requests must be displayed.
7. A request cannot transition from Under Review to Answered without at least one registered response note.
8. The list of closed requests must remain separate from the active request queue.

## 🚫 Out of Scope

The following features are explicitly excluded from this project:

- Telephone and SMS support systems.
- Network and equipment quality monitoring.
- Support agent salary calculation and performance evaluation.
- Knowledge base and user training systems.

## ⚙️ Technology & Development Environment

| Component | Technology |
|---|---|
| Programming Language | Python 3 |
| Version Control | Git |
| Repository Hosting | GitHub |
| Suggested Database | SQLite |
| Documentation | Markdown |
| UML Modeling | Enterprise Architect / PlantUML / diagrams.net |
| Testing Framework | pytest |
| Test Coverage (Optional) | pytest-cov |

The project is designed to run on laboratory or personal computers using a Python virtual environment. An internet connection is not required for the application itself.

## 📁 Repository Structure

The repository will follow an organized structure for source code, documentation, tests, and project deliverables.

```text
support-request-tracking/
│
├── src/
│   └── ...
│
├── tests/
│   └── ...
│
├── docs/
│   ├── vision.md
│   ├── requirements.md
│   ├── usecases.md
│   ├── analysis.md
│   ├── design.md
│   ├── tests.md
│   └── report.md
│
├── diagrams/
│   └── ...
│
├── README.md
├── requirements.txt
└── .gitignore
```

*Note: The structure may evolve during the analysis and development phases.*

## 📚 Required Project Deliverables

According to the project specification, Group E must prepare the following deliverables:

| Deliverable | Description |
|---|---|
| `docs/vision.md` | Project vision document (maximum 2 pages) |
| `docs/requirements.md` | Requirements with IDs, MoSCoW priorities, and acceptance criteria |
| `docs/usecases.md` | Use case diagram and descriptions of two main use cases |
| `docs/analysis.md` | Activity diagram and conceptual domain model |
| `docs/design.md` | Class diagram and sequence diagram |
| UI Prototype | Prototype consisting of 3 pages or 3 states |
| `docs/tests.md` | Test plan containing at least 10 test cases, including boundary and error cases |
| `docs/report.md` | Final project report |
| Presentation | 10-minute final presentation |

UML diagrams must be included in the repository as image exports or appropriate project files.

## 🧪 Educational Focus

The main educational challenge of Group E is modeling the request status machine.

The team must:

- Identify and model all valid status transitions.
- Identify and prevent invalid status transitions.
- Prevent a transition from Under Review to Answered without a response note.
- Design at least one test case for each status transition.

Special attention must be given to the correctness of the request lifecycle and its corresponding tests.

## 🤝 Team Collaboration

This project is developed collaboratively by **Group E**.

All team members are expected to participate in analysis, design, documentation, testing, and review.

### Contribution Workflow

1. Fork the original repository to your GitHub account.
2. Clone your fork to your local machine.
3. Create a separate branch for your assigned task.
4. Implement your changes.
5. Commit changes with meaningful commit messages.
6. Push your branch to your fork.
7. Open a Pull Request against the original repository.
8. Wait for review and approval before merging.

**Important:** Do not push directly to the original repository's `main` branch. All contributions must be reviewed through Pull Requests.

### Branch Naming Convention

Suggested branch names:

```text
main
develop

feature/request-registration
feature/agent-assignment
feature/status-management
feature/response-notes
feature/agent-queue
feature/reporting

docs/requirements
docs/uml-diagrams
docs/test-plan
```

### Commit Message Examples

```text
feat: add support request registration
feat: implement agent assignment
fix: prevent invalid status transitions
test: add request lifecycle tests
docs: update requirements documentation
```

## 🚀 Getting Started

### Prerequisites

- Python 3
- Git
- A code editor (e.g. Visual Studio Code or PyCharm)

### 1. Fork the Repository

Fork this repository to your GitHub account.

### 2. Clone Your Fork

```bash
git clone https://github.com/alirezach497/support-ticket-system.git
```

### 3. Navigate to the Project

```bash
cd REPOSITORY-NAME
```

### 4. Create a Virtual Environment

```bash
python -m venv venv
```

Activate the environment:

**Windows:**
```bash
venv\Scripts\activate
```

**Linux / macOS:**
```bash
source venv/bin/activate
```

### 5. Install Dependencies

If a `requirements.txt` file is available:

```bash
pip install -r requirements.txt
```

### 6. Run Tests

```bash
pytest
```

*Note: Execution commands may be updated as the implementation structure is finalized.*

## 📅 Project Schedule

The project follows a 12-session educational schedule.

| Session | Main Activity |
|---|---|
| 1 | Team formation, repository setup, and Git workflow |
| 2 | Requirements gathering and prioritization |
| 3 | Vision document, use cases, and UI prototype |
| 4 | Activity diagrams and domain analysis |
| 5–6 | Class and sequence diagrams |
| 7 | Test planning and implementation decision |
| 8–10 | Implementation (if selected), testing, and progress reports |
| 11 | Final documentation and presentation preparation |
| 12 | Final presentation and individual defense |

## 🏆 Evaluation

The main project evaluation is based on 20 points:

| Evaluation Part | Points |
|---|---:|
| Group Evaluation | 12 |
| Individual Evaluation | 8 |
| **Total** | **20** |

Implementation and executable testing are optional additional activities.

A maximum of 2 bonus points may be awarded for delivering executable code with at least 10 successful pytest tests, execution instructions, and coverage of the main Must scenarios.

Not submitting code does not reduce the main project grade.

## 👥 Team Information

| Role | Member |
|---|---|
| Team Leader | To be announced |
| Team Member 2 | To be announced |
| Team Member 3 | To be announced |
| Team Member 4 | To be announced |

---

**Course:** Software Engineering Laboratory  
**Group:** E  
**Academic Term:** Fall 1405  
**Project:** Support Request Tracking System

Developed collaboratively by Group E.
