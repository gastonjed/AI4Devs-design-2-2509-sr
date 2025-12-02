# Tasks: LTI ATS - Applicant Tracking System

**Input**: User stories from USER-STORIES.md
**Prerequisites**: spec.md, plan.md (to be created), data-model.md (to be created), contracts/ (to be created)

**Tests**: Tests are OPTIONAL in this implementation - only included if explicitly requested during planning phase.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story. Each P0 story represents an independently deployable increment.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US-001, US-002, etc.)
- Include exact file paths in descriptions

## Path Conventions

Based on project needs (to be determined in plan.md):
- **Assumed structure**: Backend API + Frontend Web App
- **Backend**: `backend/src/`, `backend/tests/`
- **Frontend**: `frontend/src/`, `frontend/tests/`
- Paths shown below assume this structure - adjust based on final plan.md

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Initialize backend project with Python/FastAPI (or chosen stack) in backend/
- [ ] T002 Initialize frontend project with React/Next.js (or chosen stack) in frontend/
- [ ] T003 [P] Configure database (PostgreSQL) with connection pooling
- [ ] T004 [P] Setup authentication framework (JWT tokens) in backend/src/auth/
- [ ] T005 [P] Configure CORS and API middleware in backend/src/middleware/
- [ ] T006 [P] Setup environment configuration management (.env, config files)
- [ ] T007 [P] Configure linting and formatting tools (ESLint, Prettier, Black)
- [ ] T008 [P] Setup CI/CD pipeline configuration (GitHub Actions or similar)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T009 Create base database schema and migrations framework (Alembic/TypeORM)
- [ ] T010 [P] Implement User model with roles (Recruiter, Hiring Manager, Interviewer) in backend/src/models/user.py
- [ ] T011 [P] Implement Company/Organization model in backend/src/models/company.py
- [ ] T012 [P] Setup authentication endpoints (login, logout, refresh token) in backend/src/api/auth.py
- [ ] T013 [P] Implement RBAC (Role-Based Access Control) middleware in backend/src/middleware/rbac.py
- [ ] T014 [P] Create base API response structures and error handling in backend/src/utils/responses.py
- [ ] T015 [P] Setup logging infrastructure with structured logging in backend/src/utils/logger.py
- [ ] T016 [P] Implement audit trail tracking system in backend/src/models/audit_log.py
- [ ] T017 [P] Setup frontend authentication context and protected routes in frontend/src/contexts/AuthContext.tsx
- [ ] T018 [P] Create base UI components library (buttons, forms, modals) in frontend/src/components/ui/
- [ ] T019 Setup API client with interceptors and error handling in frontend/src/services/api.ts

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: Epic 1 - User Story US-001 (P0) 🎯 MVP Core

**Story**: Post Job to Career Site
**Goal**: Recruiters can create and publish job postings to the company career site
**Independent Test**: Create a job, verify it appears on career site, apply to job, verify application is received

### Implementation for US-001

- [ ] T020 [P] [US-001] Create Job model with fields (title, description, location, requirements, status) in backend/src/models/job.py
- [ ] T021 [P] [US-001] Create JobApplication model in backend/src/models/application.py
- [ ] T022 [US-001] Implement JobService with CRUD operations in backend/src/services/job_service.py (depends on T020)
- [ ] T023 [US-001] Create POST /api/jobs endpoint for job creation in backend/src/api/jobs.py
- [ ] T024 [US-001] Create GET /api/jobs endpoint with pagination and filters in backend/src/api/jobs.py
- [ ] T025 [US-001] Create PUT /api/jobs/{id}/publish endpoint in backend/src/api/jobs.py
- [ ] T026 [P] [US-001] Create job creation form UI in frontend/src/pages/jobs/create.tsx
- [ ] T027 [P] [US-001] Create job listing dashboard for recruiters in frontend/src/pages/jobs/index.tsx
- [ ] T028 [P] [US-001] Create public career site job board in frontend/src/pages/careers/index.tsx
- [ ] T029 [P] [US-001] Create job application form for candidates in frontend/src/pages/careers/apply.tsx
- [ ] T030 [US-001] Add job view tracking and analytics in backend/src/services/analytics_service.py
- [ ] T031 [US-001] Add validation for required job fields in backend/src/validators/job_validator.py

**Checkpoint**: US-001 complete - recruiters can post jobs and candidates can apply

---

## Phase 4: Epic 1 - User Story US-003 (P0)

**Story**: Use Job Templates
**Goal**: Recruiters can create jobs from pre-built templates to save time
**Independent Test**: Create a template, use template to create new job, verify fields are pre-filled

### Implementation for US-003

- [ ] T032 [P] [US-003] Create JobTemplate model in backend/src/models/job_template.py
- [ ] T033 [US-003] Implement JobTemplateService in backend/src/services/job_template_service.py (depends on T032)
- [ ] T034 [US-003] Create GET /api/job-templates endpoint in backend/src/api/job_templates.py
- [ ] T035 [US-003] Create POST /api/job-templates endpoint to save template in backend/src/api/job_templates.py
- [ ] T036 [US-003] Create POST /api/jobs/from-template endpoint in backend/src/api/jobs.py
- [ ] T037 [P] [US-003] Create template selector UI in job creation flow in frontend/src/components/jobs/TemplateSelector.tsx
- [ ] T038 [P] [US-003] Create template management page in frontend/src/pages/templates/index.tsx
- [ ] T039 [US-003] Add "Save as Template" button to job creation form in frontend/src/pages/jobs/create.tsx
- [ ] T040 [US-003] Seed default templates (Senior Software Engineer, Product Manager, etc.) in backend/src/db/seeds/job_templates.py

**Checkpoint**: US-003 complete - recruiters can create and use job templates

---

## Phase 5: Epic 1 - User Story US-002 (P0)

**Story**: Distribute Job to Multiple Channels
**Goal**: Automatically post jobs to LinkedIn, Indeed, and other job boards
**Independent Test**: Create job, select multiple channels, verify job appears on selected channels with tracking URLs

### Implementation for US-002

