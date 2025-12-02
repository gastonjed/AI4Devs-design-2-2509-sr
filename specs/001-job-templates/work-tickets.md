# Work Tickets: US-003 Job Templates

**Feature**: Job Templates
**Branch**: `001-job-templates`
**Spec**: [spec.md](./spec.md)
**Created**: 2025-12-02

This document provides detailed work tickets for implementing US-003 (Use Job Templates) based on the functional specification. Tickets are organized by user story priority and include technical implementation details.

---

## Ticket Organization

Tickets are grouped by user story:
- **P1 Tickets (T001-T009)**: Create Job from Pre-Built Template (MVP)
- **P2 Tickets (T010-T015)**: Save Custom Job as Reusable Template
- **P3 Tickets (T016-T022)**: Manage Template Library

Each ticket includes:
- **Technical Requirements**: Specific implementation details
- **Acceptance Criteria**: Testable conditions for completion
- **Dependencies**: Other tickets that must be completed first
- **Estimated Effort**: Complexity rating using three methodologies:
  - **T-Shirt Size**: XS, S, M, L, XL (intuitive relative sizing)
  - **Fibonacci Points**: 1, 2, 3, 5, 8, 13, 21 (for velocity tracking)
  - **Time Estimate**: Rough development days (for planning)

## Effort Estimation Guide

| T-Shirt | Fibonacci | Days | Description | Examples |
|---------|-----------|------|-------------|----------|
| **XS** | 1 | 0.5 | Trivial change, no complexity | Config change, text update |
| **S** | 2-3 | 1-2 | Simple, well-defined task | Database migration, simple CRUD endpoint |
| **M** | 5 | 3-4 | Moderate complexity, some unknowns | Business logic implementation, API integration |
| **L** | 8 | 5-7 | Complex, multiple components | Full UI feature, complex service logic |
| **XL** | 13 | 8-10 | Very complex, cross-cutting concerns | Major architectural change, multi-component feature |
| **XXL** | 21+ | 10+ | Epic-sized, should be broken down | Consider splitting into smaller tickets |

**Note**: Estimates assume experienced developer, may vary by team velocity

---

## Priority 1 (P1): Create Job from Pre-Built Template

### T001: Design Job Template Data Model

**Description**: Define the data structure for job templates including all fields that can be templatized and metadata fields for template management.

**Technical Requirements**:
- Template entity must include:
  - `id`: Unique identifier for the template
  - `name`: Display name (e.g., "Senior Software Engineer")
  - `description`: Optional description explaining when to use this template
  - `category_tags`: Array of category labels (e.g., ["Engineering", "Software Development"])
  - `is_pre_built`: Boolean flag indicating if this is a system template vs. custom
  - `creator_id`: Reference to user who created the template (null for pre-built)
  - `organization_id`: Reference to organization that owns the template
  - `created_at`: Timestamp of creation
  - `updated_at`: Timestamp of last modification
  - `usage_count`: Integer tracking how many jobs have used this template
  - `is_favorite`: Boolean indicating if marked as organization favorite
  - `is_archived`: Boolean for soft deletion
  - `visibility`: Enum ("public", "private", "group") for access control
- Template content fields (matching job posting fields):
  - `job_title`: Pre-filled job title
  - `job_description`: Pre-filled description (rich text)
  - `requirements`: Pre-filled requirements (rich text)
  - `qualifications`: Pre-filled qualifications (rich text)
  - `benefits`: Pre-filled benefits description
  - `salary_range_min`: Optional minimum salary
  - `salary_range_max`: Optional maximum salary
  - `location_type`: Enum ("remote", "onsite", "hybrid")
  - `custom_fields`: JSON object for any additional custom fields

**Acceptance Criteria**:
- [ ] Data model documented with field names, types, and constraints
- [ ] All job posting fields are represented in template content
- [ ] Metadata fields support template management features (favorites, archiving, usage tracking)
- [ ] Model supports both pre-built and custom templates
- [ ] Organization-level isolation is enforced

**Dependencies**: None

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T002: Create Database Schema for Templates

**Description**: Implement the database schema for storing job templates based on the data model from T001.

**Technical Requirements**:
- Create `job_templates` table with all fields from T001
- Create indexes on:
  - `organization_id` (for filtering by organization)
  - `category_tags` (for category-based filtering)
  - `is_archived` (for filtering active templates)
  - `is_favorite` (for sorting favorites first)
- Add foreign key constraints:
  - `creator_id` references `users.id`
  - `organization_id` references `organizations.id`
- Add check constraints:
  - `name` must not be empty
  - At least one content field (title, description, requirements) must be populated
  - `usage_count` must be >= 0
- Add unique constraint on `(organization_id, name)` to prevent duplicate template names within an organization

**Acceptance Criteria**:
- [ ] Database migration script creates `job_templates` table
- [ ] All indexes are created for query performance
- [ ] Foreign key relationships are properly defined
- [ ] Constraints prevent invalid data (empty names, duplicate names per org)
- [ ] Migration can be rolled back cleanly

**Dependencies**: T001

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T003: Seed Pre-Built Template Data

**Description**: Create seed data for the initial set of pre-built job templates covering common roles.

