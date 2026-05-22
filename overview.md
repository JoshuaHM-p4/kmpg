# ARAIKO: Academic Resource AI Knowledge Orchestrator

## Project Name: ARAIKO Study Sync

### Problem
Sarah, a second-year nursing student at State University, faces severe academic friction because she must manually parse five different, complex syllabi to identify overlapping clinical practicums and major paper deadlines, frequently leading to missed assignments and extreme stress.

### Solution
Sarah uploads her syllabi via a simple interface; **Copilot Studio** then parses the documents, reasons about task duration and difficulty, and automatically suggests and schedules realistic, conflict-free study blocks directly into her **Outlook calendar** via **Power Automate**.

---

## Copilot Studio Features Used

*   **Natural Language Processing (NLP):** Used to interpret vague student requests (e.g., "Help me survive midterms week") and parse unstructured syllabus PDFs.
*   **Generative Answers & Multi-step Reasoning:** Analyzes overlapping deadlines across multiple courses to determine task prioritization and required study duration.
*   **Topic Orchestration:** Routes intents to specific backend automation flows (e.g., parsing a new document vs. adjusting an existing schedule).

---

## Tools Used

*   **Power Automate:** Executes backend workflows, specifically parsing PDF inputs and creating/modifying Outlook Calendar events based on Copilot's logic.
*   **Power Apps:** Provides the frontend mobile-friendly interface for Sarah to upload documents, review Copilot's proposed schedules, and provide feedback.
*   **Microsoft Graph API:** Interfaces with the student's calendar to read free/busy time and write new study sessions.

---

## Target Users

*   **Demographics:** University and college students, typically 18-25 years old, juggling full course loads.
*   **Specific Needs:** They need a way to visualize their entire workload across all classes and find guaranteed time to complete tasks before deadlines hit.
*   **Current Management:** They currently rely on manual entry into static tools like Google Calendar, Notion, or paper planners, which fail to warn them when cumulative workload exceeds available time.
*   **Workflow Integration:** ARAIKO lives in an app they check daily, actively modifying their calendar based on changing conditions rather than waiting for manual updates.

---

## Core Feature (MVP)

**Syllabus Ingestion Flow:**
1.  Sarah uploads a PDF syllabus in **Power Apps**.
2.  **Copilot Studio** extracts key dates and assesses task difficulty using GenAI.
3.  Copilot queries Sarah's **Outlook** via **Power Automate** to find open time blocks.
4.  Copilot proposes a phased study schedule.
5.  Upon Sarah's approval, **Power Automate** creates the calendar events.

---

## Why This Wins

This solution directly addresses a ubiquitous, high-stress student problem by transforming Copilot from a passive chatbot into an active orchestrator of academic life. Judges will find it compelling because it demonstrates complex multi-step reasoning over unstructured data, seamlessly integrates Microsoft tools (Power Apps, Automate, Copilot Studio) into a practical workflow, and clearly improves student outcomes.

---

## Scenarios

### 1. Initial Syllabus Ingestion
*   **Who:** Freshmen starting a new semester.
*   **Trigger:** User uploads three PDF syllabi.
*   **Flow:** Copilot Studio extracts the assignment schedules. It notices two major essays are due on the same Friday. It calculates that writing both will take 15 hours. It checks the calendar, finds available evenings in the preceding two weeks, and proposes a schedule breaking the essays into research, drafting, and editing blocks.
*   **Result:** User approves, and 8 study blocks are added to their calendar.

### 2. Proactive Conflict Resolution
*   **Who:** A student who just picked up an extra work shift.
*   **Trigger:** User tells Copilot, "I have to work on Tuesday from 4 PM to 8 PM."
*   **Flow:** Copilot recognizes this overlaps with a previously scheduled "Biology Midterm Prep" block. It immediately reasons that the prep cannot be skipped. It searches for alternative slots before the midterm and suggests moving the prep to Wednesday morning.
*   **Result:** Copilot updates the calendar event via Power Automate.

### 3. Ambiguous Intent Handling
*   **Who:** An overwhelmed student during midterms.
*   **Trigger:** User types, "I'm falling behind and don't know what to do first."
*   **Flow:** Copilot queries the upcoming deadlines and the user's recent calendar activity. It identifies that a Chemistry lab report is due in 48 hours and requires significant time. It responds: "Your Chem lab is the most urgent. I've found a 3-hour gap tonight at 6 PM. Shall I block that out for Chemistry?"
*   **Result:** User confirms, reducing cognitive load and directing immediate action.

### 4. Task Re-evaluation Based on Progress
*   **Who:** A student who finished a task early.
*   **Trigger:** User marks a "Read Chapter 4" block as complete after 30 minutes instead of the scheduled 1 hour.
*   **Flow:** Copilot asks, "You finished early! Do you want to take a break, or start early on your History reading scheduled for tomorrow?"
*   **Result:** User chooses to start History, and Copilot pulls the future task forward, freeing up time the next day.

### 5. Intelligent Course Material Bridge
*   **Who:** A student preparing for finals.
*   **Trigger:** User asks, "Create a study plan for my Economics final next week."
*   **Flow:** Copilot accesses the previously uploaded syllabus to identify the key concepts covered. It generates a 5-day study plan, dedicating specific days to specific macro/micro concepts, and schedules these blocks into the user's free time.
*   **Result:** A customized, structured review schedule is actively deployed to the student's calendar.

### 6. Direct Task Entry
*   **Who:** A student with a new, one-off deadline.
*   **Trigger:** User says, "I have an exam on Tuesday" or "Remind me about [Task]."
*   **Flow:** Copilot uses built-in Entity Extraction to automatically identify the date and task name. It skips multi-step reasoning and immediately triggers Lawrence's `ChildFlow_CreateEvent` to write the event straight to the Outlook calendar.
*   **Result:** "Noted! I've added your exam on Tuesday to your calendar."