- [ ] T041 [P] [US-002] Create JobDistribution model to track channel postings in backend/src/models/job_distribution.py
- [ ] T042 [P] [US-002] Implement LinkedIn API integration in backend/src/integrations/linkedin.py
- [ ] T043 [P] [US-002] Implement Indeed API integration in backend/src/integrations/indeed.py
- [ ] T044 [US-002] Create DistributionService with channel management in backend/src/services/distribution_service.py
- [ ] T045 [US-002] Create POST /api/jobs/{id}/distribute endpoint in backend/src/api/jobs.py
- [ ] T046 [US-002] Create GET /api/jobs/{id}/distribution-status endpoint in backend/src/api/jobs.py
- [ ] T047 [US-002] Implement webhook handlers for channel status updates in backend/src/api/webhooks.py
- [ ] T048 [US-002] Add tracking URL generation with UTM parameters in backend/src/utils/tracking.py
- [ ] T049 [P] [US-002] Create channel selection UI in job creation/edit form in frontend/src/components/jobs/ChannelSelector.tsx
- [ ] T050 [P] [US-002] Create distribution status dashboard in frontend/src/pages/jobs/[id]/distribution.tsx
- [ ] T051 [US-002] Add error handling and retry logic for failed distributions in backend/src/services/distribution_service.py
- [ ] T052 [US-002] Create admin page for channel configuration in frontend/src/pages/admin/channels.tsx

**Checkpoint**: US-002 complete - jobs can be distributed to multiple channels with tracking

---

## Phase 6: Epic 2 - User Story US-010 (P0)

**Story**: Parse Resumes Automatically
**Goal**: Automatically extract candidate data from uploaded resumes
**Independent Test**: Upload resume, verify extracted fields appear in candidate profile

### Implementation for US-010

- [ ] T053 [P] [US-010] Create Candidate model with parsed fields in backend/src/models/candidate.py
- [ ] T054 [P] [US-010] Create Resume model to store file metadata in backend/src/models/resume.py
- [ ] T055 [US-010] Integrate resume parsing service (Textkernel, Sovren, or open-source) in backend/src/integrations/resume_parser.py
- [ ] T056 [US-010] Implement ResumeParsingService with extraction logic in backend/src/services/resume_parsing_service.py
- [ ] T057 [US-010] Create POST /api/resumes/parse endpoint in backend/src/api/resumes.py
- [ ] T058 [US-010] Implement file upload handling (S3/cloud storage) in backend/src/utils/file_storage.py
- [ ] T059 [US-010] Create confidence scoring for parsed fields in backend/src/services/resume_parsing_service.py
- [ ] T060 [P] [US-010] Create resume upload component with drag-and-drop in frontend/src/components/candidates/ResumeUpload.tsx
- [ ] T061 [P] [US-010] Create candidate profile view with parsed data in frontend/src/pages/candidates/[id]/profile.tsx
- [ ] T062 [US-010] Add manual review UI for low-confidence fields in frontend/src/components/candidates/FieldReview.tsx
- [ ] T063 [US-010] Add resume parsing to application flow in frontend/src/pages/careers/apply.tsx

**Checkpoint**: US-010 complete - resumes are automatically parsed on upload

---

## Phase 7: Epic 2 - User Story US-011 (P0)

**Story**: Score Candidates with AI Matching
**Goal**: Candidates are automatically ranked by AI fit score (0-100)
**Independent Test**: Create job with requirements, add candidates, verify AI scores are calculated and candidates are sorted

### Implementation for US-011

- [ ] T064 [P] [US-011] Create CandidateScore model to store AI match scores in backend/src/models/candidate_score.py
- [ ] T065 [US-011] Integrate AI/ML service (OpenAI, custom model, or embedding service) in backend/src/integrations/ai_service.py
- [ ] T066 [US-011] Implement AIMatchingService with scoring algorithm in backend/src/services/ai_matching_service.py
- [ ] T067 [US-011] Create async job for batch candidate scoring in backend/src/workers/candidate_scoring_worker.py
- [ ] T068 [US-011] Add scoring trigger on new application in backend/src/services/job_service.py
- [ ] T069 [US-011] Create GET /api/jobs/{id}/candidates with score sorting in backend/src/api/jobs.py
- [ ] T070 [US-011] Implement criteria matching logic (skills, experience, location) in backend/src/services/ai_matching_service.py
- [ ] T071 [P] [US-011] Create candidate pipeline view with score display in frontend/src/pages/jobs/[id]/candidates.tsx
- [ ] T072 [P] [US-011] Create match score visualization component in frontend/src/components/candidates/MatchScore.tsx
- [ ] T073 [P] [US-011] Create criteria checklist component (✅ met, ⚠️ gaps) in frontend/src/components/candidates/CriteriaChecklist.tsx
- [ ] T074 [US-011] Add score recalculation when job requirements change in backend/src/services/ai_matching_service.py

**Checkpoint**: US-011 complete - candidates are scored and ranked by AI

---

## Phase 8: Epic 2 - User Story US-012 (P0)

**Story**: Filter and Search Candidates
**Goal**: Recruiters can filter candidates by skills, experience, location, and other criteria
**Independent Test**: Apply multiple filters, verify only matching candidates are displayed

### Implementation for US-012

- [ ] T075 [P] [US-012] Create SavedSearch model in backend/src/models/saved_search.py
- [ ] T076 [US-012] Implement CandidateSearchService with filter logic in backend/src/services/candidate_search_service.py
- [ ] T077 [US-012] Create GET /api/candidates/search endpoint with query parameters in backend/src/api/candidates.py
- [ ] T078 [US-012] Implement Boolean search parser (AND, OR, NOT) in backend/src/utils/search_parser.py
- [ ] T079 [US-012] Add Elasticsearch/search index for fast queries in backend/src/services/search_index_service.py
- [ ] T080 [US-012] Create POST /api/searches endpoint to save searches in backend/src/api/searches.py
- [ ] T081 [P] [US-012] Create advanced filter panel UI in frontend/src/components/candidates/FilterPanel.tsx
- [ ] T082 [P] [US-012] Create Boolean search input with syntax highlighting in frontend/src/components/candidates/BooleanSearch.tsx
- [ ] T083 [P] [US-012] Create saved searches manager in frontend/src/pages/searches/index.tsx
- [ ] T084 [US-012] Add filter persistence in URL query parameters in frontend/src/pages/jobs/[id]/candidates.tsx