**Technical Requirements**:
- Create seed script to populate 10 pre-built templates:
  1. **Software Engineer**: Entry-level software development role
  2. **Senior Software Engineer**: Experienced software development role with leadership
  3. **Engineering Manager**: People management role for engineering teams
  4. **Product Manager**: Product strategy and roadmap ownership
  5. **Product Designer**: UI/UX design and user research
  6. **Sales Representative**: Individual contributor sales role
  7. **Account Executive**: Strategic sales and account management
  8. **Marketing Manager**: Marketing campaigns and strategy
  9. **Customer Success Manager**: Customer relationship and retention
  10. **Data Analyst**: Data analysis and reporting
- Each template must include:
  - Realistic job title, description, requirements, qualifications, benefits
  - Appropriate category tags (e.g., "Engineering", "Sales", "Marketing")
  - `is_pre_built = true`
  - `creator_id = null` (system-generated)
  - `organization_id = null` (available to all organizations)
- Templates should be generic enough to work for most organizations while being specific enough to be useful
- Include placeholders in templates (e.g., "[Company Name]", "[Team Name]") that recruiters can replace

**Acceptance Criteria**:
- [ ] Seed script creates exactly 10 pre-built templates
- [ ] Each template has complete, realistic content for all major fields
- [ ] Templates cover diverse roles across Engineering, Product, Sales, Marketing, and Data
- [ ] Pre-built templates have `is_pre_built = true` and no `organization_id`
- [ ] Seed script is idempotent (can be run multiple times without duplicates)

**Dependencies**: T002

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T004: Implement Template List API Endpoint

**Description**: Create API endpoint to retrieve list of available templates for an organization, with filtering and search capabilities.

**Technical Requirements**:
- Endpoint: `GET /api/job-templates`
- Query parameters:
  - `organization_id`: Required, filter templates for specific organization
  - `search`: Optional, search by template name (case-insensitive partial match)
  - `category`: Optional, filter by category tag
  - `include_archived`: Optional boolean, default false (exclude archived templates)
  - `favorites_first`: Optional boolean, default true (sort favorites to top)
- Response format:
  ```json
  {
    "templates": [
      {
        "id": "uuid",
        "name": "Senior Software Engineer",
        "description": "Template for senior engineering roles",
        "category_tags": ["Engineering", "Software Development"],
        "is_pre_built": true,
        "is_favorite": false,
        "creator_name": null,
        "usage_count": 42,
        "created_at": "2025-01-01T00:00:00Z",
        "updated_at": "2025-01-15T10:30:00Z"
      }
    ],
    "total_count": 15
  }
  ```
- Authorization: Require authenticated user with "Recruiter" role or higher
- Data access rules:
  - Include pre-built templates (`organization_id IS NULL`)
  - Include organization's custom templates (`organization_id = :org_id`)
  - Exclude archived templates unless `include_archived=true`
  - Apply search and category filters
  - Sort by: favorites first (if enabled), then usage_count desc, then name asc

**Acceptance Criteria**:
- [ ] Endpoint returns both pre-built and organization-specific templates
- [ ] Search parameter filters by template name (case-insensitive)
- [ ] Category parameter filters by exact category tag match
- [ ] Archived templates are excluded by default
- [ ] Favorites appear at top of results when `favorites_first=true`
- [ ] Unauthorized requests return 401 error
- [ ] Requests for non-existent organization return 404 error
- [ ] Response includes all required metadata fields

**Dependencies**: T002, T003

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T005: Implement Template Detail API Endpoint

**Description**: Create API endpoint to retrieve full template content including all pre-filled job fields.

**Technical Requirements**:
- Endpoint: `GET /api/job-templates/{template_id}`
- Path parameters:
  - `template_id`: UUID of the template to retrieve
- Response format:
  ```json
  {
    "id": "uuid",
    "name": "Senior Software Engineer",
    "description": "Template for senior engineering roles",
    "category_tags": ["Engineering", "Software Development"],
    "is_pre_built": true,
    "is_favorite": false,
    "creator_name": null,
    "usage_count": 42,
    "content": {
      "job_title": "Senior Software Engineer",
      "job_description": "<rich text content>",
      "requirements": "<rich text content>",
      "qualifications": "<rich text content>",
      "benefits": "<rich text content>",
      "salary_range_min": null,
      "salary_range_max": null,
      "location_type": "remote",
      "custom_fields": {}
    },
    "created_at": "2025-01-01T00:00:00Z",
    "updated_at": "2025-01-15T10:30:00Z"
  }
  ```
- Authorization: Require authenticated user with "Recruiter" role or higher
- Data access rules:
  - User can access pre-built templates (organization_id IS NULL)
  - User can access templates belonging to their organization
  - User cannot access templates from other organizations
  - Return 403 if user tries to access template from different organization

**Acceptance Criteria**:
- [ ] Endpoint returns complete template content including all job fields
- [ ] Pre-built templates are accessible to all authenticated users
- [ ] Organization templates are accessible only to users in that organization
- [ ] Attempting to access another organization's template returns 403 error
- [ ] Non-existent template ID returns 404 error
- [ ] Response includes all template metadata and content fields

