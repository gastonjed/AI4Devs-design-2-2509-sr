# LTI ATS - User Stories

**Document Purpose**: Product backlog of user stories for the LTI Applicant-Tracking System

**Date Created**: 2025-12-02

**Status**: Draft - Ready for Refinement

---

## Table of Contents

- [Introduction](#introduction)
- [User Personas](#user-personas)
- [Epic Overview](#epic-overview)
- [User Stories by Priority](#user-stories-by-priority)
- [Story Map](#story-map)
- [Next Steps](#next-steps)

---

## Introduction

This document contains all user stories for the LTI ATS system, organized by priority and mapped to epics. Each story follows the standard format:

**As a** [persona], **I want** [capability], **so that** [benefit/value]

Stories are prioritized using MoSCoW method:

- **Must Have (P0)**: Critical for MVP launch
- **Should Have (P1)**: Important but not critical for initial launch
- **Could Have (P2)**: Desirable for competitive feature set
- **Won't Have (P3)**: Future consideration, not in current scope

---

## User Personas

### Primary Personas

**P1: Recruiter (Rita)**

- **Role**: Corporate Recruiter
- **Goals**: Fill positions quickly with quality candidates, maintain positive candidate experience
- **Pain Points**: Overwhelmed with applications, manual screening takes 60% of day, scheduling chaos
- **Tech Savviness**: High - uses multiple tools daily

**P2: Hiring Manager (Henry)**

- **Role**: Engineering Manager
- **Goals**: Hire best talent for team, make data-driven decisions, minimize time spent in hiring process
- **Pain Points**: Lack of visibility into pipeline, delayed feedback from interviewers, gut-feel decisions
- **Tech Savviness**: Medium - focused on efficiency

**P3: Interviewer (Irene)**

- **Role**: Senior Software Engineer
- **Goals**: Evaluate candidates fairly, provide timely feedback, minimize administrative overhead
- **Pain Points**: Email-based feedback requests, unclear evaluation criteria, forgotten feedback deadlines
- **Tech Savviness**: High - prefers mobile-first tools

### Secondary Personas

**P4: Candidate (Carlos)**

- **Role**: Job Seeker
- **Goals**: Find right job, understand application status, have positive experience
- **Pain Points**: Black hole applications, no status updates, impersonal communication
- **Tech Savviness**: Varies - system must be intuitive

**P5: HR Leader (Helen)**

- **Role**: VP of People Operations
- **Goals**: Optimize recruitment ROI, ensure compliance, strategic hiring planning
- **Pain Points**: No visibility into recruitment metrics, compliance risks, budget justification
- **Tech Savviness**: Low-Medium - needs simple dashboards

**P6: Employee (Emma)**

- **Role**: Current Employee
- **Goals**: Refer great candidates, earn referral bonuses, help company grow
- **Pain Points**: Don't know about open positions, unclear referral process, no bonus tracking
- **Tech Savviness**: Medium

---

## Epic Overview

| Epic ID | Epic Name | Description | Target Personas |
|---------|-----------|-------------|-----------------|
| **E1** | Job Management | Create, publish, and manage job postings across multiple channels | Rita (Recruiter) |
| **E2** | Candidate Screening | AI-powered candidate evaluation and pipeline management | Rita (Recruiter), Henry (Hiring Manager) |
| **E3** | Interview Management | Schedule, conduct, and collect feedback for interviews | Rita, Henry, Irene (Interviewer) |
| **E4** | Candidate Communication | Automated and personalized candidate engagement | Rita (Recruiter), Carlos (Candidate) |
| **E5** | Analytics & Reporting | Recruitment metrics, insights, and strategic planning | Helen (HR Leader), Henry |
| **E6** | Talent Pool Management | Search and engage past candidates for new opportunities | Rita (Recruiter) |
| **E7** | Offer Management | Create, send, and track job offers | Rita (Recruiter), Henry |
| **E8** | Employee Referrals | Internal referral tracking and incentive management | Rita, Emma (Employee) |

---

## User Stories by Priority

### Must Have (P0) - MVP Critical

#### Epic 1: Job Management

**US-001** [P0] Post Job to Career Site

**As a** Recruiter,
**I want to** create a job posting with title, description, and requirements,
**so that** candidates can discover and apply for open positions on our career site.

**Acceptance Criteria**:

- Given I have a job requisition approved, when I create a job with required fields (title, description, location), then the job is published to the career site within 5 minutes
- Given I publish a job, when candidates visit the career site, then they can see the job listing and apply
- Given a job is published, when I view the job dashboard, then I see application count and views

**Story Points**: 8

**Dependencies**: None (foundational feature)

---

**US-002** [P0] Distribute Job to Multiple Channels

**As a** Recruiter,
**I want to** automatically post jobs to LinkedIn, Indeed, and other job boards from a single interface,
**so that** I can maximize candidate reach without manual posting to each platform.

**Acceptance Criteria**:

- Given I publish a job, when I select distribution channels (LinkedIn, Indeed, career site), then the job is posted to all selected channels with unique tracking URLs
- Given a job is distributed, when I view analytics, then I can see application source breakdown by channel
- Given a job board integration fails, when the error occurs, then I receive an alert with manual posting instructions

**Story Points**: 13

**Dependencies**: US-001

---

**US-003** [P0] Use Job Templates

**As a** Recruiter,
**I want to** create jobs from pre-built templates for common roles,
**so that** I can reduce time spent writing job descriptions from scratch.

**Acceptance Criteria**:

- Given I start creating a job, when I select a template (e.g., "Senior Software Engineer"), then all fields are pre-filled with template content
- Given I have created a job, when I save it as a template, then it appears in my template library for future use
- Given I use a template, when I edit fields, then only my current job is affected (template remains unchanged)

**Story Points**: 5

**Dependencies**: US-001

---

#### Epic 2: Candidate Screening

**US-010** [P0] Parse Resumes Automatically

**As a** Recruiter,
**I want** candidate resumes to be automatically parsed and data extracted (name, email, skills, experience),
**so that** I don't waste time manually entering candidate information.

**Acceptance Criteria**:

- Given a candidate applies with a resume, when the resume is uploaded, then key fields (name, email, phone, skills, experience) are extracted within 10 seconds
- Given resume parsing completes, when I view the candidate profile, then extracted data is pre-filled in structured fields
- Given parsing fails, when extraction confidence is low, then the system flags fields for manual review

**Story Points**: 13

**Dependencies**: US-001 (need jobs to receive applications)

---

**US-011** [P0] Score Candidates with AI Matching

**As a** Recruiter,
**I want** candidates to be automatically ranked by AI fit score (0-100),
**so that** I can quickly identify the most qualified applicants.

**Acceptance Criteria**:

- Given a candidate applies, when resume is parsed, then an AI match score (0-100) is calculated based on job requirements within 10 seconds
- Given candidates are scored, when I view the pipeline, then candidates are sorted by score (highest first) by default
- Given I view a candidate profile, when I see the match score, then I also see highlighted matching criteria (✅ requirements met, ⚠️ gaps)

**Story Points**: 21

**Dependencies**: US-010

---

**US-012** [P0] Filter and Search Candidates

**As a** Recruiter,
**I want to** filter candidates by skills, experience, location, and other criteria,
**so that** I can narrow large applicant pools to the most relevant candidates.

**Acceptance Criteria**:

- Given I have candidates in a job pipeline, when I apply filters (e.g., "5+ years Python", "AWS certified"), then only matching candidates are displayed
- Given I have complex search needs, when I use Boolean search (AND, OR, NOT), then results match my query logic
- Given I create a useful filter combination, when I save it, then I can reuse it for future searches

**Story Points**: 8

**Dependencies**: US-010

---

**US-013** [P0] Move Candidates Through Pipeline

**As a** Recruiter,
**I want to** drag and drop candidates between pipeline stages (Applied → Phone Screen → Interview → Offer),
**so that** I can visually manage candidate progress.

**Acceptance Criteria**:

- Given I view a job pipeline, when I drag a candidate from "Applied" to "Phone Screen", then the candidate's status updates and they receive an automated notification
- Given I select multiple candidates, when I use bulk actions to move them to a new stage, then all candidates are updated simultaneously
- Given I move a candidate backward (e.g., Interview → Phone Screen), when this occurs, then the system logs the reason and alerts the hiring manager

**Story Points**: 8

**Dependencies**: US-011

---

**US-014** [P0] Reject Candidates with Compliance

**As a** Recruiter,
**I want to** reject candidates with a required rejection reason,
**so that** we maintain EEOC compliance and audit trails.

**Acceptance Criteria**:

- Given I reject a candidate, when I select "Reject" action, then I must choose a rejection reason from a predefined list before proceeding
- Given a rejection reason is selected, when I confirm, then the decision is logged with timestamp, reason, and recruiter ID for audit purposes
- Given a candidate is rejected, when the action completes, then they receive a professional rejection email (automated template)

**Story Points**: 5

**Dependencies**: US-013

---

#### Epic 4: Candidate Communication

**US-030** [P0] Send Automated Status Emails

**As a** Recruiter,
**I want** candidates to automatically receive email updates when their application status changes,
**so that** they have a positive experience and I don't manually send hundreds of emails.

**Acceptance Criteria**:

- Given a candidate's status changes (e.g., Applied → Phone Screen), when the change occurs, then they receive an automated email within 5 minutes
- Given an automated email is sent, when the candidate receives it, then it includes personalized details (name, job title, next steps)
- Given I am a recruiter, when I view email templates, then I can customize content while maintaining required compliance language

**Story Points**: 8

**Dependencies**: US-013

---

**US-031** [P0] Use Email Templates

**As a** Recruiter,
**I want to** send emails to candidates using pre-built templates (phone screen invite, rejection, offer, etc.),
**so that** I maintain consistent communication without writing from scratch each time.

**Acceptance Criteria**:

- Given I need to email a candidate, when I select an action (e.g., "Request Info"), then I can choose from relevant email templates
- Given I select a template, when I review it, then placeholders (candidate name, job title, company) are auto-filled
- Given I send an email, when the candidate receives it, then it appears professional and on-brand

**Story Points**: 5

**Dependencies**: US-030

---

### Should Have (P1) - Important for Competitive Product

#### Epic 3: Interview Management

**US-020** [P1] Schedule Interviews with Calendar Integration

**As a** Recruiter,
**I want to** schedule interviews that automatically sync with Google Calendar and Outlook,
**so that** I eliminate back-and-forth email coordination.

**Acceptance Criteria**:

- Given a candidate advances to interview stage, when I schedule an interview, then calendar invites are sent to all participants (candidate, interviewers) with meeting details
- Given I create an interview, when I check availability, then the system shows interviewer availability from their calendars
- Given an interview is scheduled, when I view the candidate profile, then I see all upcoming and past interviews in a timeline view

**Story Points**: 13

**Dependencies**: US-013

---

**US-021** [P1] Create Interview Kits

**As a** Recruiter,
**I want to** prepare interview kits with assigned questions per interviewer,
**so that** interviews are structured and all team members know their role.

**Acceptance Criteria**:

- Given I schedule an interview, when I create an interview kit, then I can assign specific questions to each interviewer (e.g., "Tech Lead: System Design Q1-3")
- Given an interview kit is created, when I attach documents (resume, AI match report), then all interviewers can access them from their calendar invite
- Given an interviewer receives their kit, when they open it, then they see only their assigned questions and candidate background

**Story Points**: 8

**Dependencies**: US-020

---

**US-022** [P1] Collect Structured Interview Feedback

**As a** Hiring Manager,
**I want** interviewers to submit feedback using structured scorecards (1-5 ratings + comments),
**so that** I can make data-driven hiring decisions instead of relying on gut feel.

**Acceptance Criteria**:

- Given an interview completes, when the interviewer opens the feedback form, then they see a scorecard with evaluation criteria (Technical Skills, Communication, Culture Fit, etc.) and a 1-5 rating scale
- Given an interviewer submits feedback, when they rate a candidate below 3 or above 4, then they must provide written justification
- Given I am a hiring manager, when I view aggregated feedback, then I see average scores, individual ratings, and highlighted consensus/conflicts

**Story Points**: 13

**Dependencies**: US-021

---

**US-023** [P1] Receive Feedback Reminders

**As an** Interviewer,
**I want to** receive reminder notifications if I haven't submitted feedback within 12 hours,
**so that** I don't forget and hold up the hiring process.

**Acceptance Criteria**:

- Given an interview completes, when 12 hours pass without feedback submission, then I receive a reminder email/notification
- Given 24 hours pass, when feedback is still not submitted, then my hiring manager receives an escalation notification
- Given I submit feedback, when it's completed, then reminders stop and the hiring manager is notified

**Story Points**: 5

**Dependencies**: US-022

---

**US-024** [P1] Flag Conflicting Interview Feedback

**As a** Hiring Manager,
**I want** the system to automatically flag conflicting feedback (e.g., 2 "Strong Yes", 2 "No"),
**so that** I can facilitate a debrief meeting to reach consensus.

**Acceptance Criteria**:

- Given all interviewers submit feedback, when recommendations conflict significantly, then the system flags the candidate profile with "Conflicting Feedback" badge
- Given conflicting feedback exists, when I view the dashboard, then I see suggested actions (schedule debrief meeting, request additional interview)
- Given I resolve the conflict, when I make a final decision, then the resolution is logged with justification

**Story Points**: 8

**Dependencies**: US-022

---

#### Epic 5: Analytics & Reporting

**US-040** [P1] View Recruitment Dashboard

**As an** HR Leader,
**I want to** see a real-time dashboard with key metrics (time-to-hire, applications by source, pipeline conversion),
**so that** I can monitor recruitment health at a glance.

**Acceptance Criteria**:

- Given I log into the system, when I navigate to the dashboard, then I see key metrics: total open jobs, active candidates, time-to-hire average, source effectiveness
- Given metrics are displayed, when I click on a metric, then I can drill down to see detailed breakdowns (e.g., time-to-hire by department, by job type)
- Given the dashboard loads, when data is requested, then it refreshes in under 3 seconds

**Story Points**: 13

**Dependencies**: Multiple (US-001, US-010, US-013 for data collection)

---

**US-041** [P1] Generate Custom Reports

**As an** HR Leader,
**I want to** create custom reports with selected metrics and date ranges,
**so that** I can analyze specific recruitment trends and present data to executives.

**Acceptance Criteria**:

- Given I need a report, when I use the report builder, then I can select metrics (time-to-hire, cost-per-hire, source ROI), filters (date range, department, job type), and visualization type (chart, table)
- Given I create a report, when I save it, then I can schedule it to run automatically (daily, weekly, monthly) and email results to stakeholders
- Given a report is generated, when I download it, then I can export as PDF, Excel, or CSV

**Story Points**: 13

**Dependencies**: US-040

---

### Could Have (P2) - Competitive Differentiators

#### Epic 6: Talent Pool Management

**US-050** [P2] Search Past Candidates

**As a** Recruiter,
**I want to** search our entire candidate database (including past applicants and rejected candidates),
**so that** I can quickly fill new roles with pre-vetted talent.

**Acceptance Criteria**:

- Given I have a new job opening, when I search the talent pool with criteria (skills, experience, location), then I see all matching candidates (including those from past job applications)
- Given I find a past candidate, when I view their profile, then I see their application history, past interview feedback, and rejection reasons (if applicable)
- Given a candidate was a "silver medalist" (strong but not hired), when I search, then they are ranked higher in results

**Story Points**: 13

**Dependencies**: US-010, US-012 (reuse search/filter logic)

---

**US-051** [P2] Save and Reuse Talent Searches

**As a** Recruiter,
**I want to** save my talent pool searches with filters,
**so that** I can quickly rerun them when similar jobs open.

**Acceptance Criteria**:

- Given I create a complex search (e.g., "Python + AWS + 5+ years + Remote"), when I save it, then it appears in my saved searches list
- Given I have saved searches, when a new candidate matches criteria, then I receive a notification (optional alert setting)
- Given I run a saved search, when results appear, then I can see "New since last run" badge on recently added candidates

**Story Points**: 5

**Dependencies**: US-050

---

#### Epic 7: Offer Management

**US-060** [P2] Generate Offer Letters

**As a** Recruiter,
**I want to** create offer letters from templates with auto-filled candidate and job details,
**so that** I can send professional offers quickly.

**Acceptance Criteria**:

- Given a candidate is approved for offer, when I create an offer letter, then I can select from templates (full-time, contract, intern) with placeholders auto-filled (name, title, salary, start date)
- Given I generate an offer, when I preview it, then it displays as a formatted PDF with company branding
- Given an offer is created, when I send it, then the candidate receives it via email with secure access link

**Story Points**: 8

**Dependencies**: US-013

---

**US-061** [P2] Track Offer Acceptance

**As a** Recruiter,
**I want to** track offer status (sent, viewed, accepted, declined, negotiating),
**so that** I know where we stand and can follow up appropriately.

**Acceptance Criteria**:

- Given I send an offer, when the candidate opens the email, then the status updates to "Viewed" with timestamp
- Given a candidate accepts or declines, when they respond, then the status updates automatically and I receive a notification
- Given an offer is declined, when this occurs, then I can log the decline reason for future analysis

**Story Points**: 5

**Dependencies**: US-060

---

#### Epic 8: Employee Referrals

**US-070** [P2] Submit Employee Referrals

**As an** Employee,
**I want to** submit referrals for open jobs with candidate contact info,
**so that** I can help my company hire great talent and earn referral bonuses.

**Acceptance Criteria**:

- Given I am an employee, when I view open jobs, then I see a "Refer a Friend" button on each job
- Given I click refer, when I submit a referral form (candidate name, email, relationship, why good fit), then the referral is created and linked to my employee profile
- Given my referral applies, when they enter the system, then they are tagged with my referral ID

**Story Points**: 8

**Dependencies**: US-001

---

**US-071** [P2] Track Referral Status and Bonuses

**As an** Employee,
**I want to** see the status of my referrals and potential bonus amounts,
**so that** I know if my referrals are progressing and when I'll receive rewards.

**Acceptance Criteria**:

- Given I submitted referrals, when I check my referral dashboard, then I see each referral's current status (Applied, Interviewing, Hired, Rejected)
- Given my referral is hired, when they complete their probation period, then my bonus is marked as "Approved" with payout date
- Given bonus rules exist, when I view open jobs, then I can see the referral bonus amount for each position

**Story Points**: 8

**Dependencies**: US-070

---

### Won't Have (P3) - Future Consideration

**US-080** [P3] Native Mobile Apps

**As a** Recruiter,
**I want** native iOS and Android apps,
**so that** I can review candidates and approve actions on-the-go.

**Reason Deferred**: Web-responsive interface sufficient for MVP; mobile apps add significant development overhead

---

**US-081** [P3] Chatbot for Candidate FAQs

**As a** Candidate,
**I want** a chatbot that answers common questions (application status, next steps, company info),
**so that** I can get immediate answers without waiting for recruiter response.

**Reason Deferred**: Advanced AI feature; deprioritized in favor of core screening AI

---

**US-082** [P3] Video Interview Integration

**As a** Recruiter,
**I want** built-in video interview recording and playback,
**so that** hiring managers can review interviews asynchronously.

**Reason Deferred**: Calendar integration with Zoom/Teams sufficient for MVP; native video adds complexity

---

## Story Map

### Release 1: MVP (Must Have - P0)

**Epic 1: Job Management**

- US-001: Post Job to Career Site
- US-003: Use Job Templates
- US-002: Distribute Job to Multiple Channels

**Epic 2: Candidate Screening**

- US-010: Parse Resumes Automatically
- US-011: Score Candidates with AI Matching
- US-012: Filter and Search Candidates
- US-013: Move Candidates Through Pipeline
- US-014: Reject Candidates with Compliance

**Epic 4: Candidate Communication**

- US-030: Send Automated Status Emails
- US-031: Use Email Templates

**MVP Delivers**: Core ATS workflow - post jobs, screen candidates with AI, manage pipeline, communicate professionally

---

### Release 2: Enhanced Product (Should Have - P1)

**Epic 3: Interview Management**

- US-020: Schedule Interviews with Calendar Integration
- US-021: Create Interview Kits
- US-022: Collect Structured Interview Feedback
- US-023: Receive Feedback Reminders
- US-024: Flag Conflicting Interview Feedback

**Epic 5: Analytics & Reporting**

- US-040: View Recruitment Dashboard
- US-041: Generate Custom Reports

**Release 2 Delivers**: Data-driven decision making, structured interviews, process insights

---

### Release 3: Full Product (Could Have - P2)

**Epic 6: Talent Pool Management**

- US-050: Search Past Candidates
- US-051: Save and Reuse Talent Searches

**Epic 7: Offer Management**

- US-060: Generate Offer Letters
- US-061: Track Offer Acceptance

**Epic 8: Employee Referrals**

- US-070: Submit Employee Referrals
- US-071: Track Referral Status and Bonuses

**Release 3 Delivers**: Cost optimization (talent pool reuse, referrals), offer automation

---

## Story Statistics

| Priority | Count | Total Story Points |
|----------|-------|--------------------|
| P0 (Must Have) | 11 | 96 |
| P1 (Should Have) | 7 | 68 |
| P2 (Could Have) | 6 | 47 |
| P3 (Won't Have) | 3 | Not estimated |
| **Total** | **27** | **211** |

**Estimated MVP Velocity**: Assuming 2-week sprints and team velocity of 30-40 story points per sprint, MVP delivery would take approximately 3-4 sprints (6-8 weeks).

---

## Next Steps

1. **Backlog Refinement**: Review these user stories with stakeholders (recruiters, hiring managers, HR leaders) to validate priorities and acceptance criteria

2. **Story Breakdown**: Break down large stories (13+ points) into smaller, deliverable increments for sprint planning

3. **Acceptance Criteria Expansion**: Work with QA to expand acceptance criteria with edge cases and error scenarios

4. **UI/UX Mockups**: Create wireframes for high-priority stories (US-001, US-011, US-013, US-022) to validate user flows

5. **Technical Specifications**: For each epic, create detailed use cases and technical specifications using `/speckit.specify` command

6. **Sprint Planning**: Organize Release 1 stories into 2-week sprints based on dependencies and team capacity

---

**Document Metadata**

- **Version**: 1.0
- **Last Updated**: 2025-12-02
- **Owner**: Product Team
- **Status**: Draft - Pending Stakeholder Review