**Checkpoint**: US-012 complete - candidates can be filtered and searched with complex queries

---

## Phase 9: Epic 2 - User Story US-013 (P0)

**Story**: Move Candidates Through Pipeline
**Goal**: Drag and drop candidates between pipeline stages
**Independent Test**: Move candidate from "Applied" to "Phone Screen", verify status updates and notification sent

### Implementation for US-013

- [ ] T085 [P] [US-013] Create PipelineStage model in backend/src/models/pipeline_stage.py
- [ ] T086 [P] [US-013] Create CandidateStageHistory model for audit trail in backend/src/models/candidate_stage_history.py
- [ ] T087 [US-013] Implement PipelineService with stage management in backend/src/services/pipeline_service.py
- [ ] T088 [US-013] Create PATCH /api/candidates/{id}/stage endpoint in backend/src/api/candidates.py
- [ ] T089 [US-013] Create POST /api/candidates/bulk-stage-update endpoint in backend/src/api/candidates.py
- [ ] T090 [US-013] Add automatic notification trigger on stage change in backend/src/services/pipeline_service.py
- [ ] T091 [US-013] Implement backward movement validation and logging in backend/src/services/pipeline_service.py
- [ ] T092 [P] [US-013] Create drag-and-drop pipeline board UI (Kanban style) in frontend/src/pages/jobs/[id]/pipeline.tsx
- [ ] T093 [P] [US-013] Create bulk action toolbar in frontend/src/components/candidates/BulkActions.tsx
- [ ] T094 [US-013] Add stage change confirmation modal in frontend/src/components/candidates/StageChangeModal.tsx
- [ ] T095 [US-013] Add optimistic UI updates for drag operations in frontend/src/pages/jobs/[id]/pipeline.tsx

**Checkpoint**: US-013 complete - candidates can be moved through pipeline stages

---

## Phase 10: Epic 2 - User Story US-014 (P0)

**Story**: Reject Candidates with Compliance
**Goal**: Reject candidates with required rejection reason for EEOC compliance
**Independent Test**: Attempt to reject without reason (should fail), reject with reason, verify audit log and email sent

### Implementation for US-014

- [ ] T096 [P] [US-014] Create RejectionReason lookup table in backend/src/models/rejection_reason.py
- [ ] T097 [US-014] Add rejection tracking to CandidateStageHistory in backend/src/models/candidate_stage_history.py
- [ ] T098 [US-014] Create POST /api/candidates/{id}/reject endpoint with required reason in backend/src/api/candidates.py
- [ ] T099 [US-014] Implement rejection validation middleware in backend/src/middleware/rejection_validator.py
- [ ] T100 [US-014] Add rejection reason to audit log in backend/src/services/pipeline_service.py
- [ ] T101 [US-014] Trigger automated rejection email in backend/src/services/email_service.py
- [ ] T102 [P] [US-014] Create rejection modal with reason dropdown in frontend/src/components/candidates/RejectModal.tsx
- [ ] T103 [P] [US-014] Create rejection audit report in frontend/src/pages/reports/rejections.tsx
- [ ] T104 [US-014] Seed standard EEOC-compliant rejection reasons in backend/src/db/seeds/rejection_reasons.py

**Checkpoint**: US-014 complete - candidates can be rejected with compliance tracking

---

## Phase 11: Epic 4 - User Story US-030 (P0)

**Story**: Send Automated Status Emails
**Goal**: Candidates receive automatic email updates when status changes
**Independent Test**: Change candidate status, verify email is received within 5 minutes with personalized content

### Implementation for US-030

- [ ] T105 [P] [US-030] Create EmailTemplate model in backend/src/models/email_template.py
- [ ] T106 [P] [US-030] Create EmailLog model for tracking sent emails in backend/src/models/email_log.py
- [ ] T107 [US-030] Implement EmailService with template rendering in backend/src/services/email_service.py
- [ ] T108 [US-030] Integrate email provider (SendGrid, AWS SES, Mailgun) in backend/src/integrations/email_provider.py
- [ ] T109 [US-030] Create async email queue worker in backend/src/workers/email_worker.py
- [ ] T110 [US-030] Add email trigger on stage change events in backend/src/services/pipeline_service.py
- [ ] T111 [US-030] Implement template variable substitution (name, job title, next steps) in backend/src/services/email_service.py
- [ ] T112 [P] [US-030] Create email template editor UI in frontend/src/pages/admin/email-templates.tsx
- [ ] T113 [P] [US-030] Create email log viewer in frontend/src/pages/candidates/[id]/emails.tsx
- [ ] T114 [US-030] Seed default email templates for each stage in backend/src/db/seeds/email_templates.py
- [ ] T115 [US-030] Add email delivery status tracking and retry logic in backend/src/workers/email_worker.py

**Checkpoint**: US-030 complete - automated emails are sent on status changes

---

## Phase 12: Epic 4 - User Story US-031 (P0)

**Story**: Use Email Templates
**Goal**: Recruiters can send emails using pre-built templates
**Independent Test**: Select a template, verify placeholders are auto-filled, send email, verify candidate receives it

### Implementation for US-031

- [ ] T116 [US-031] Extend EmailService with manual template selection in backend/src/services/email_service.py
- [ ] T117 [US-031] Create POST /api/candidates/{id}/send-email endpoint in backend/src/api/candidates.py
- [ ] T118 [US-031] Create GET /api/email-templates endpoint with filtering in backend/src/api/email_templates.py
- [ ] T119 [P] [US-031] Create email composer modal with template selector in frontend/src/components/candidates/EmailComposer.tsx
- [ ] T120 [P] [US-031] Add template preview with filled placeholders in frontend/src/components/candidates/EmailComposer.tsx
- [ ] T121 [US-031] Add email action buttons to candidate profile in frontend/src/pages/candidates/[id]/profile.tsx
- [ ] T122 [US-031] Implement template categorization (by stage, by action) in backend/src/models/email_template.py

