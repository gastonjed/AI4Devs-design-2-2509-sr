# Feature Specification: Job Templates

**Feature Branch**: `001-job-templates`
**Created**: 2025-12-02
**Status**: Draft
**Input**: User description: "US-003: Use Job Templates - Recruiters can create jobs from pre-built templates"

## User Scenarios & Testing

### User Story 1 - Create Job from Pre-Built Template (Priority: P1)

A recruiter needs to post a new Senior Software Engineer position. Instead of writing the job description from scratch, they browse the template library, select the "Senior Software Engineer" template, and all fields (title, description, requirements, qualifications, location type) are automatically populated. The recruiter reviews the pre-filled content, makes minor adjustments specific to their team (e.g., updates tech stack from "Python" to "Python/Go"), and publishes the job in under 5 minutes.

**Why this priority**: This is the core value proposition - reducing job creation time from 30-60 minutes to under 5 minutes by leveraging pre-built content. This delivers immediate ROI to recruiters and is the minimum viable feature.

**Independent Test**: Can be fully tested by selecting a template, verifying all fields are pre-filled, making edits, and publishing the job. Delivers standalone value by reducing job creation time even without the ability to save custom templates.

**Acceptance Scenarios**:

1. **Given** I am on the job creation page, **When** I click "Use Template", **Then** I see a list of available templates organized by job category (Engineering, Sales, Marketing, etc.)
2. **Given** I view the template list, **When** I select "Senior Software Engineer" template, **Then** all job fields (title, description, requirements, qualifications, benefits, salary range) are pre-filled with template content
3. **Given** template fields are pre-filled, **When** I edit the "Technical Requirements" section to add "Go experience required", **Then** my changes are saved to the new job draft but the original template remains unchanged
4. **Given** I have edited a templated job, **When** I publish it, **Then** the job is posted with my customized content and the template is still available for future use
5. **Given** no templates exist for my job type, **When** I search for "Chief Data Officer" template, **Then** I see a message "No templates found for this role" with an option to create the job from scratch or request a template

---

### User Story 2 - Save Custom Job as Reusable Template (Priority: P2)

A recruiter has just created a comprehensive job posting for a "DevOps Engineer" role with specific requirements for their company (cloud certifications, on-call rotation details, specific tools). They want to reuse this content for future DevOps openings. After creating the job, they click "Save as Template", provide a template name "DevOps Engineer - Platform Team", optionally add tags ("Engineering", "DevOps", "Cloud"), and the template is saved to their organization's template library for all recruiters to use.

**Why this priority**: Enables organizations to build their own template library based on real job postings, ensuring templates reflect actual company needs and language. Important for customization but not required for initial value delivery.

**Independent Test**: Can be tested independently by creating a job from scratch, clicking "Save as Template", and verifying it appears in the template library for reuse. Works whether or not P1 templates exist.

**Acceptance Scenarios**:

1. **Given** I have created a job posting, **When** I click "Save as Template" button, **Then** I see a dialog prompting for template name, optional description, and category tags
2. **Given** I am saving a template, **When** I enter template name "DevOps Engineer - Platform Team" and select tags "Engineering" and "DevOps", **Then** the template is saved and appears in my template library
3. **Given** I save a template, **When** I navigate to the template library, **Then** I see my custom template listed alongside pre-built templates, with an indicator showing "Created by [My Name]"
4. **Given** I attempt to save a template with a duplicate name, **When** I enter a name that already exists, **Then** I see a warning "Template 'Senior Software Engineer' already exists. Overwrite or choose a different name?"
5. **Given** a template is saved, **When** another recruiter in my organization searches for templates, **Then** they can see and use my custom template

---

### User Story 3 - Manage Template Library (Priority: P3)

A recruiting manager wants to maintain quality control over the organization's template library. They access the template management page, view all templates (both pre-built and custom), edit outdated templates (e.g., update salary ranges for "Product Manager" role), archive templates that are no longer used (e.g., "Flash Developer"), and set certain templates as "organization favorites" that appear at the top of the template selector for all recruiters.

**Why this priority**: Ensures templates remain current and relevant over time, but not essential for initial feature adoption. Organizations can start with default templates and add management capabilities later.

**Independent Test**: Can be tested by accessing template management, editing a template, archiving an unused template, and verifying changes are reflected for all recruiters. Provides governance value independently of P1/P2.

**Acceptance Scenarios**:

1. **Given** I am a recruiting manager, **When** I access "Manage Templates" page, **Then** I see all templates with columns for name, category, last modified date, usage count (how many jobs created from this template), and actions (Edit, Archive, Star as Favorite)
2. **Given** I view a template, **When** I click "Edit" on "Product Manager" template, **Then** I can modify any field and save changes that affect future uses of the template (existing jobs created from this template remain unchanged)
3. **Given** I want to retire an outdated template, **When** I click "Archive" on "Flash Developer" template, **Then** the template is hidden from the main template selector but existing jobs created from it remain unaffected
4. **Given** I want to highlight important templates, **When** I click "Star as Favorite" on "Senior Software Engineer" template, **Then** it appears in the "Favorites" section at the top of the template selector for all recruiters in my organization
5. **Given** I want to track template effectiveness, **When** I view template analytics, **Then** I see metrics for each template: total uses, time saved (vs. creating from scratch), and average time-to-publish for jobs using each template

---

### Edge Cases

- **What happens when a recruiter starts using a template but the template is edited by an admin mid-session?** The recruiter's draft should remain unchanged (they loaded a "snapshot" of the template at selection time). The updated template applies only to future uses.

- **What happens when a template contains fields that are no longer supported in the job creation form?** System should gracefully skip unsupported fields and log a warning for the template owner, suggesting they update the template.

- **How does the system handle a recruiter trying to save a template with no title or description?** System validates that template name is provided (required field) and at least one content field (title, description, or requirements) is populated before allowing save.

- **What happens when an organization has 100+ custom templates?** Template library should support search, filtering by category/tags, and sorting by usage frequency or date modified to help users find relevant templates quickly.

- **How does the system handle template permissions across different departments or business units?** Templates should be organization-wide by default, with option for recruiting managers to mark templates as "Private" (only visible to specific users/groups) or "Public" (available to all).

## Requirements

### Functional Requirements

- **FR-001**: System MUST allow recruiters to browse available job templates organized by job category (Engineering, Sales, Marketing, Operations, Executive, etc.)
- **FR-002**: System MUST pre-fill all job creation fields (title, description, requirements, qualifications, benefits, location type) when a recruiter selects a template
- **FR-003**: System MUST preserve the original template content when a recruiter edits a templated job - changes apply only to the new job draft, not the source template
- **FR-004**: System MUST allow recruiters to save any job posting as a reusable template with a custom name, optional description, and category tags
- **FR-005**: System MUST display saved templates in the template library with metadata: template name, creator name, creation date, last modified date, and usage count
- **FR-006**: System MUST allow recruiting managers to edit existing templates, with changes affecting only future uses (not jobs already created from that template)
- **FR-007**: System MUST allow recruiting managers to archive templates, hiding them from the template selector while preserving historical job data
- **FR-008**: System MUST support template search by name and filtering by category/tags to help users find relevant templates quickly
- **FR-009**: System MUST prevent duplicate template names within the same organization by validating uniqueness on save
- **FR-010**: System MUST include a set of pre-built templates for common roles (minimum 10 templates covering: Software Engineer, Senior Software Engineer, Engineering Manager, Product Manager, Product Designer, Sales Representative, Account Executive, Marketing Manager, Customer Success Manager, Data Analyst)
- **FR-011**: System MUST allow recruiting managers to mark templates as "Favorites", causing them to appear at the top of the template selector for all recruiters
- **FR-012**: System MUST track template usage metrics: total times used, average time saved per use, and job publication rate for templated vs. non-templated jobs
- **FR-013**: System MUST allow templates to be shared organization-wide by default, with optional visibility controls (private to creator, shared with specific groups, or public to all)

### Key Entities

- **Job Template**: Represents a reusable job posting template containing pre-filled content for job fields (title, description, requirements, qualifications, benefits, salary range, location type). Includes metadata: template name, optional description, category tags, creator information, creation/modification timestamps, usage count, favorite status, and visibility settings.

- **Template Category**: Represents a job category or department (e.g., "Engineering", "Sales", "Marketing", "Operations") used to organize templates. Each template can belong to one or more categories via tags.

- **Template Usage Record**: Tracks each instance when a template is used to create a job, including: which template was used, which recruiter used it, timestamp of use, and whether the resulting job was published or abandoned. Used for analytics.

- **Job Posting**: The actual job posting created from a template (or from scratch). Contains a reference to the source template (if created from a template) but remains independent - edits to the job do not affect the template, and vice versa.

## Success Criteria

### Measurable Outcomes