**Dependencies**: T002, T003

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T006: Implement "Use Template" UI in Job Creation Flow

**Description**: Add "Use Template" button and template selector to the job creation page, allowing recruiters to browse and select templates.

**Technical Requirements**:
- Add "Use Template" button prominently on job creation page (before/above the job form)
- On button click, open template selector modal/drawer with:
  - Search input field (filters templates by name in real-time)
  - Category filter dropdown (filters by category tag)
  - "Favorites" toggle to show only favorites
  - Template list displaying:
    - Template name
    - Category tags as badges
    - Star icon if marked as favorite
    - Usage count (e.g., "Used 42 times")
    - "Preview" button to see template content summary
  - Pagination or infinite scroll for large template lists
- On template selection:
  - Fetch full template content via API (GET /api/job-templates/{id})
  - Pre-fill all job form fields with template content
  - Show visual indicator that job was created from a template (e.g., "Based on template: Senior Software Engineer")
  - Store reference to source template in job draft (for analytics)
- Handle edge cases:
  - If no templates available, show empty state with message "No templates available yet"
  - If search returns no results, show "No templates found for '[search term]'"
  - Show loading spinner while fetching template content
  - Show error message if template fetch fails

**Acceptance Criteria**:
- [ ] "Use Template" button is visible and accessible on job creation page
- [ ] Template selector modal opens on button click
- [ ] Search and category filters work correctly
- [ ] Template list displays all required information (name, tags, usage count)
- [ ] Selecting a template pre-fills all job form fields
- [ ] Job form indicates which template was used
- [ ] Empty state and error states are handled gracefully
- [ ] UI is responsive and accessible (keyboard navigation, screen reader support)

**Dependencies**: T004, T005

**Estimated Effort**:
- **T-Shirt**: L
- **Fibonacci**: 8 points
- **Time**: 5-7 days

---

### T007: Implement Template Application Logic

**Description**: Create the core logic that applies a template to a job draft, ensuring changes to the draft don't affect the original template.

**Technical Requirements**:
- When a template is selected:
  1. Create a new job draft record with `status = "draft"`
  2. Copy all content fields from template to job draft (deep copy, not reference)
  3. Set `source_template_id` field on job draft to track which template was used
  4. Increment `usage_count` on the template record
  5. Create a `template_usage_record` entry with:
     - `template_id`: Source template
     - `job_id`: New job draft
     - `user_id`: Recruiter who used the template
     - `used_at`: Current timestamp
- Ensure data isolation:
  - Job draft gets its own copy of template content
  - Edits to job draft do not modify template
  - Edits to template do not affect existing job drafts
- Track template usage for analytics:
  - Record timestamp when template is used
  - Record which recruiter used it
  - Track whether resulting job was published or abandoned

**Acceptance Criteria**:
- [ ] Selecting a template creates a new job draft with pre-filled content
- [ ] Job draft content is independent of template (deep copy, not reference)
- [ ] Template `usage_count` increments when template is used
- [ ] `template_usage_record` is created with correct metadata
- [ ] Editing job draft does not modify template
- [ ] Editing template does not affect existing job drafts
- [ ] Template usage can be tracked for analytics

**Dependencies**: T005, T006

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T008: Add Template Selection Confirmation

**Description**: Implement confirmation step when recruiter selects a template, showing a preview and allowing them to confirm before pre-filling the form.

**Technical Requirements**:
- After clicking a template in the selector, show a preview modal with:
  - Template name and description
  - Abbreviated preview of key fields (title, first 200 chars of description, requirements summary)
  - "Use This Template" button
  - "Cancel" button
- On "Use This Template" click:
  - Close preview modal
  - Apply template to job form (T007)
  - Close template selector
  - Focus on first editable field in job form
- On "Cancel" click:
  - Close preview modal
  - Return to template selector
- Include warning message: "Using this template will replace any content you've already entered. Your changes will be lost."
- Show warning only if job form has unsaved changes

**Acceptance Criteria**:
- [ ] Preview modal displays template summary before application
- [ ] "Use This Template" button applies template and closes modals
- [ ] "Cancel" button returns to template selector without changes
- [ ] Warning message appears if job form has unsaved content
- [ ] No warning appears if form is empty
- [ ] Preview shows sufficient detail for recruiter to make informed decision

**Dependencies**: T006, T007

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T009: Add Template Analytics Tracking

**Description**: Implement analytics tracking for template usage to measure adoption and effectiveness.

**Technical Requirements**:
- Track the following events:
  1. **Template List Viewed**: User opens template selector
  2. **Template Searched**: User enters search query
  3. **Template Category Filtered**: User selects category filter
  4. **Template Previewed**: User clicks to preview a template
  5. **Template Used**: User applies template to job
  6. **Job Published from Template**: Job created from template is published
  7. **Job Abandoned from Template**: Job created from template is deleted/abandoned
- For each event, capture:
  - User ID
  - Organization ID
  - Template ID (if applicable)
  - Timestamp
  - Additional context (search query, category, job ID, etc.)