**Checkpoint**: US-031 complete - MVP RELEASE READY 🎉 All P0 stories complete

---

## Phase 13: Epic 3 - User Story US-020 (P1)

**Story**: Schedule Interviews with Calendar Integration
**Goal**: Schedule interviews that sync with Google Calendar and Outlook
**Independent Test**: Schedule interview, verify calendar invites sent to all participants, check availability view

### Implementation for US-020

- [ ] T123 [P] [US-020] Create Interview model in backend/src/models/interview.py
- [ ] T124 [P] [US-020] Create InterviewParticipant model in backend/src/models/interview_participant.py
- [ ] T125 [US-020] Integrate Google Calendar API in backend/src/integrations/google_calendar.py
- [ ] T126 [US-020] Integrate Microsoft Outlook API in backend/src/integrations/outlook_calendar.py
- [ ] T127 [US-020] Implement InterviewService with scheduling logic in backend/src/services/interview_service.py
- [ ] T128 [US-020] Create POST /api/interviews endpoint in backend/src/api/interviews.py
- [ ] T129 [US-020] Create GET /api/interviewers/availability endpoint in backend/src/api/interviews.py
- [ ] T130 [US-020] Implement calendar invite generation in backend/src/services/calendar_service.py
- [ ] T131 [P] [US-020] Create interview scheduling UI with date/time picker in frontend/src/pages/candidates/[id]/schedule.tsx
- [ ] T132 [P] [US-020] Create availability checker component in frontend/src/components/interviews/AvailabilityChecker.tsx
- [ ] T133 [P] [US-020] Create interview timeline view in frontend/src/pages/candidates/[id]/interviews.tsx
- [ ] T134 [US-020] Add timezone handling for global teams in backend/src/utils/timezone.py

**Checkpoint**: US-020 complete - interviews can be scheduled with calendar integration

---

## Phase 14: Epic 3 - User Story US-021 (P1)

**Story**: Create Interview Kits
**Goal**: Prepare interview kits with assigned questions per interviewer
**Independent Test**: Create interview kit, assign questions to interviewers, verify interviewers can access their assigned questions

### Implementation for US-021

- [ ] T135 [P] [US-021] Create InterviewKit model in backend/src/models/interview_kit.py
- [ ] T136 [P] [US-021] Create InterviewQuestion model in backend/src/models/interview_question.py
- [ ] T137 [P] [US-021] Create QuestionAssignment model in backend/src/models/question_assignment.py
- [ ] T138 [US-021] Implement InterviewKitService in backend/src/services/interview_kit_service.py
- [ ] T139 [US-021] Create POST /api/interviews/{id}/kit endpoint in backend/src/api/interviews.py
- [ ] T140 [US-021] Create GET /api/interviews/{id}/kit endpoint for interviewers in backend/src/api/interviews.py
- [ ] T141 [US-021] Add document attachment support (resume, AI report) in backend/src/models/interview_kit.py
- [ ] T142 [P] [US-021] Create interview kit builder UI in frontend/src/pages/interviews/[id]/kit.tsx
- [ ] T143 [P] [US-021] Create question assignment interface in frontend/src/components/interviews/QuestionAssigner.tsx
- [ ] T144 [P] [US-021] Create interviewer kit view in frontend/src/pages/interviews/[id]/my-kit.tsx
- [ ] T145 [US-021] Seed default interview question library in backend/src/db/seeds/interview_questions.py

**Checkpoint**: US-021 complete - interview kits can be created and assigned

---

## Phase 15: Epic 3 - User Story US-022 (P1)

**Story**: Collect Structured Interview Feedback
**Goal**: Interviewers submit feedback using structured scorecards
**Independent Test**: Submit feedback with ratings, verify validation for justification on extreme ratings, view aggregated feedback

### Implementation for US-022

- [ ] T146 [P] [US-022] Create InterviewFeedback model in backend/src/models/interview_feedback.py
- [ ] T147 [P] [US-022] Create FeedbackCriteria model (Technical Skills, Communication, etc.) in backend/src/models/feedback_criteria.py
- [ ] T148 [P] [US-022] Create FeedbackRating model (1-5 scale with justification) in backend/src/models/feedback_rating.py
- [ ] T149 [US-022] Implement FeedbackService with aggregation logic in backend/src/services/feedback_service.py
- [ ] T150 [US-022] Create POST /api/interviews/{id}/feedback endpoint in backend/src/api/interviews.py
- [ ] T151 [US-022] Create GET /api/interviews/{id}/feedback endpoint with aggregation in backend/src/api/interviews.py
- [ ] T152 [US-022] Add validation for justification on ratings <3 or >4 in backend/src/validators/feedback_validator.py
- [ ] T153 [P] [US-022] Create feedback scorecard form in frontend/src/pages/interviews/[id]/feedback.tsx
- [ ] T154 [P] [US-022] Create aggregated feedback view for hiring managers in frontend/src/pages/candidates/[id]/feedback-summary.tsx
- [ ] T155 [US-022] Add consensus/conflict highlighting in frontend/src/components/interviews/FeedbackAnalysis.tsx
- [ ] T156 [US-022] Seed default feedback criteria in backend/src/db/seeds/feedback_criteria.py

**Checkpoint**: US-022 complete - structured interview feedback can be collected

---

## Phase 16: Epic 3 - User Story US-023 (P1)

**Story**: Receive Feedback Reminders
**Goal**: Interviewers receive reminders if feedback not submitted within 12 hours
**Independent Test**: Complete interview, wait 12 hours without submitting feedback, verify reminder email received

### Implementation for US-023

