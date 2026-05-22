# Build Plan: ARAIKO Study Sync

## 1. Key Milestones

- **Milestone 1 (Day 1 - Hours 1-8): Infrastructure & Interface**
  - Establish environment variables for API connections.
  - Design and build the Power Apps mobile UI for document upload and chat interface.
  - Setup Copilot Studio environment and define primary topics.
- **Milestone 2 (Day 2 - Hours 9-16): Intelligent Orchestration & Backend**
  - Develop Power Automate flows for PDF parsing and data extraction.
  - Implement Copilot Studio multi-step reasoning to analyze overlapping deadlines.
  - Build Power Automate integration with Microsoft Graph API for Calendar management.
- **Milestone 3 (Day 3 - Hours 17-24): Refinement & Demo Prep**
  - Implement data masking for user PII in logs.
  - Refactor Power Automate flows into modular child flows.
  - End-to-end testing of the 5 core scenarios.
  - Record and prepare the 15-minute pitch presentation.

## 2. Timeline Breakdown

- **Day 1: Friday (8 Hours)**
  - 2 Hours: System architecture design and environment setup.
  - 4 Hours: Power Apps UI/UX development (Upload screen, Dashboard, Chat interface).
  - 2 Hours: Copilot Studio basic setup and intent routing configuration.
- **Day 2: Saturday (8 Hours)**
  - 3 Hours: Power Automate document parsing (PDF to structured JSON).
  - 3 Hours: Copilot Studio generative answers and scheduling logic implementation.
  - 2 Hours: Outlook Calendar integration via Power Automate.
- **Day 3: Sunday (8 Hours)**
  - 2 Hours: Modularization and adherence to coding standards.
  - 3 Hours: Scenario testing and bug fixing.
  - 3 Hours: Final polish, script writing, and video recording for the pitch.

## 3. Team Member Tasks

- **Joshua (Agentic Workflow Lead)**
  - Configure Copilot Studio topics and triggers.
  - Design the prompt engineering for generative responses and task difficulty assessment.
  - Ensure Copilot effectively manages ambiguous student inputs.
- **Lawrence (Backend & Automation Engineer)**
  - Develop all Power Automate flows.
  - Handle the logic for extracting data from unstructured PDFs.
  - Manage the Microsoft Graph API connection for reading/writing calendar events.
- **Farhana (UI/UX & Quality Assurance)**
  - Build the Power Apps canvas application.
  - Implement masking for sensitive data.
  - Run end-to-end testing on the MVP flow and ensure modularization standards are met.

## 4. Dependencies & Resources

- Microsoft 365 Developer Account (Access to Power Apps, Power Automate, Copilot Studio).
- Sample Syllabus PDFs (Need 3-5 distinct, complex syllabi for testing).
- Outlook Test Account (To verify calendar creation without disrupting real schedules).

## 5. Coding Standards Compliance

- **Variables:** Use Global variables in Power Apps for user session state. Use Environment Variables in Power Platform to store API endpoints and tenant IDs to ensure portability.
- **Masking:** Implement secure input/output in Power Automate steps handling student names or calendar details to ensure PII is hidden in execution logs.
- **Modularization:** The main scheduling logic in Power Automate will be split into an orchestrator flow that calls specific child flows (e.g., `ChildFlow_ParsePDF`, `ChildFlow_CheckAvailability`, `ChildFlow_CreateEvent`) to promote reusability and clean architecture.

### Team Development Breakdown: ARAIKO Study Sync

| Team Member | Component / Flow Name | Platform | Detailed Responsibilities & Actions |
| --- | --- | --- | --- |
| **Lawrence** (Backend) | `Main_Orchestrator_Flow` | Power Automate | Acts as the primary backend logic hub triggered by Copilot. It routes requests to specific child flows to promote reusability and clean architecture. |
| **Lawrence** (Backend) | `ChildFlow_ParsePDF` | Power Automate | Takes the uploaded syllabus PDF and extracts the unstructured text into structured JSON data. |
| **Lawrence** (Backend) | `ChildFlow_CheckAvailability` | Power Automate | Connects to the Microsoft Graph API to read the student's Outlook calendar and find open blocks of free time. |
| **Lawrence** (Backend) | `ChildFlow_CreateEvent` | Power Automate | Uses the Microsoft Graph API to write new study sessions directly into the student's Outlook calendar upon their approval. |
| **Lawrence** (Backend) | `ChildFlow_ModifyEvent` | Power Automate | Updates existing calendar events (e.g., moving a "Biology Midterm Prep" block) when conflicts like a new work shift arise. |
| **Joshua** (Workflow Lead) | `Topic: Ambiguous Intent` | Copilot Studio | Conversational flow to interpret vague requests (e.g., "falling behind"), query upcoming deadlines, and identify the most urgent task. |
| **Joshua** (Workflow Lead) | `Topic: Syllabus Ingestion` | Copilot Studio | Triggered when a new PDF is uploaded. Uses multi-step reasoning to extract dates, assess task difficulty, calculate required hours, and propose a phased schedule. |
| **Joshua** (Workflow Lead) | `Topic: Proactive Conflict` | Copilot Studio | Recognizes manual schedule changes (e.g., "I have to work Tuesday") and triggers Lawrence's backend flow to find alternative slots for displaced study blocks. |
| **Joshua** (Workflow Lead) | `Topic: Task Re-evaluation` | Copilot Studio | Engages the user when a task is marked complete early, offering to either take a break or pull a future task forward. |
| **Joshua** (Workflow Lead) | `Topic: Course Material Bridge` | Copilot Studio | Generates a multi-day study plan dedicated to specific syllabus concepts (e.g., macro/micro economics) and schedules the blocks. |
| **Joshua** (Workflow Lead) | `Topic: Direct Task Entry` | Copilot Studio | Baseline topic for straightforward commands (e.g., "I have an exam on [Day]"). Uses built-in Entity Extraction (Date, TaskName) and triggers `ChildFlow_CreateEvent` for immediate Outlook calendar entry. |
| **Farhana** (UI/UX & QA) | `App Flow: Document Upload` | Power Apps | The mobile-friendly frontend interface where the student uploads their PDF syllabi. |
| **Farhana** (UI/UX & QA) | `App Flow: Review & Feedback` | Power Apps | The dashboard or chat interface where the student reviews Copilot's proposed schedules and provides confirmation. |
| **Farhana** (UI/UX & QA) | `Data Masking Implementation` | Power Apps / Automate | Secures input/output in steps handling student names or calendar details to ensure PII is hidden in execution logs. |

### Detailed Topic: Direct Task Entry

A baseline topic to handle everyday, straightforward commands without requiring heavy generative AI reasoning.

- **Trigger Phrases:** "I have an exam on [Day]", "Remind me about [Task]", "Add [Event] to my schedule."
- **Entity Extraction:** Uses Copilot Studio's built-in Entity Extraction to automatically grab entities (e.g., "Tuesday" -> `Date` variable, "exam" -> `TaskName` variable).
- **The Action:** Skips complex availability checks and immediately triggers `ChildFlow_CreateEvent` to write directly to the Outlook calendar.
- **The Response:** "Noted! I've added your [TaskName] on [Date] to your calendar."
