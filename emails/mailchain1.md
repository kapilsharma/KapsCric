# Mail chain 1

## Prince (Product Owner) to Mac (Customer)

`Subject: Clarification Questions for KapsCric Project Requirements`

Dear Mac,

Thank you for sharing the initial requirements for the KapsCric project, which aims to create a platform similar to ESPNCricInfo. To ensure we fully understand your vision and can develop a detailed Software Requirements Specification (SRS), we have a few follow-up questions. Since you’ve indicated that communication via email or chat is preferred, we’ve outlined our questions below for your convenience. Please provide as much detail as possible, and feel free to share any additional insights or priorities.

### General Questions

**Scope and Scale**: What is the expected user base for the platform (e.g., number of visitors, users, or concurrent viewers per match)? This will help us plan for scalability and performance.
**Geographic Focus**: Are the matches primarily focused on specific regions, leagues, or tournaments (e.g., international cricket, IPL, domestic leagues)? Should the platform support multiple languages or regions?
**Monetization and Access:** Is the platform intended to be free for all users, or are there plans for premium features (e.g., ad-free experience, exclusive content)? Should certain features be restricted to Role: user after login?

### Feature-Specific Questions

#### Upcoming Matches

**Match Data Source:** Where will the data for upcoming matches come from (e.g., manual entry by Role: admin, third-party API, or another source)? If an API is involved, do you have a preferred provider?
**Match Details:** What specific information should be displayed for upcoming matches (e.g., teams, date, time, venue, match type like T20/ODI/Test, or additional details like weather or pitch conditions)?
**Filtering and Sorting:** Should Role: visitor be able to filter or sort upcoming matches (e.g., by date, team, or tournament)?

#### User Management

**Role Hierarchy and Permissions: **

- Can Role: superadmin also perform Role: admin tasks (e.g., adding matches or assigning commentators)?
- Should Role: admin have permissions to edit or delete matches, or only add them?
Are there any limits on the number of Role: commentator users that can be assigned to a match?


- **User Creation Process:** How should Role: admin and Role: commentator accounts be created (e.g., manually by Role: superadmin, through an approval workflow, or self-registration with verification)?
- **Authentication: **Should the platform support specific authentication methods for Role: user, Role: admin, and Role: commentator (e.g., email/password, social login, or multi-factor authentication)?

#### Match Page

**Live Commentary:**

- Should the ball-by-ball commentary be text-based, or are there plans for audio/video commentary in the future?
- How frequently should commentary updates refresh for Role: visitor and Role: user (e.g., real-time via WebSocket, every few seconds)?
- Should Role: commentator have tools to edit or delete their commentary post-submission?


#### Scoreboard:

- What details should the scoreboard include (e.g., runs, wickets, overs, batting/bowling stats, or a graphical representation)?
- Should the scoreboard update in real-time, and if so, what is the acceptable latency?


#### User Comments:
- What is the approval process for comments by Role: user? For example, should Role: commentator manually approve each comment, or is there an automated moderation system?
- Can Role: user edit or delete their comments after submission?
Should comments be visible to all users immediately after approval, or only during specific match events (e.g., tied to a specific delivery)?


**Match Status:**

- How should the platform handle different match states (e.g., upcoming, live, completed)? Should completed matches remain accessible with historical commentary and scores?
- Should Role: visitor have access to live matches without signing up, or is registration required to view live content?

### Technical and Operational Questions

- **Performance Expectations:** Are there specific performance requirements, such as page load times or handling high traffic during popular matches?
- **Analytics and Reporting:** Should the platform include analytics for Role: admin or Role: superadmin (e.g., user engagement, match viewership, or comment activity)?
- **Hosting Preferences:** Do you have a preferred cloud provider (e.g., Azure, AWS, GCP) or specific requirements for hosting, security, or compliance?
- **Integration Needs:** Are there any third-party integrations required (e.g., payment gateways, social media sharing, or analytics tools)?
- **Maintenance and Updates:** After launch, how frequently do you anticipate adding new features or updating match data? Should we plan for automated updates or manual management?

### Project Timeline and Priorities

- **Feature Prioritization:** Which features are most critical to launch first (e.g., match listing, live commentary, or user comments)? Are there any “nice-to-have” features we can defer to later phases?
- **Timeline:** What is your expected timeline for the initial release? Are there specific milestones or deadlines we should align with?

Please feel free to answer these questions at your convenience, either by addressing each numbered point or providing a general response with relevant details. If there are additional stakeholders we should include in this conversation, let us know their contact information. We’re excited to move forward with KapsCric and ensure the platform meets your expectations.

Looking forward to your response.

Best regards,
Prince,
Product Owner,
KapsCric Project Team.

## Reply 1 Mac to Prince

Hi Prince,

Thanks for reaching with specific questions. Let me try to answer them one by one

### General questions

- Scope and scale: Initially, with MVP, let's assume there will be a few thousand users watching while game is in progress. However, we will first have test runs so initially, there will be just 30-40 users watching commentery during test runs.
- Geographic focus: During MVP, we can consider 90% + users will be in India but we need to think and keep options open assuming there will be several thousand users across the world. However, not need to focus on that for the MVP.
- Monetization and access: It is free for everyone and there are no premium features. However, we can consider it after a few years, once product is popular.