- [ ] T157 [US-023] Create scheduled job for feedback reminder checks in backend/src/workers/feedback_reminder_worker.py
- [ ] T158 [US-023] Implement reminder logic in FeedbackService in backend/src/services/feedback_service.py
- [ ] T159 [US-023] Create feedback reminder email template in backend/src/db/seeds/email_templates.py
- [ ] T160 [US-023] Add escalation notification to hiring manager after 24 hours in backend/src/services/feedback_service.py
- [ ] T161 [US-023] Create GET /api/my/pending-feedback endpoint in backend/src/api/interviews.py
- [ ] T162 [P] [US-023] Create pending feedback dashboard for interviewers in frontend/src/pages/my/pending-feedback.tsx
- [ ] T163 [US-023] Add notification indicator in app header in frontend/src/components/layout/Header.tsx

**Checkpoint**: US-023 complete - feedback reminders are automated

---

## Phase 17: Epic 3 - User Story US-024 (P1)

**Story**: Flag Conflicting Interview Feedback
**Goal**: System automatically flags conflicting feedback for debrief
**Independent Test**: Submit conflicting feedback (2 Strong Yes, 2 No), verify conflict badge and suggested actions appear

### Implementation for US-024

- [ ] T164 [US-024] Implement conflict detection algorithm in FeedbackService in backend/src/services/feedback_service.py
- [ ] T165 [US-024] Create ConflictResolution model to track debrief outcomes in backend/src/models/conflict_resolution.py
- [ ] T166 [US-024] Add conflict flag calculation on feedback submission in backend/src/services/feedback_service.py
- [ ] T167 [US-024] Create POST /api/candidates/{id}/resolve-conflict endpoint in backend/src/api/candidates.py
- [ ] T168 [P] [US-024] Create conflict badge in candidate profile in frontend/src/components/candidates/ConflictBadge.tsx
- [ ] T169 [P] [US-024] Create conflict resolution modal with suggested actions in frontend/src/components/candidates/ConflictResolutionModal.tsx
- [ ] T170 [US-024] Add conflict logging to audit trail in backend/src/services/feedback_service.py

**Checkpoint**: US-024 complete - conflicting feedback is automatically flagged

---

## Phase 18: Epic 5 - User Story US-040 (P1)

**Story**: View Recruitment Dashboard
**Goal**: Real-time dashboard with key recruitment metrics
**Independent Test**: Navigate to dashboard, verify metrics display (open jobs, candidates, time-to-hire), drill down into details

### Implementation for US-040

- [ ] T171 [P] [US-040] Create AnalyticsMetrics model for cached aggregations in backend/src/models/analytics_metrics.py
- [ ] T172 [US-040] Implement AnalyticsService with metric calculations in backend/src/services/analytics_service.py
- [ ] T173 [US-040] Create GET /api/analytics/dashboard endpoint in backend/src/api/analytics.py
- [ ] T174 [US-040] Create GET /api/analytics/time-to-hire endpoint with drill-down in backend/src/api/analytics.py
- [ ] T175 [US-040] Create GET /api/analytics/source-effectiveness endpoint in backend/src/api/analytics.py
- [ ] T176 [US-040] Create GET /api/analytics/pipeline-conversion endpoint in backend/src/api/analytics.py
- [ ] T177 [US-040] Implement metric caching with refresh strategy in backend/src/services/analytics_service.py
- [ ] T178 [P] [US-040] Create dashboard page with metric cards in frontend/src/pages/dashboard/index.tsx
- [ ] T179 [P] [US-040] Create drill-down views for each metric in frontend/src/pages/dashboard/[metric].tsx
- [ ] T180 [US-040] Add real-time data refresh (3s requirement) in frontend/src/pages/dashboard/index.tsx
- [ ] T181 [P] [US-040] Create chart components (time-to-hire, source breakdown) in frontend/src/components/analytics/Charts.tsx

**Checkpoint**: US-040 complete - recruitment dashboard is functional

---

## Phase 19: Epic 5 - User Story US-041 (P1)

**Story**: Generate Custom Reports
**Goal**: Create custom reports with selected metrics and date ranges
**Independent Test**: Build custom report, save it, schedule for automatic delivery, download in multiple formats

### Implementation for US-041

- [ ] T182 [P] [US-041] Create Report model in backend/src/models/report.py
- [ ] T183 [P] [US-041] Create ScheduledReport model in backend/src/models/scheduled_report.py
- [ ] T184 [US-041] Implement ReportService with builder logic in backend/src/services/report_service.py
- [ ] T185 [US-041] Create POST /api/reports endpoint in backend/src/api/reports.py
- [ ] T186 [US-041] Create POST /api/reports/{id}/schedule endpoint in backend/src/api/reports.py
- [ ] T187 [US-041] Create GET /api/reports/{id}/export endpoint with format parameter in backend/src/api/reports.py
- [ ] T188 [US-041] Implement PDF export using ReportLab or similar in backend/src/utils/report_exporter.py
- [ ] T189 [US-041] Implement Excel/CSV export in backend/src/utils/report_exporter.py
- [ ] T190 [US-041] Create scheduled report worker in backend/src/workers/scheduled_report_worker.py
- [ ] T191 [P] [US-041] Create report builder UI in frontend/src/pages/reports/builder.tsx
- [ ] T192 [P] [US-041] Create saved reports list in frontend/src/pages/reports/index.tsx
- [ ] T193 [US-041] Add report scheduling interface in frontend/src/components/reports/ScheduleModal.tsx

**Checkpoint**: US-041 complete - custom reports can be created and scheduled

---

## Phase 20: Epic 6 - User Story US-050 (P2)

**Story**: Search Past Candidates
**Goal**: Search entire candidate database including past applicants
**Independent Test**: Search for candidates with specific criteria, verify past candidates appear with history

### Implementation for US-050

- [ ] T194 [US-050] Extend CandidateSearchService for talent pool queries in backend/src/services/candidate_search_service.py
- [ ] T195 [US-050] Create GET /api/talent-pool/search endpoint in backend/src/api/talent_pool.py
- [ ] T196 [US-050] Implement "silver medalist" ranking boost in backend/src/services/candidate_search_service.py
- [ ] T197 [P] [US-050] Create talent pool search page in frontend/src/pages/talent-pool/search.tsx
- [ ] T198 [P] [US-050] Enhance candidate profile to show application history in frontend/src/pages/candidates/[id]/history.tsx
- [ ] T199 [US-050] Add past feedback and rejection reasons to profile view in frontend/src/pages/candidates/[id]/profile.tsx