- Calculate key metrics:
  - Template usage rate: (jobs from templates / total jobs)
  - Time to publish: (time from job creation to publication)
  - Template effectiveness: (published jobs from template / total uses of template)
  - Most popular templates: (ranked by usage count)
- Make metrics available via API for reporting (P3 feature):
  - `GET /api/analytics/templates/usage`
  - `GET /api/analytics/templates/{template_id}/effectiveness`

**Acceptance Criteria**:
- [ ] All template-related events are tracked with proper metadata
- [ ] Template usage rate can be calculated from tracked data
- [ ] Time-to-publish can be compared between templated and non-templated jobs
- [ ] Template effectiveness metrics are available
- [ ] Analytics data respects organization boundaries (users only see their org's data)
- [ ] Analytics endpoints require "Recruiting Manager" role or higher

**Dependencies**: T007

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

## Priority 2 (P2): Save Custom Job as Reusable Template

### T010: Implement "Save as Template" API Endpoint

**Description**: Create API endpoint to save an existing job posting as a reusable template.

**Technical Requirements**:
- Endpoint: `POST /api/job-templates`
- Request body:
  ```json
  {
    "source_job_id": "uuid",
    "template_name": "DevOps Engineer - Platform Team",
    "template_description": "Template for platform team DevOps roles",
    "category_tags": ["Engineering", "DevOps", "Cloud"]
  }
  ```
- Process:
  1. Fetch job posting by `source_job_id`
  2. Validate user has permission to access that job
  3. Copy all content fields from job to new template
  4. Set template metadata:
     - `name = template_name`
     - `description = template_description`
     - `category_tags = category_tags`
     - `is_pre_built = false`
     - `creator_id = current_user_id`
     - `organization_id = job.organization_id`
     - `created_at = now()`
     - `usage_count = 0`
  5. Validate template name is unique within organization
  6. Save template to database
- Response:
  ```json
  {
    "template_id": "uuid",
    "name": "DevOps Engineer - Platform Team",
    "message": "Template saved successfully"
  }
  ```
- Authorization: Require authenticated user with "Recruiter" role or higher
- Validation rules:
  - `template_name` is required and must be 1-100 characters
  - `template_name` must be unique within organization
  - `source_job_id` must exist and belong to user's organization
  - `category_tags` is optional (empty array if not provided)
  - At least one content field must have data (title, description, or requirements)

**Acceptance Criteria**:
- [ ] Endpoint creates new template from existing job
- [ ] Template name uniqueness is enforced within organization
- [ ] Template is associated with correct organization and creator
- [ ] Content fields are copied from source job
- [ ] Template appears in template list for organization
- [ ] Duplicate name returns 409 error with helpful message
- [ ] Invalid job ID returns 404 error
- [ ] Unauthorized access returns 403 error

**Dependencies**: T002

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T011: Add "Save as Template" Button to Job Form

**Description**: Add UI element to job creation/edit form allowing recruiters to save the current job as a template.

**Technical Requirements**:
- Add "Save as Template" button to job form (near "Save Draft" or "Publish" buttons)
- Button should be visible on:
  - Job creation page (after recruiter has entered content)
  - Job edit page (for existing jobs)
- On button click, open "Save as Template" modal with:
  - Template name input (required, pre-filled with current job title)
  - Template description textarea (optional)
  - Category tags multi-select dropdown (with common categories like "Engineering", "Sales", "Marketing", "Operations", "Executive")
  - "Save Template" button
  - "Cancel" button
- On "Save Template" click:
  - Validate template name is not empty
  - Call POST /api/job-templates endpoint (T010)
  - Show success message: "Template '[template_name]' saved successfully"
  - Close modal
  - Keep job form open (recruiter can continue editing job)
- Handle errors:
  - Duplicate name: Show error "A template named '[name]' already exists. Please choose a different name."
  - Network error: Show error "Failed to save template. Please try again."
  - Validation error: Show specific field errors inline

**Acceptance Criteria**:
- [ ] "Save as Template" button is visible on job creation and edit pages
- [ ] Button opens modal with template name, description, and category inputs
- [ ] Template name is required and pre-filled with job title
- [ ] Category tags dropdown includes common categories
- [ ] Clicking "Save Template" creates template via API
- [ ] Success message confirms template was saved
- [ ] Duplicate name error is shown clearly
- [ ] Job form remains open after saving template (not closed or reset)
- [ ] Modal can be canceled without saving

**Dependencies**: T010

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T012: Implement Template Name Uniqueness Validation

**Description**: Add real-time validation to check if template name already exists before user submits the form.

**Technical Requirements**:
- Add API endpoint: `GET /api/job-templates/check-name?organization_id={id}&name={name}`
  - Returns: `{ "exists": true/false }`
- In "Save as Template" modal:
  - On template name input blur (when user leaves the field):
    - Call check-name endpoint
    - If name exists, show inline error: "A template with this name already exists"
    - Disable "Save Template" button if name exists
  - On template name input change (while user is typing):
    - Clear previous validation error
    - Re-enable "Save Template" button
  - Debounce API calls (wait 500ms after user stops typing)
- Don't block submission if validation fails (server will validate anyway)
- Show friendly suggestion if duplicate: "Consider adding more detail to the name, like '[name] - [Team/Location]'"

**Acceptance Criteria**:
- [ ] Real-time validation checks for duplicate names
- [ ] Error message appears inline when duplicate name is detected
- [ ] Error message clears when user modifies the name
- [ ] "Save Template" button is disabled when duplicate exists
- [ ] API calls are debounced to avoid excessive requests
- [ ] Helpful suggestion is shown when duplicate is detected
- [ ] Validation works across different organizations (doesn't block same name in different orgs)

**Dependencies**: T010, T011

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T013: Add Template Library View

**Description**: Create a dedicated page where recruiters can view all templates available to their organization.

**Technical Requirements**:
- Create new page: `/templates` (or `/job-templates`)
- Page layout:
  - Header: "Job Templates" with optional "Create Template" button (links to job creation with instruction to use "Save as Template")
  - Search input: Filters templates by name
  - Category filter dropdown: Filters by category tag
  - Toggle: "Show Favorites Only"
  - Template grid/list:
    - Each template card shows:
      - Template name
      - Description (first 100 characters)
      - Category tags as badges
      - Star icon (filled if favorite, outlined if not)
      - Creator name (e.g., "Created by Rita Smith" or "Pre-built template")
      - Usage count (e.g., "Used 42 times")
      - Last updated date
      - "Use Template" button
      - "Edit" button (only for templates user has permission to edit - P3 feature)
    - Clicking "Use Template" redirects to job creation page with template pre-applied (T006, T007)
  - Empty state if no templates: "No templates available. Create your first template by posting a job and clicking 'Save as Template'."
- Pagination: Show 20 templates per page with "Load More" button or pagination controls
- Sort options: "Most Used", "Recently Updated", "Alphabetical"

**Acceptance Criteria**:
- [ ] Template library page is accessible from main navigation
- [ ] Page displays all templates available to user's organization (pre-built + custom)
- [ ] Search filters templates by name in real-time
- [ ] Category filter works correctly
- [ ] "Show Favorites Only" toggle filters to favorites
- [ ] Template cards display all required information
- [ ] "Use Template" button redirects to job creation with template applied
- [ ] Empty state is shown when no templates are available
- [ ] Pagination or infinite scroll works for large template lists
- [ ] Sort options correctly reorder templates

**Dependencies**: T004, T011

**Estimated Effort**:
- **T-Shirt**: L
- **Fibonacci**: 8 points
- **Time**: 5-7 days

---

### T014: Add Template Sharing Indicator

**Description**: Show visual indicators on templates to distinguish between pre-built, own custom, and shared by others.

**Technical Requirements**:
- In template list/cards, add visual badge or icon indicating template type:
  - **Pre-built**: Badge "Pre-built" with distinct color (e.g., blue)
  - **Own Custom**: Badge "My Template" (for templates created by current user)
  - **Shared**: Badge "Shared" with creator name (e.g., "Shared by Rita Smith")
- In template selector (T006) and template library (T013):
  - Show appropriate badge on each template card
  - Add tooltip on hover explaining badge (e.g., "Pre-built: Default templates provided by LTI ATS")
- In template detail view:
  - Show "Created by [name]" for custom templates
  - Show "Pre-built template" for system templates
  - Show "Last updated [date] by [name]" for edited templates

**Acceptance Criteria**:
- [ ] Pre-built templates show "Pre-built" badge
- [ ] User's own templates show "My Template" badge
- [ ] Templates created by others show "Shared" badge with creator name
- [ ] Badges are visually distinct and easy to understand
- [ ] Tooltips provide additional context on hover
- [ ] Template detail view shows creator and last modifier information

**Dependencies**: T005, T013

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T015: Implement Template Usage Success Message

**Description**: Show confirmation message to user after they successfully save a template, with quick link to view all templates.

**Technical Requirements**:
- After template is saved successfully (T011):
  - Show toast/snackbar notification with:
    - Success icon
    - Message: "Template '[template_name]' saved successfully"
    - Action link: "View All Templates" (links to template library T013)
    - Auto-dismiss after 5 seconds or manual close
- In template library (T013):
  - If user just saved a template (URL parameter: ?new_template={id}):
    - Scroll to and highlight the newly created template
    - Show badge "New" on the template card for 10 seconds
- Track in analytics (T009):
  - Event: "Template Saved"
  - Metadata: template_id, user_id, source_job_id, template_name

**Acceptance Criteria**:
- [ ] Success message appears after template is saved
- [ ] Message includes template name and link to template library
- [ ] Message auto-dismisses after 5 seconds
- [ ] User can manually close message
- [ ] Newly created template is highlighted in template library
- [ ] "New" badge appears on fresh template for 10 seconds
- [ ] Analytics event is tracked when template is saved

**Dependencies**: T011, T013

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

## Priority 3 (P3): Manage Template Library

### T016: Implement Template Edit API Endpoint

**Description**: Create API endpoint allowing authorized users to edit existing templates.

**Technical Requirements**:
- Endpoint: `PUT /api/job-templates/{template_id}`
- Request body:
  ```json
  {
    "name": "Senior Software Engineer (Updated)",
    "description": "Updated description",
    "category_tags": ["Engineering", "Senior Roles"],
    "content": {
      "job_title": "Senior Software Engineer",
      "job_description": "<updated rich text>",
      "requirements": "<updated rich text>",
      "qualifications": "<updated rich text>",
      "benefits": "<updated rich text>",
      "salary_range_min": 120000,
      "salary_range_max": 180000,
      "location_type": "hybrid"
    }
  }
  ```
- Authorization rules:
  - **Recruiting Managers**: Can edit any template in their organization
  - **Recruiters**: Can edit only templates they created (creator_id = current_user_id)
  - **Pre-built templates**: Can only be edited by Recruiting Managers
- Process:
  1. Validate user has permission to edit template
  2. Validate template name uniqueness (if name changed)
  3. Update template fields
  4. Set `updated_at = now()`
  5. Do NOT modify existing jobs created from this template
- Response:
  ```json
  {
    "template_id": "uuid",
    "name": "Senior Software Engineer (Updated)",
    "message": "Template updated successfully"
  }
  ```

**Acceptance Criteria**:
- [ ] Endpoint updates template metadata and content
- [ ] Recruiting Managers can edit any template in their organization
- [ ] Recruiters can only edit templates they created
- [ ] Pre-built templates require Recruiting Manager role
- [ ] Template name uniqueness is enforced (if name changed)
- [ ] Existing jobs created from template are not affected
- [ ] Unauthorized edit attempts return 403 error
- [ ] Non-existent template returns 404 error
- [ ] Duplicate name returns 409 error

**Dependencies**: T002

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

### T017: Implement Template Archive API Endpoint

**Description**: Create API endpoint allowing authorized users to archive (soft delete) templates.

**Technical Requirements**:
- Endpoint: `POST /api/job-templates/{template_id}/archive`
- Authorization: Require "Recruiting Manager" role
- Process:
  1. Validate user has permission (Recruiting Manager in template's organization)
  2. Set `is_archived = true` on template
  3. Set `archived_at = now()`
  4. Do NOT delete template from database (soft delete)
  5. Do NOT affect existing jobs created from this template
  6. Archived templates are excluded from template list (T004) by default
- Response:
  ```json
  {
    "template_id": "uuid",
    "message": "Template archived successfully"
  }
  ```
- Also implement unarchive endpoint:
  - `POST /api/job-templates/{template_id}/unarchive`
  - Sets `is_archived = false`
  - Requires same permissions

**Acceptance Criteria**:
- [ ] Endpoint archives template (sets is_archived = true)
- [ ] Only Recruiting Managers can archive templates
- [ ] Archived templates are hidden from default template list
- [ ] Archived templates can be unarchived
- [ ] Existing jobs created from template are not affected
- [ ] Archiving a template does not delete it from database
- [ ] Unauthorized archive attempts return 403 error
- [ ] Non-existent template returns 404 error

**Dependencies**: T002

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T018: Implement Toggle Favorite API Endpoint

**Description**: Create API endpoint allowing Recruiting Managers to mark templates as favorites.

**Technical Requirements**:
- Endpoint: `POST /api/job-templates/{template_id}/toggle-favorite`
- Authorization: Require "Recruiting Manager" role
- Process:
  1. Validate user has permission (Recruiting Manager in template's organization)
  2. Toggle `is_favorite` field (if true, set to false; if false, set to true)
  3. Return new favorite status
- Response:
  ```json
  {
    "template_id": "uuid",
    "is_favorite": true,
    "message": "Template marked as favorite"
  }
  ```
- Favorite status is organization-wide (not per-user)
- Favorite templates appear at top of template list when `favorites_first=true` (T004)

**Acceptance Criteria**:
- [ ] Endpoint toggles is_favorite field
- [ ] Only Recruiting Managers can toggle favorite status
- [ ] Favorite status applies to entire organization
- [ ] Favorite templates appear first in template list (T004)
- [ ] Toggling favorite updates immediately without page refresh
- [ ] Unauthorized toggle attempts return 403 error
- [ ] Non-existent template returns 404 error

**Dependencies**: T002

**Estimated Effort**:
- **T-Shirt**: S
- **Fibonacci**: 2 points
- **Time**: 1-2 days

---

### T019: Add Template Management Page

**Description**: Create a dedicated management page for Recruiting Managers to view, edit, archive, and manage templates.

**Technical Requirements**:
- Create new page: `/templates/manage` (accessible only to Recruiting Managers)
- Page layout:
  - Header: "Manage Templates" with statistics:
    - Total templates
    - Custom templates
    - Pre-built templates
    - Archived templates
  - Filter tabs: "All", "Pre-built", "Custom", "Archived"
  - Search and category filters (same as T013)
  - Template table with columns:
    - Template name (with badge: Pre-built / Custom)
    - Category tags
    - Creator name
    - Last modified date
    - Usage count
    - Favorite status (star icon, clickable to toggle)
    - Actions: "Edit", "Archive/Unarchive", "View Analytics"
  - Pagination for large lists
- Actions:
  - **Edit button**: Opens template edit modal (inline editing or navigate to edit page)
  - **Archive button**: Shows confirmation dialog, then archives template (T017)
  - **Favorite toggle**: Toggles favorite status immediately (T018)
  - **View Analytics**: Shows template usage metrics (T009 data)
- Permission check: If user is not Recruiting Manager, redirect to regular template library (T013)

**Acceptance Criteria**:
- [ ] Management page is accessible only to Recruiting Managers
- [ ] Page displays all templates with management metadata (usage, creator, modified date)
- [ ] Filter tabs correctly filter templates (all, pre-built, custom, archived)
- [ ] Edit button allows editing template content
- [ ] Archive button shows confirmation and archives template
- [ ] Favorite toggle works immediately without page refresh
- [ ] Usage count is displayed accurately
- [ ] Non-managers are redirected to regular template library
- [ ] Empty states are shown when no templates match filters

**Dependencies**: T004, T013, T016, T017, T018

**Estimated Effort**:
- **T-Shirt**: L
- **Fibonacci**: 8 points
- **Time**: 5-7 days

---

### T020: Implement Template Edit Modal

**Description**: Create modal UI for editing template metadata and content.

**Technical Requirements**:
- Modal opened from template management page (T019) when "Edit" is clicked
- Modal layout:
  - Header: "Edit Template: [template_name]"
  - Form fields (matching save template flow T011):
    - Template name (required, with uniqueness validation T012)
    - Template description (optional textarea)
    - Category tags (multi-select dropdown)
    - Content fields (matching job form):
      - Job title
      - Job description (rich text editor)
      - Requirements (rich text editor)
      - Qualifications (rich text editor)
      - Benefits (textarea)
      - Salary range (min/max numeric inputs)
      - Location type (dropdown: Remote/Onsite/Hybrid)
  - Buttons: "Save Changes", "Cancel"
- On "Save Changes" click:
  - Validate all fields
  - Call PUT /api/job-templates/{id} endpoint (T016)
  - Show success message: "Template updated successfully"
  - Refresh template list
  - Close modal
- On "Cancel" click:
  - Show confirmation if form has unsaved changes: "You have unsaved changes. Discard changes?"
  - Close modal without saving
- Handle errors:
  - Show inline validation errors for each field
  - Show general error message if save fails

**Acceptance Criteria**:
- [ ] Modal opens with current template data pre-filled
- [ ] All template fields are editable
- [ ] Rich text editor works for description, requirements, qualifications
- [ ] Template name uniqueness validation works (T012)
- [ ] "Save Changes" updates template via API
- [ ] Success message confirms update
- [ ] Template list refreshes to show updated data
- [ ] "Cancel" closes modal without saving
- [ ] Confirmation appears if unsaved changes exist
- [ ] Validation errors are shown inline

**Dependencies**: T016, T019

**Estimated Effort**:
- **T-Shirt**: L
- **Fibonacci**: 8 points
- **Time**: 5-7 days

---

### T021: Add Template Usage Analytics View

**Description**: Create analytics view showing template usage metrics and effectiveness.

**Technical Requirements**:
- Accessible from template management page (T019) via "View Analytics" button
- Analytics modal/page shows:
  - **Template Overview**:
    - Template name
    - Total times used
    - Date range selector (last 7 days, 30 days, 90 days, all time)
  - **Usage Metrics**:
    - Chart: Usage over time (line chart showing uses per day/week)
    - Jobs created from template: Count
    - Jobs published from template: Count
    - Jobs abandoned from template: Count
    - Publish rate: (published / total uses) as percentage
  - **Effectiveness Metrics**:
    - Average time to publish: For jobs using this template
    - Comparison to non-templated jobs: "X% faster than creating from scratch"
    - User adoption: "Used by X recruiters"
  - **Recent Usage**:
    - Table of recent jobs created from template:
      - Job title
      - Created by (recruiter name)
      - Date created
      - Status (draft, published, archived)
- Data source: Query from `template_usage_records` (T007) and job publishing events (T009)

**Acceptance Criteria**:
- [ ] Analytics view displays template usage over time
- [ ] Metrics show total uses, published jobs, abandoned jobs
- [ ] Publish rate is calculated correctly
- [ ] Average time to publish is displayed
- [ ] Comparison to non-templated jobs is shown
- [ ] Recent usage table lists jobs created from template
- [ ] Date range selector filters metrics accordingly
- [ ] Analytics respect organization boundaries (only show data for user's org)
- [ ] Empty state shown for templates with no usage

**Dependencies**: T007, T009, T019

**Estimated Effort**:
- **T-Shirt**: L
- **Fibonacci**: 8 points
- **Time**: 5-7 days

---

### T022: Add Bulk Template Actions

**Description**: Allow Recruiting Managers to perform actions on multiple templates at once.

**Technical Requirements**:
- In template management page (T019):
  - Add checkbox column to template table
  - Add "Select All" checkbox in table header
  - When templates are selected, show bulk action toolbar with:
    - "Mark as Favorite" button
    - "Remove from Favorites" button
    - "Archive" button
    - "Cancel" button (clears selection)
- Bulk actions:
  - **Mark as Favorite**: Call toggle-favorite API (T018) for each selected template where `is_favorite = false`
  - **Remove from Favorites**: Call toggle-favorite API (T018) for each selected template where `is_favorite = true`
  - **Archive**: Show confirmation dialog: "Archive [X] templates? Archived templates will be hidden but can be restored later." Then call archive API (T017) for each selected template
- After bulk action completes:
  - Show success message: "[X] templates updated successfully"
  - Refresh template list
  - Clear selection
- Handle errors:
  - If some actions fail, show partial success message: "[Y] of [X] templates updated. [Z] failed."
  - Log failed template IDs for debugging

**Acceptance Criteria**:
- [ ] Checkboxes appear in template table for selection
- [ ] "Select All" checkbox selects/deselects all visible templates
- [ ] Bulk action toolbar appears when templates are selected
- [ ] "Mark as Favorite" button adds selected templates to favorites
- [ ] "Remove from Favorites" button removes selected templates from favorites
- [ ] "Archive" button shows confirmation and archives selected templates
- [ ] Success message shows count of updated templates
- [ ] Partial success message shows if some actions fail
- [ ] Template list refreshes after bulk action
- [ ] Selection is cleared after action completes

**Dependencies**: T017, T018, T019

**Estimated Effort**:
- **T-Shirt**: M
- **Fibonacci**: 5 points
- **Time**: 3-4 days

---

## Summary

### Total Effort Estimate

| Priority | Ticket Count | T-Shirt Sizes | Fibonacci Points | Time Estimate | MVP? |
|----------|--------------|---------------|------------------|---------------|------|
| P1 | 9 tickets (T001-T009) | 1L, 5M, 3S | 8 + 25 + 6 = **39 pts** | ~6 weeks | ✅ Yes |
| P2 | 6 tickets (T010-T015) | 1L, 3M, 2S | 8 + 15 + 4 = **27 pts** | ~3 weeks | ⚠️ Nice to have |
| P3 | 7 tickets (T016-T022) | 3L, 3M, 1S | 24 + 15 + 2 = **41 pts** | ~4 weeks | ❌ Future |
| **Total** | **22 tickets** | **5L, 11M, 6S** | **107 points** | **~13 weeks** | - |

**Breakdown by Size**:
- **Large (L)**: 5 tickets × 8 points = 40 points (~5-7 days each)
- **Medium (M)**: 11 tickets × 5 points = 55 points (~3-4 days each)
- **Small (S)**: 6 tickets × 2 points = 12 points (~1-2 days each)

**Velocity Planning**:
- Assuming 2-week sprints (10 working days)
- Typical developer capacity: 10-15 points per sprint
- **P1 MVP**: 39 points ÷ 12 points/sprint = ~3 sprints (6 weeks)
- **P1+P2**: 66 points ÷ 12 points/sprint = ~5.5 sprints (11 weeks)
- **All features**: 107 points ÷ 12 points/sprint = ~9 sprints (18 weeks)

### Recommended Implementation Order

**Sprint 1 (MVP - P1)**:
1. T001: Data model design
2. T002: Database schema
3. T003: Seed pre-built templates
4. T004: Template list API
5. T005: Template detail API

**Sprint 2 (MVP - P1)**:
6. T006: "Use Template" UI
7. T007: Template application logic
8. T008: Selection confirmation
9. T009: Analytics tracking

**Sprint 3 (P2 - Custom Templates)**:
10. T010: Save template API
11. T011: "Save as Template" button
12. T012: Name uniqueness validation
13. T013: Template library view

**Sprint 4 (P2 - Polish)**:
14. T014: Sharing indicators
15. T015: Success messages

**Sprint 5 (P3 - Management)**:
16. T016: Edit template API
17. T017: Archive template API
18. T018: Toggle favorite API
19. T019: Management page

**Sprint 6 (P3 - Advanced Features)**:
20. T020: Template edit modal
21. T021: Usage analytics view
22. T022: Bulk actions

### Dependencies Graph

```
T001 (Data Model)
  └─ T002 (Database Schema)
      ├─ T003 (Seed Data)
      │   └─ T004 (List API)
      │       └─ T006 (Use Template UI)
      │           └─ T007 (Application Logic)
      │               ├─ T008 (Confirmation)
      │               └─ T009 (Analytics)
      ├─ T005 (Detail API)
      │   └─ T006 (Use Template UI)
      ├─ T010 (Save API)
      │   └─ T011 (Save Button)
      │       ├─ T012 (Validation)
      │       └─ T013 (Library View)
      │           ├─ T014 (Sharing Indicators)
      │           └─ T015 (Success Messages)
      ├─ T016 (Edit API)
      │   └─ T019 (Management Page)
      │       └─ T020 (Edit Modal)
      ├─ T017 (Archive API)
      │   └─ T019 (Management Page)
      └─ T018 (Toggle Favorite API)
          └─ T019 (Management Page)
              └─ T022 (Bulk Actions)

T009 (Analytics)
  └─ T021 (Analytics View)
```

---

**Next Steps**:

1. Review and validate work tickets with engineering team
2. Break down Large (L) tickets into smaller subtasks if needed
3. Run `/speckit.plan` to create technical implementation plan with data models and API contracts
4. Begin Sprint 1 implementation with T001-T005