### Feature specific questions

- Upcoming matches
  - Match data source: For now, Role: Admin will do it manually.
  - Match details: For MVP, we will focus only on IPL so they will be T20 matches. We just need to show team, date, time and venue.
  - Filtering and sorting: Not during MVP. For now, we will just show current and upcoming matches. No need to show finished match on home page for now but we may have some separate page to see historical matches.
- User Management
  - Superadmin have ability to perform every thing incuding admin's work. However, they  generally will not do it. Role hirarchy is sumeradmin > admin > commentrator > user > Visitor. Higher role can do anything that lower role could do.
  - It is good to have ability to edit/delete matches but not very crucial for the MVP, provided you can do it directly in database, in case needed.
- User Creation process
  - Superadmin will create admin account. Admin will create commentator account. Only extra feature superadmin have over admin is, he can create admin account. Admin cannot create account for other admin.
  - For MVP, we should support email/password authentication. We can work on 2FA after MVP
- Match page
  - Live commentary
    - During MVP, lets keep text based commentery
    - Realtime update will be nice. However, if it is difficuly or will have lot of server load, updating every 5 seconds is fine during MVP.
  - Scoreboard
    - We should cover same details like cricinfo. A screenshot is attached
    - ![Scorecard](../images/scorecard.png)
  - User comment
    - Let's skip user comment from MVP. Wewill do it later.
- Match status
  - Home page should show only current and upcoming matches. We will have separate page to show historical matches.
  - Yes, visitors can see commentery and scorecard. There is no registration needed.
- Technical and operational questions
  - Performance expectation: Page load time should be minimal, not more than half second. During MVP, to keep server cost low, we can consider there will be only 40-50 visitors max.
  - No analytics and reporting needed during MVP.
  - Hosting preference: AWS is preferred to start with but we should not be bound to AWS. We should be able to switch ot Azure and GCP. There is no compliance requirements.
  - Integration: There is no third party integration planned yet. However, we must have option open for the future.
  - Maintenance: It will be continuous maintenance and updates for new features. However, let's focus on quickly delivering MVP for now.
- Timelines
  - For MVP, we need ability to show matches, do commentery and view live commentery.
  - We expect MVP as soon as possible to showcase the idea to investors. No need for lot of features, as our focus is on fast delivery and approval from stake holders.

Let me know if you have any other questions

Thanks,
Mac,
KAPsCric.

## Reply 2 Prince to Mac

Subject: Follow-Up Questions for KapsCric MVP Requirements

Dear Mac,

Thank you for your detailed responses regarding the KapsCric project. Your answers have provided a clear direction for the MVP, focusing on IPL match listings, live text-based commentary, and a scoreboard, with a streamlined user management system. To finalize the Software Requirements Specification (SRS) and ensure our design aligns with your expectations, we have a few additional questions. As before, we’ve outlined them below for your convenience. Please address these at your earliest convenience, either by responding to each point or providing general clarifications.

### Feature-Specific Questions

#### Upcoming Matches

**Match Data Entry:** Since Role: admin will manually enter match data, should there be a form or interface for inputting match details (e.g., teams, date, time, venue)? Are there specific validation rules (e.g., mandatory fields, date formats)?
Historical Matches: You mentioned a separate page for historical matches. Should this page be included in the MVP, or can it be deferred to a later phase? If included, what details should be displayed (e.g., final scores, commentary archive)?
Match Status Updates: Who updates the match status (e.g., from “upcoming” to “live” to “completed”)? Is this a manual process by Role: admin, or should it be automated based on date/time?

Live Commentary

Commentary Input: Should Role: commentator have a specific interface for ball-by-ball commentary (e.g., predefined fields for ball number, runs, wickets, or free-text)? Are there templates or standard phrases they can use to speed up input?
Commentary Volume: What is the expected frequency of commentary updates per over (e.g., one comment per ball, or more/less frequent)? This will help us optimize the real-time update mechanism (WebSocket or polling every 5 seconds).
Error Handling: If a Role: commentator makes a mistake in their commentary, should they be able to edit/delete entries during the match, or is this not needed for MVP?

Scoreboard

Scoreboard Details: The provided scorecard.png shows a detailed layout (runs, wickets, overs, batsmen, bowlers, etc.). Should Role: admin or Role: commentator manually input scoreboard data (e.g., runs per ball, wickets), or should we plan for integration with a future API? For MVP, who is responsible for updating the scoreboard in real-time?
Display Format: Should the scoreboard be purely text-based, or are graphical elements (e.g., progress bars, run-rate graphs) required for MVP?

User Management

Account Creation Workflow: Should Role: superadmin and Role: admin have an admin panel or interface to create accounts, or is direct database access sufficient for MVP?
Role Limits: Is there a maximum number of Role: admin or Role: commentator accounts that can be created (e.g., to prevent overloading the system during MVP)?
Visitor Access: Should Role: visitor have any limitations on viewing live commentary or scoreboards (e.g., rate limits, session timeouts)?

Technical and Operational Questions