**Checkpoint**: US-050 complete - past candidates can be searched

---

## Phase 21: Epic 6 - User Story US-051 (P2)

**Story**: Save and Reuse Talent Searches
**Goal**: Save complex searches for reuse
**Independent Test**: Create complex search, save it, rerun saved search, verify "New since last run" indicator

### Implementation for US-051

- [ ] T200 [US-051] Extend SavedSearch model with last_run_at timestamp in backend/src/models/saved_search.py
- [ ] T201 [US-051] Create GET /api/talent-pool/saved-searches endpoint in backend/src/api/talent_pool.py
- [ ] T202 [US-051] Create POST /api/talent-pool/saved-searches/{id}/run endpoint in backend/src/api/talent_pool.py
- [ ] T203 [US-051] Implement new candidate alerts for saved searches in backend/src/workers/talent_pool_alert_worker.py
- [ ] T204 [P] [US-051] Create saved search manager in frontend/src/pages/talent-pool/saved-searches.tsx
- [ ] T205 [US-051] Add "New since last run" badge in search results in frontend/src/components/talent-pool/SearchResults.tsx

**Checkpoint**: US-051 complete - talent searches can be saved and reused

---

## Phase 22: Epic 7 - User Story US-060 (P2)

**Story**: Generate Offer Letters
**Goal**: Create offer letters from templates with auto-filled details
**Independent Test**: Create offer for candidate, preview PDF, send to candidate, verify secure access

### Implementation for US-060

- [ ] T206 [P] [US-060] Create Offer model in backend/src/models/offer.py
- [ ] T207 [P] [US-060] Create OfferTemplate model in backend/src/models/offer_template.py
- [ ] T208 [US-060] Implement OfferService with template rendering in backend/src/services/offer_service.py
- [ ] T209 [US-060] Create POST /api/offers endpoint in backend/src/api/offers.py
- [ ] T210 [US-060] Create GET /api/offers/{id}/preview endpoint (PDF generation) in backend/src/api/offers.py
- [ ] T211 [US-060] Create POST /api/offers/{id}/send endpoint in backend/src/api/offers.py
- [ ] T212 [US-060] Implement secure access link generation in backend/src/utils/secure_links.py
- [ ] T213 [P] [US-060] Create offer creation form in frontend/src/pages/candidates/[id]/create-offer.tsx
- [ ] T214 [P] [US-060] Create offer preview modal in frontend/src/components/offers/OfferPreview.tsx
- [ ] T215 [US-060] Seed default offer templates (full-time, contract, intern) in backend/src/db/seeds/offer_templates.py

**Checkpoint**: US-060 complete - offer letters can be generated

---

## Phase 23: Epic 7 - User Story US-061 (P2)

**Story**: Track Offer Acceptance
**Goal**: Track offer status (sent, viewed, accepted, declined)
**Independent Test**: Send offer, track status changes, decline offer with reason, verify status updates

### Implementation for US-061

- [ ] T216 [US-061] Add status tracking to Offer model in backend/src/models/offer.py
- [ ] T217 [US-061] Create offer view tracking webhook in backend/src/api/webhooks.py
- [ ] T218 [US-061] Create POST /api/offers/{id}/accept endpoint in backend/src/api/offers.py
- [ ] T219 [US-061] Create POST /api/offers/{id}/decline endpoint in backend/src/api/offers.py
- [ ] T220 [US-061] Add notification on offer status change in backend/src/services/offer_service.py
- [ ] T221 [P] [US-061] Create offer status dashboard in frontend/src/pages/offers/index.tsx
- [ ] T222 [P] [US-061] Create candidate offer response page in frontend/src/pages/offers/[token]/respond.tsx
- [ ] T223 [US-061] Add decline reason logging in frontend/src/components/offers/DeclineModal.tsx

**Checkpoint**: US-061 complete - offer acceptance can be tracked

---

## Phase 24: Epic 8 - User Story US-070 (P2)

**Story**: Submit Employee Referrals
**Goal**: Employees can submit referrals for open jobs
**Independent Test**: Employee submits referral, verify referral is tagged to employee, candidate applies and is linked

### Implementation for US-070

- [ ] T224 [P] [US-070] Create Referral model in backend/src/models/referral.py
- [ ] T225 [US-070] Implement ReferralService in backend/src/services/referral_service.py
- [ ] T226 [US-070] Create POST /api/referrals endpoint in backend/src/api/referrals.py
- [ ] T227 [US-070] Add referral linking on candidate application in backend/src/services/job_service.py
- [ ] T228 [US-070] Create GET /api/jobs with referral button for employees in backend/src/api/jobs.py
- [ ] T229 [P] [US-070] Create referral submission form in frontend/src/pages/referrals/submit.tsx
- [ ] T230 [P] [US-070] Add "Refer a Friend" button to job listings in frontend/src/pages/careers/index.tsx
- [ ] T231 [US-070] Add referral tag to candidate profile in frontend/src/pages/candidates/[id]/profile.tsx

**Checkpoint**: US-070 complete - employees can submit referrals

---

## Phase 25: Epic 8 - User Story US-071 (P2)

**Story**: Track Referral Status and Bonuses
**Goal**: Employees can track referral status and bonus amounts
**Independent Test**: Track referred candidate through pipeline, mark as hired, verify bonus approval with payout date

### Implementation for US-071

- [ ] T232 [US-071] Add bonus tracking to Referral model in backend/src/models/referral.py
- [ ] T233 [US-071] Create ReferralBonus model in backend/src/models/referral_bonus.py
- [ ] T234 [US-071] Implement bonus calculation logic in ReferralService in backend/src/services/referral_service.py
- [ ] T235 [US-071] Create GET /api/referrals/my endpoint for employees in backend/src/api/referrals.py
- [ ] T236 [US-071] Create bonus approval workflow in backend/src/services/referral_service.py
- [ ] T237 [P] [US-071] Create referral dashboard for employees in frontend/src/pages/my/referrals.tsx
- [ ] T238 [US-071] Add bonus amount display to job listings in frontend/src/pages/careers/index.tsx

