# KapsCric MVP Software Requirements Specification (SRS)

# 1. Introduction

## 1.1 Purpose
This document outlines the Software Requirements Specification (SRS) for the KapsCric MVP, a web-based platform resembling ESPNCricInfo, focused on IPL matches. The MVP enables administrators to manage match data, commentators to provide live text-based ball-by-ball commentary, and visitors to view matches and scoreboards without registration. The SRS defines functional and non-functional requirements to guide development within a 4-week timeline for an investor demo.

## 1.2 Scope
The KapsCric MVP provides:

Match Management: Role: admin manually adds/edits IPL match details (teams, date, time, venue).
Live Commentary: Role: commentator inputs ball-by-ball commentary, updating the scoreboard automatically.
Public Access: Role: visitor views current/upcoming matches, live commentary, and scoreboards without registration.
User Management: Role: superadmin creates Role: admin, who creates Role: commentator, with email/password authentication.
Scalability and Performance: Supports 30–40 concurrent users with ≤0.5s page load times, hosted on AWS with a staging environment.

## 1.3 Definitions

### Roles:
- superadmin: Creates admin accounts; can perform all actions.
- admin: Adds/edits matches, creates commentator accounts.
- commentator: Provides live commentary and scoreboard inputs.
- visitor: Views matches, commentary, and scoreboards without login.


### MVP: Minimum Viable Product, targeting IPL matches (~70 matches, ~16,800 commentary entries).
- Match Status:
  - Upcoming: Matches with a future date/time.
  - Live: Matches on past date/time without a result.
  - Completed: Matches with a result.

# 2. Functional Requirements

## 2.1 Match Management

- FR1.1: Role: admin shall access a form to add/edit IPL match details (teams, date, time, venue).
  - Fields: Team 1, Team 2, Date (D/M/YY or DD/MMM/YYYY), Time, Venue.
  - Validation: All fields mandatory; date formats as specified (e.g., “1/2/25”, “01/Jan/2025”, or “1st January 2025”).

- FR1.2: Match status shall update automatically based on date/time and result (upcoming, live, completed).
- FR1.3: The homepage shall display current and upcoming matches (teams, date, time, venue) for Role: visitor.

## 2.2 Live Commentary

- FR2.1: Role: commentator shall access a dedicated interface for assigned matches, restricted to authorized commentators.
- FR2.2: The commentary interface shall include buttons for quick inputs (W, 0, 1, 2, 3, 4, 6, wide, no ball) and a text field for optional commentary.
- FR2.3: Commentary shall update after every ball, automatically updating the scoreboard.
- FR2.4: Role: visitor shall view live commentary in real-time (WebSocket preferred, polling every 5 seconds acceptable).

## 2.3 Scoreboard

- FR3.1: Before a match, Role: commentator shall input team details (player names for both teams).
- FR3.2: At the start of an innings, Role: commentator shall select two opening batsmen.
- FR3.3: Before each over, Role: commentator shall select the bowler.
- FR3.4: After each wicket, Role: commentator shall select the next batsman.
- FR3.5: After each delivery, Role: commentator shall input the score (via buttons), updating team and batsman scores and strike rotation automatically.
- FR3.6: The scoreboard shall display runs, wickets, overs, batsmen, bowlers, and other details per scorecard.png, in a polished text-based design.
- FR3.7: Role: visitor shall view the scoreboard in real-time.

## 2.4 User Management

- FR4.1: One Role: superadmin account shall be pre-seeded in the database.
- FR4.2: Role: superadmin shall access an admin panel to create Role: admin accounts.
- FR4.3: Role: admin shall access an admin panel to create Role: commentator accounts.
- FR4.4: Authentication shall use email/password with:
  - Password rules: ≥8 characters, 1 capital letter, 1 number, 1 special character.
Forgot password functionality via email.
- FR4.5: No limits on account creation or visitor access.

# 3. Non-Functional Requirements

- NFR1: Performance: Page load time ≤0.5 seconds for 30–40 concurrent users.
- NFR2: Scalability: System shall support ~16,800 commentary entries for 70 IPL matches, designed for future scalability.
- NFR3: Hosting: Deployed on AWS with a staging environment; portable to Azure/GCP.
- NFR4: Security: Secure email/password authentication and password reset functionality.
- NFR5: Usability: Polished UI for scoreboard and commentary, resembling scorecard.png.
- NFR6: Development Timeline: MVP demo in 4 weeks, supporting Agile sprints with demos.

# 4. System Architecture

- Frontend: React + TypeScript, using Tailwind CSS for styling, hosted via Vite for fast builds.
- Backend: Node.js + Express + TypeScript, handling API requests and authentication.
- Database: MongoDB for flexible schema (matches, commentary, users), with pre-seeded superadmin account.
- Real-Time: WebSocket for commentary and scoreboard updates (fallback to 5-second polling if needed).
- Hosting: AWS EC2 (minimal instance for MVP), with Docker for portability and GitHub Actions for CI/CD.
- Authentication: JWT-based email/password authentication with password reset via email.

# 5. User Stories

- US1: As a Role: admin, I want to add/edit IPL match details via a form, so that visitors can view current/upcoming matches.
- US2: As a Role: commentator, I want a dedicated interface with quick-input buttons, so I can efficiently provide ball-by-ball commentary.
- US3: As a Role: commentator, I want my inputs to automatically update the scoreboard, so I don’t need to manage scores separately.
- US4: As a Role: visitor, I want to view live commentary and scoreboards without registration, so I can follow IPL matches easily.
- US5: As a Role: superadmin, I want to create admin accounts via a panel, so I can delegate match management.
- US6: As a Role: admin, I want to create commentator accounts, so they can be assigned to matches.
- US7: As a Role: admin or commentator, I want to log in securely and recover my password, so I can access my account reliably.

# 6. Assumptions

- scorecard.png mimics ESPNCricInfo’s layout (runs, wickets, overs, batsmen, bowlers).
- No third-party APIs for match or scoreboard data in MVP.
- Historical matches page and commentary error handling deferred post-MVP.
- Minimal server setup (e.g., AWS t3.micro) for 30–40 users.
- Agile development with 2-week sprints, demo after each sprint.

# 7. Constraints

- 4-week timeline for MVP demo.
- Supports ~70 IPL matches, ~16,800 commentary entries.
- No user comments, analytics, or 2FA in MVP.
- Text-based scoreboard with polished design.

# 8. Deliverables

- MVP Features: Match listing, live commentary, scoreboard, user management.
- Documentation: Updated README, technical design document.
- Deployment: Staging environment on AWS with CI/CD pipeline. Environment 
- Demos: Sprint demos every 2 weeks, final demo in 4 weeks.