Database Storage: What is the expected volume of match data and commentary for the MVP (e.g., number of matches, average commentary entries per match)? This will guide our database design.
Real-Time Updates: You mentioned real-time updates are preferred but polling every 5 seconds is acceptable. Are there specific performance constraints (e.g., server cost limits) we should prioritize over real-time accuracy?
Security: For email/password authentication, are there specific password strength requirements or account recovery processes (e.g., password reset via email) needed for MVP?
Testing Environment: Since you mentioned test runs with 30–40 users, should we set up a separate staging environment for these tests, or is the production environment sufficient?

Timeline and Deliverables

MVP Definition: To confirm, the MVP includes:
Role: admin manually adding IPL match details (teams, date, time, venue).
Role: commentator providing text-based ball-by-ball commentary.
Role: visitor viewing current/upcoming matches and live commentary/scoreboard without registration.
Email/password authentication for Role: admin and Role: commentator.Is this correct, or are there additional features you’d like included in the MVP?


Timeline Expectations: You emphasized a fast delivery for the MVP to showcase to investors. Can you specify a target delivery date (e.g., 4–6 weeks from now) or any investor demo deadlines?

Please let us know your responses to these questions or any additional details that could help us refine the requirements. If there are other stakeholders we should include, kindly share their contact information. We’re eager to move forward with the SRS and begin designing the system to meet your goals.
Best regards,PrinceProduct OwnerKapsCric Project Team


## Reply 3: Mac to Prince

Hi Prince,

Thanks for taking the time and come up with the clarification questions. PFA my responses below

- Feature sepcific questions
  - Upcoming Matches
    - Match data entry
      - Yes, we should have some page or form for Admin to add/edit matches. Site wise, date format should be D/M/YY like 1/2/25 for short date and DD/MMM/YYYY like 01/Jan/2025 for long date. If date comes in text, it should be like 1st January 2025 or Jaruary 1, 2025.
      - We can defer historical matches page for a later time to get MVP quickly.
      - Match status: It should happen automatically based on current date and time. Match in future date will be upcoming matches. Match on past date time but not finished (no result) will be current matches. Matches with result will be considered as historical matches.
    - Live commentary
      - Commentary input: Yes, we must have specific page, accessible only to the commentators assigned on a given match. We may want to discuss about that page in details, so that, commentator can do efficient commentery with minimal input. For example, for every ball, he should get buttoms like W, 0, 1, 2, 3, 4, 6 so that result of ball is published quickly. We could also have button for wide, no ball, etc. Later they can add text commentery to be updated later.
      - Commentary volume: Commentary will be updated after every ball. Updating commentary should automatically update the scoreboard.
      - Error handling is a much needed feature as anyone can make a mistake. However, it can be deffered until after MVP.
    - Scoreboard
      - Scoreboard details: No, it must be updated automatically. Before the match  start, we can ask the commentator for the detials like player details for both teams. When an inning start, commentator should enter which two batsman are opening from the list of players. After every wicket, we should ask who is new batsman. Similarly, before every over, we must ask who is the next baller. After every deliver, we enter score. Based on score, we must update batsman and team score as well as who is next batsman on strike.
      - Display format: For MVP, let's keep it text-based. In future, we will need graphical elements. However, text based do not mean minimal design. It must look polished design as in the screenshot attached in previous mail.
    - User management
      - Yes, we must have an interface for all kind of data entry, including account creation. Only exception is one super admin account, which must be present in database so that he can login and add other data.
      - Rate limit: There is no such limit. However, we can assume in the starting, there will be less than 15 system accounts. System accounts include superadmin, admin and commentator.
      - Visitor access: No there is no limit on visitors. We want them to have best experience, as they are our end customers. Higher the number and better the experience for tham will be directly propotional to the revanue.
    - Technical and operational questions
      - Our first target is next season of IPL. There will be up to 70 matches per IPL season. Each match will have two inninga of 20 ovurs. Each over have 6 balls + few extra balls like wide, no ball etc. So on rough target. there could be 70 * 40 * 6 = 16800 commentery in first IPL season. However, later from next year, we plan to cover all national and international matches so the colume can be really very high. However, we need not to worry about it during MVP but must design our system so that it can scale.
      - Real-time update: We should keep server cost minimal but it is not a constraints. Performance has higher priority than cost. I would suggest we start with minimal server for MVP, which will be tested internally with limited users. We should worry about scale only once we go public so we have few month time to reach there.
      - Security: For MVP, username password are enough. We can have password length of 8 characters minimum with at least one Capital letter, one number and a special character. We definitely need account recovery provess. User should be able to reset the password through forgot password functionality.
      - Test engironment: Testing will have separate test environment. MVP is available internally and will be there on stage environment. We will create production environment once we get stakeholder approval on MVP. So no need to worry about production environment for now.
    - Timeline and deliverables
      - MVP definition looks good.
      - For MVP, we can skip few time taking parts to give initial demo as soon as possible. I'm looking for 4 weeks timeline for initial demo or Agile based development with demo after every sprint.

I hope this clear some of the doubts. Feel free to let me know if you have other questions.

Thanks,
Mac.