**Checkpoint**: US-071 complete - referral bonuses can be tracked

---

## Phase 26: Polish & Cross-Cutting Concerns

**Purpose**: Final improvements that affect multiple user stories

- [ ] T239 [P] Add comprehensive API documentation (OpenAPI/Swagger) in backend/docs/
- [ ] T240 [P] Security audit and penetration testing
- [ ] T241 [P] Performance optimization across all endpoints (< 2s target)
- [ ] T242 [P] Accessibility audit (WCAG 2.1 AA compliance) for frontend
- [ ] T243 [P] Mobile responsive design testing for all pages
- [ ] T244 [P] Error tracking integration (Sentry or similar)
- [ ] T245 [P] Setup monitoring and alerting (Datadog, New Relic, or similar)
- [ ] T246 Setup production deployment pipeline
- [ ] T247 Create user documentation and help guides
- [ ] T248 Setup backup and disaster recovery procedures
- [ ] T249 Conduct GDPR/EEOC compliance review
- [ ] T250 Final QA testing across all user stories

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **P0 User Stories (Phases 3-12)**: All depend on Foundational phase completion
  - Epic 1 stories (US-001, US-003, US-002) can proceed in parallel after Foundation
  - US-010 (Resume Parsing) can start in parallel with Epic 1
  - US-011 (AI Scoring) depends on US-010
  - US-012 (Search) depends on US-010 (needs candidate data)
  - US-013 (Pipeline) depends on US-011 (needs scored candidates)
  - US-014 (Reject) depends on US-013 (needs pipeline stages)
  - US-030 (Automated Emails) depends on US-013 (needs stage changes)
  - US-031 (Email Templates) depends on US-030
- **P1 User Stories (Phases 13-19)**: Depend on relevant P0 stories being complete
  - US-020-024 (Interview Management) depend on US-013 (pipeline stages)
  - US-040-041 (Analytics) depend on multiple P0 stories for data collection
- **P2 User Stories (Phases 20-25)**: Depend on P0/P1 stories
  - US-050-051 (Talent Pool) depend on US-010, US-012 (search infrastructure)
  - US-060-061 (Offers) depend on US-013 (pipeline stages)
  - US-070-071 (Referrals) depend on US-001 (job management)
- **Polish (Phase 26)**: Depends on all implemented user stories

### Epic Dependencies Summary

```
Foundation
    ├── Epic 1: Job Management (US-001, US-003, US-002)
    ├── Epic 2: Candidate Screening
    │       US-010 → US-011 → US-012
    │                   ↓
    │              US-013 → US-014
    ├── Epic 4: Communication (US-030 → US-031)
    │       (depends on US-013)
    ├── Epic 3: Interview Management (US-020 → US-021 → US-022 → US-023, US-024)
    │       (depends on US-013)
    ├── Epic 5: Analytics (US-040 → US-041)
    │       (depends on multiple P0 stories)
    ├── Epic 6: Talent Pool (US-050 → US-051)
    │       (depends on US-010, US-012)
    ├── Epic 7: Offers (US-060 → US-061)
    │       (depends on US-013)
    └── Epic 8: Referrals (US-070 → US-071)
            (depends on US-001)
```

### Critical Path for MVP (P0 Only)

```
Phase 1 (Setup) → Phase 2 (Foundation) → Phase 3 (US-001) → Phase 4 (US-003) → Phase 5 (US-002)
                                       ↓
                                    Phase 6 (US-010) → Phase 7 (US-011) → Phase 8 (US-012)
                                                            ↓
                                                Phase 9 (US-013) → Phase 10 (US-014)
                                                        ↓
                                                Phase 11 (US-030) → Phase 12 (US-031)
```

**Total P0 Tasks**: T001-T122 (122 tasks)
**Estimated MVP Duration**: 8-10 weeks with 2-3 developers (based on 96 story points from USER-STORIES.md)

### Parallel Opportunities

#### Within Setup Phase
All tasks T003, T004, T005, T006, T007, T008 can run in parallel (marked [P])

#### Within Foundational Phase
Tasks T010, T011, T012, T013, T014, T015, T016, T017, T018 can run in parallel (marked [P])

#### Across User Stories
Once Foundation is complete:
- **Wave 1 (Independent)**: US-001 (Epic 1) + US-010 (Epic 2) can start in parallel
- **Wave 2**: US-003, US-002 (Epic 1) + US-011 (Epic 2) can proceed in parallel
- **Wave 3**: US-012, US-013 (Epic 2) in sequence
- **Wave 4**: US-014 (Epic 2) + US-030 (Epic 4) in sequence
- **Wave 5**: US-031 (Epic 4) completes MVP

#### Within Each User Story
All tasks marked [P] within a story phase can be parallelized (typically models, views, independent components)

---

## Parallel Example: Foundational Phase

```bash
# All these tasks can run simultaneously after Setup is complete:
Task: "Implement User model with roles in backend/src/models/user.py"
Task: "Implement Company/Organization model in backend/src/models/company.py"
Task: "Setup authentication endpoints in backend/src/api/auth.py"
Task: "Implement RBAC middleware in backend/src/middleware/rbac.py"
Task: "Create base API response structures in backend/src/utils/responses.py"
Task: "Setup logging infrastructure in backend/src/utils/logger.py"
Task: "Implement audit trail tracking in backend/src/models/audit_log.py"
Task: "Setup frontend authentication context in frontend/src/contexts/AuthContext.tsx"
Task: "Create base UI components library in frontend/src/components/ui/"
```

---

## Implementation Strategy

### MVP First (P0 Only - Recommended)

1. **Phase 1-2**: Complete Setup + Foundational (2 weeks)
2. **Phases 3-5**: Epic 1 - Job Management (2 weeks)
   - Delivers: Job posting and distribution
3. **Phases 6-10**: Epic 2 - Candidate Screening (3 weeks)
   - Delivers: Resume parsing, AI scoring, pipeline management
4. **Phases 11-12**: Epic 4 - Communication (1 week)
   - Delivers: Automated candidate emails