- **SC-001**: Recruiters can create a new job posting from a template in under 5 minutes (vs. 30-60 minutes when writing from scratch), representing an 80%+ time savings
- **SC-002**: At least 70% of new job postings are created using templates within 3 months of feature launch, indicating strong adoption
- **SC-003**: Organizations build a library of at least 5 custom templates within the first month of using the feature, showing they are tailoring templates to their needs
- **SC-004**: Recruiters rate the template feature as "Very Useful" or "Essential" in user satisfaction surveys (target: 85%+ positive rating)
- **SC-005**: Time-to-publish for templated jobs is 50% faster than non-templated jobs, as measured by time from job creation start to job publication
- **SC-006**: Template search and selection takes under 30 seconds for 90% of users, ensuring the template library remains easy to navigate even as it grows
- **SC-007**: Reduce number of job postings with incomplete or poorly formatted descriptions by 40%, as templates ensure consistent quality and completeness

## Assumptions

- **Assumption 1**: Pre-built templates will be created by the LTI ATS product team based on industry-standard job descriptions and maintained as part of the product (not customer-managed)
- **Assumption 2**: Organizations will start with pre-built templates and gradually build custom templates based on their specific needs over time
- **Assumption 3**: Recruiters have permission to create and save templates by default; only recruiting managers can edit pre-built templates or archive templates created by others
- **Assumption 4**: Template content includes all job fields available in the job creation form, but templates with missing fields can still be saved and used (missing fields remain empty when template is applied)
- **Assumption 5**: Template usage analytics will be available only to recruiting managers and admins, not to individual recruiters
- **Assumption 6**: When a recruiter selects a template, they receive a "snapshot" copy of the template at that moment - subsequent edits by admins to the template do not affect in-progress job drafts
- **Assumption 7**: Organizations using the ATS will post jobs for similar roles multiple times (e.g., hiring multiple software engineers per year), making templates valuable for reuse
- **Assumption 8**: Salary ranges in templates are optional and can be left blank, as compensation varies significantly by location and candidate experience

## Dependencies

- **Dependency 1**: Requires US-001 (Post Job to Career Site) to be implemented first, as templates populate the same job creation form and fields
- **Dependency 2**: Job creation form must support all fields that templates will populate (title, description, requirements, qualifications, benefits, salary range, location type)
- **Dependency 3**: User permissions system must distinguish between "Recruiter" (can use and create templates) and "Recruiting Manager" (can edit and archive any template)

## Constitution Check

This specification aligns with the LTI ATS Constitution:

- **Design-First Approach**: Specification defines user scenarios and functional requirements before any implementation, ensuring all stakeholders understand the feature
- **User-Centric Design**: Feature is organized by user persona workflows (Recruiter using templates, saving templates, Manager governing templates), with independent test criteria for each
- **Compliance by Design**: Template visibility controls and audit trail (usage records) support potential future compliance needs
- **Documentation as Code**: This specification is version-controlled and will be maintained alongside implementation
- **Independent User Stories**: Each user story (P1, P2, P3) can be implemented and tested independently, enabling incremental delivery
- **AI-Native Development**: While this feature doesn't directly involve AI, templates set the foundation for future AI-powered template suggestions based on job requirements

## Out of Scope

The following are explicitly **not** included in this feature:

- **AI-generated template suggestions**: Future enhancement where AI recommends templates based on job title or requirements entered by the recruiter
- **Template versioning**: Ability to track multiple versions of the same template and rollback to previous versions (current scope: single version per template, with edit history in audit log)
- **Template approval workflow**: Requirement for manager approval before custom templates are added to the organization library (current scope: recruiters can save templates immediately)
- **Template sharing across organizations**: Ability for different companies to share their templates in a public marketplace (current scope: templates are private to each organization)
- **Localized templates**: Templates translated into multiple languages for international hiring (current scope: templates are in the organization's primary language)
- **Template preview before selection**: Side-by-side comparison of multiple templates or live preview of how the template will populate the job form (current scope: template name and description provide sufficient context)
- **Template usage reports with ROI calculations**: Detailed analytics showing cost savings and recruiter productivity gains from template usage (current scope: basic metrics only - usage count and time saved)

---

**Next Steps**:

1. Run `/speckit.clarify` to identify any underspecified areas through targeted clarification questions
2. Run `/speckit.plan` to create technical implementation plan including data models, API contracts, and architecture
3. Run `/speckit.tasks` to generate dependency-ordered work tickets for implementation