5. **Validation**: Test complete MVP workflow (1 week)
6. **Deploy**: Launch MVP to pilot customers

**MVP Delivers**: Complete ATS workflow - post jobs to multiple channels, receive applications, AI-powered screening, visual pipeline management, automated professional communication

### Incremental Delivery (P0 → P1 → P2)

1. **Release 1 (MVP)**: Complete all P0 stories (Phases 1-12) → 8-10 weeks
2. **Release 2**: Add Interview Management + Analytics (Phases 13-19) → 6-8 weeks
3. **Release 3**: Add Talent Pool + Offers + Referrals (Phases 20-25) → 4-6 weeks
4. **Release 4**: Polish and optimization (Phase 26) → 2 weeks

### Parallel Team Strategy

With 3 developers after Foundation is complete:

- **Developer A**: Focus on Epic 1 (Job Management) - US-001, US-003, US-002
- **Developer B**: Focus on Epic 2 Part 1 (Screening) - US-010, US-011, US-012
- **Developer C**: Join A or B, then take Epic 2 Part 2 (Pipeline) - US-013, US-014

Then converge on Epic 4 (Communication) together.

### Prioritization Methodology Used

This backlog uses **MoSCoW prioritization** combined with **dependency-based sequencing**:

1. **Must Have (P0)**: Critical for MVP - 11 stories covering core ATS workflow
   - Job posting and distribution (US-001, US-002, US-003)
   - AI-powered candidate screening (US-010, US-011, US-012)
   - Pipeline management (US-013, US-014)
   - Professional communication (US-030, US-031)

2. **Should Have (P1)**: Important for competitive product - 7 stories
   - Structured interviews (US-020, US-021, US-022, US-023, US-024)
   - Data-driven analytics (US-040, US-041)

3. **Could Have (P2)**: Competitive differentiators - 6 stories
   - Talent pool reuse (US-050, US-051)
   - Offer automation (US-060, US-061)
   - Referral program (US-070, US-071)

4. **Won't Have (P3)**: Future consideration - deferred
   - Native mobile apps (US-080)
   - Candidate chatbot (US-081)
   - Video interview integration (US-082)

**Criteria Applied**:
- **Value to user**: Does this solve a critical pain point?
- **Business impact**: Does this drive user adoption and retention?
- **Dependencies**: What must be built first?
- **Risk reduction**: Does this validate core assumptions early?
- **Complexity**: Can this be delivered in the current phase?

---

## Task Statistics

| Phase | User Story | Priority | Task Count | Parallel Tasks |
|-------|------------|----------|------------|----------------|
| 1 | Setup | N/A | 8 | 6 |
| 2 | Foundation | N/A | 11 | 9 |
| 3 | US-001 | P0 | 12 | 5 |
| 4 | US-003 | P0 | 9 | 3 |
| 5 | US-002 | P0 | 12 | 4 |
| 6 | US-010 | P0 | 11 | 4 |
| 7 | US-011 | P0 | 11 | 4 |
| 8 | US-012 | P0 | 10 | 4 |
| 9 | US-013 | P0 | 11 | 3 |
| 10 | US-014 | P0 | 9 | 3 |
| 11 | US-030 | P0 | 11 | 4 |
| 12 | US-031 | P0 | 7 | 3 |
| **P0 Subtotal** | **11 stories** | **P0** | **122** | **52** |
| 13 | US-020 | P1 | 12 | 4 |
| 14 | US-021 | P1 | 11 | 4 |
| 15 | US-022 | P1 | 11 | 4 |
| 16 | US-023 | P1 | 7 | 2 |
| 17 | US-024 | P1 | 7 | 2 |
| 18 | US-040 | P1 | 11 | 4 |
| 19 | US-041 | P1 | 12 | 3 |
| **P1 Subtotal** | **7 stories** | **P1** | **71** | **23** |
| 20 | US-050 | P2 | 6 | 3 |
| 21 | US-051 | P2 | 6 | 1 |
| 22 | US-060 | P2 | 10 | 3 |
| 23 | US-061 | P2 | 8 | 3 |
| 24 | US-070 | P2 | 8 | 3 |
| 25 | US-071 | P2 | 7 | 1 |
| **P2 Subtotal** | **6 stories** | **P2** | **45** | **14** |
| 26 | Polish | N/A | 12 | 7 |
| **GRAND TOTAL** | **24 stories** | **All** | **250** | **105** |

**Key Insights**:
- **42% of tasks can be parallelized** (105 out of 250)
- **MVP (P0) represents 49% of total work** (122 out of 250 tasks)
- **Foundational phase is critical** - blocks all 24 user stories
- **Each P0 user story is independently testable** after Foundation

---

## Notes

- **[P] marker**: Tasks marked [P] operate on different files or have no dependencies, allowing parallel execution
- **[Story] label**: Maps each task to its user story for traceability and independent validation
- **File paths**: All tasks include specific file paths for clarity (adjust based on final plan.md structure)
- **Independent stories**: Each user story phase is designed to be independently completable and testable
- **Commit strategy**: Commit after each task or logical group of related tasks
- **Checkpoints**: Stop at phase checkpoints to validate story independently before proceeding
- **Validation**: Test each user story against its acceptance criteria from USER-STORIES.md before marking complete

---

## Next Steps

1. **Create spec.md**: Run `/speckit.specify` to create detailed specifications for MVP stories (P0)
2. **Create plan.md**: Run `/speckit.plan` to document technical architecture, data models, and API contracts
3. **Refine estimates**: Review story points (USER-STORIES.md) against task complexity
4. **Sprint planning**: Organize P0 tasks into 2-week sprints based on team capacity
5. **Begin implementation**: Run `/speckit.implement` after plan.md is complete

---

**Document Metadata**

- **Version**: 1.0
- **Generated**: 2025-12-02
- **Total User Stories**: 24 (11 P0, 7 P1, 6 P2)
- **Total Tasks**: 250
- **Estimated MVP Duration**: 8-10 weeks (with 2-3 developers)
- **Status**: Ready for Review and Planning
