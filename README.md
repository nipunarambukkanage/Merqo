This README provides a working overview of the MERQO platform and its main application surfaces.

Current repository scope

- `frontend/`: manager portal for planning, approvals, analytics, configuration, and administration
- `backend/`: NestJS API, RBAC, workflow orchestration, persistence, and reporting endpoints
- `ai-service/`: Python service for AI/statistics support endpoints
- `mobile/`: React Native Expo field-execution app for riders / merchandisers

Operational model

- Managers and admins plan routes, assignments, products, questionnaires, and approvals in the web portal.
- Riders and merchandisers execute outlet visits from the mobile app.
- Both applications share the same backend and data model so field evidence flows back into approvals, alerts, dashboards, and compliance reporting.

Note: the long sections below still describe the manager portal in detail and contain some historical design commentary. Use the folder-level READMEs plus `mobile/README.md` for the current implementation view.

This README provides a complete overview of the Merqo Manager Portal, its purpose, technical implementation, and how to get started. It should serve as the primary reference for new developers and stakeholders to understand and extend the system. 

Application Overview
The Merqo Manager Portal is a comprehensive field sales and merchandising dashboard used by managers to oversee field reps, routes, outlets, products and competitor activities. It integrates data from field visits (orders, stock checks, photos, notes) and provides analytics, scheduling, alerts, and communications. The portal is organized into several main areas:

Dashboards: Sales Performance, Outlet Performance, Route Performance, Rider Productivity, Product Performance, Product Priorities, Competitor Insights, Exceptions & Alerts, and Approvals Queue. Each dashboard shows KPIs, charts, and lists to analyze coverage, sales, and quality metrics.
Operations: Visits scheduling and tracking (Assignments), Follow-up tasks (Actions Center), and internal communications (Messages). Managers assign visit plans, track completion, and create follow-up tasks from alerts, all while messaging riders.
Global UI: A fixed left sidebar (navigation) and top app bar. Common elements include a company selector, global search, filters, export, notifications, and user menu.
Each section includes a filter bar (date range, region/area, rider, channel, etc.) that updates all widgets. The style is a clean SaaS dashboard: light gray background, white cards with soft shadows, purple/lavender accents, rounded corners, and modern iconography. Typography is consistent (Inter-like), with clear hierarchy (titles 20–24px, headings 14–16px, body 12–14px). Buttons are purple-filled for primary actions, outlined for secondary, with subtle status badges.

Below is a summary of each major area, its purpose, and current functionality, followed by identified issues and suggested improvements.

Sales Performance Dashboard
Purpose: Track overall sales metrics and trends by region, product, and time.
Key Features (expected):

KPIs: Total sales revenue, units sold, new customers, average order value, growth rates.
Trends: Line/bar charts for sales over time, daily/weekly toggles.
Breakdowns: Sales by region/area (map or bar), by category, by top products/outlets.
Top Lists: Top-selling products or top-performing routes/outlets.
Filters: Date range, region, channel, product category, route, etc.
Actions: Export reports, compare periods, drill down to product or route details.
Observed Issues & Improvements:

Missing Implementation: The current design image does not show a dedicated Sales Performance page. Adding a dashboard with realistic dummy sales data would complete the suite.
Cross-Linking: KPIs (e.g. growth %) should link to relevant drill-downs (Product or Route dashboards) when clicked.
Charts: Ensure charts have tooltips, toggles (daily/weekly), and download icon.
Empty State: Handle cases with “No sales data” gracefully.
Outlet Performance Dashboard
Purpose: Monitor outlet coverage, distribution, and visit quality.
Key Features (expected):

KPIs: Total outlets, outlets visited %, new outlets added, outlets with issues (stockouts).
Coverage Maps: Region map highlighting outlet visit coverage.
Funnel: Outlets assigned → visited → purchased.
Channels/Priority: Breakdown by channel (GT/MT/HORECA) or priority tags (Gold/Silver).
Top/Bottom Lists: Under-served outlets, high OOS outlets, high growth outlets.
Filters: Region, channel, priority, route, rider, date.
Observed Issues & Improvements:

Not Present: If an Outlet Performance page exists in the Figma, check for missing tables or charts. If not, suggest adding one.
Functionality: Include ability to drill from an outlet list to outlet details or history.
Data Gaps: Allow filtering by visit status and prioritizing planning for uncovered outlets.
Route Performance & Management
Purpose: Analyze route efficiency, coverage and optimize routing.
Features:

Overview:

Header with Route Performance title, description.
Actions: Optimize Routes, Compare Routes, Share, date picker.
Filter bar: Region, Area, Route, Rider, outlet/channel/priority filters, “Save View”, “Advanced Filters”.
KPI Cards: Planned vs Completed visits, Coverage %, Avg Visit Duration, Efficiency Score, Distance vs Time, Exceptions count. Each has sparkline and tooltip.
Map & Heatmap Card: Shows all routes on map with coverage overlay; toggles Map/Heatmap/Territories; route selector; “Show live positions” toggle. Pin-click opens an Outlet Quick View drawer.
Trend Chart: Tabs (Completion, Coverage, Efficiency, Distance) with line chart and granularity toggle.
Top Routes List: Ranked top 5–10 routes by completion and efficiency, with mini progress bars and quick actions (View, Compare, Flag).
Exceptions Preview: List of recent route exceptions (missed, late, detours) with severity badges, linking to full exceptions page.
Coverage Gaps Card: Shows outlets not visited recently, breakdown by channel/priority, “Create catch-up route” button.
Routes Table: Full table with columns (Name, Region/Area, Rider, Planned vs Completed Stops %, Coverage %, Planned vs Actual km, Start/End times, Efficiency, Exceptions). Row actions: details, compare, export, reassign, create improvement plan. Supports multi-select, column chooser.
Route Details (drill-down):

Breadcrumb navigation.
Title: Route name and performance tagline.
Actions: Export report, Optimize this route, Reassign rider, Share.
Summary Strip: Key metrics (Completion %, Coverage %, Efficiency score, Planned vs Actual distance/time, On-time start %, Avg visit duration).
Timeline View: Vertical sequence of all stops showing outlet name, planned vs actual times, duration, status (Completed/Missed/Delayed/Out-of-route) with icons for photo/order/OOS. Filters to show only exceptions. Each outlet links to Outlet Coverage Detail.
Map (Route Only): Map with route polyline and stop markers.
Insights Panel: Highlights what drove score (e.g. “Late start + 2 missed stops”), with recommended actions (e.g. reorder stops, split route). Each recommended action has an “Apply” button (opens confirm modal).
Activity & Notes: Audit log of rider changes, optimizations, and manager notes.
Issues & Improvements:

Styling Consistency: Ensure the route map card and quick view drawer have enough width and content. Fix any modals/pickers that appear too narrow.
Filter Chips: Make sure chip filters match design elsewhere (same rounded style, removable).
Tooltips & Legend: Add proper legends on map and clear status color coding.
Data Flow: Clicking “Compare” should pre-fill with selected routes.
Missing Features: Consider adding a “Route Bottlenecks” report (e.g. repeated delays or missed clusters).
Help: Include inline tooltips or a Help (?) popover explaining metrics like “Efficiency Score”.
Rider Productivity Dashboard
Purpose: Track individual field rep performance and data quality.
Features:

Overview:

Title and subtitle on rider tracking.
Filters: date range, region, route, rider, channel, outlet priority, status toggles, “Only exceptions”, etc.
Header Actions: Message Riders, Compare, Create Task, Export.
KPIs: Visits Completed %, On-time %, Avg Visits per Day, Avg Visit Duration, Data Quality Score, Exceptions Count.
Trend Chart: Tabs for Visits, Punctuality, Quality, Exceptions with line chart.
Rider Leaderboard Table: Columns (Rider, Region/Area, Routes, Planned vs Completed Visits, Completion %, On-time %, Avg duration, Data Quality %, Exceptions). Row actions: View, Message, Coaching Note, Reassign, Export.
Riders Needing Attention: List top 5–10 riders flagged (high missed, late start, low quality) with quick actions.
Live Activity: Toggle to show currently active riders with last ping.
Data Quality Breakdown: Charts or badges for photo capture rate, GPS accuracy, form completion.
Exceptions Preview: Table or list of exceptions filtered to riders (linked to Exceptions & Alerts).
Rider Details (drill-down):

Breadcrumbs and rider name header.
Actions: Message, Create Task, Add Coaching Note, Export Report.
Summary Cards: Completion %, On-time %, Avg Duration, Data Quality, Exceptions, Distance.
Visits Table: Each visit with date, route, outlet, status, check-in/out, duration, evidence icons, order outcome, flags. Click opens Visit Timeline.
Trends: Similar charts for this rider (completions, punctuality, data quality over time).
Insights & Recommendations: “Late starts on 3 of 5 days – recommend morning call”, etc., with “Create coaching plan” or “Assign follow-up” actions.
Route Adherence: Mini map or summary of out-of-route events.
Notes & Flags: Manager notes and flags history.
Visit Timeline / Day Detail:

For a specific day’s visits. Shows chronological stops with planned vs actual times, duration, evidence, etc.
Right-side mini-map of travel.
“Open Outlet” links.
Issues & Improvements:

KPI Definitions: Add info popovers (e.g. what counts as “On-time”).
Sticky Headers: Ensure tables have sticky column headers and consistent row styling.
Evidence Icons: Clearly label (photo, form, note) with tooltips.
Filter States: “Only exceptions” should gray out completed visits or similar.
Missing Data: If no visits in range, show helpful empty state (with “Adjust date or filters” CTA).
Interactivity: Clicking leaderboard rows and tasks should pre-fill context (e.g. message rider button selects rider).
Product Performance & Priorities
Purpose: Monitor product-level sales, distribution, availability, pricing, and promotional status.
Features:

Overview:

Filters: date, region, channel, brand/category, product/SKU, availability, promotion flag, competitor.
Header Actions: “Add to Priorities”, Compare Products, Create Alert, Export.
KPIs: Total Sales (value or index), Units Sold, Distribution Coverage %, Out-of-Stock %, Avg Observed Price vs RSP, Competitor Price Gap.
Trends Card: Tabs (Sales, Coverage, OOS, Price) with line charts.
Category/Brand Mix: Stacked bar (Sales or Checks by Category/Brand).
Top/Bottom Products Table: Tabbed (Top Performers, Underperformers, Rising, Declining) with columns (Product, Brand, Sales, Coverage %, OOS %, Price Compliance %, Promotion flag, Exceptions). Row actions: View details, Add to Priorities, Create Alert, Compare, Export.
OOS Watchlist: List of products at high stockout risk.
Price Compliance Alerts: List of pricing issues (above RSP, below floor).
Competitive Insights: Summary (avg price gap, frequent competitor seen) and “Open competitor comparison”.
Data Quality Snapshot: Photo capture rate, form completion for product checks.
Product Details (drill-down):

Product header with tags (category, brand, priority).
Actions: Add/Remove Priority, Create Alert, Compare, Export.
Summary metrics (Sales, Coverage, OOS, Price, Competitor Gap, Alerts).
Tabs: Overview (trends, map of availability), Outlet Breakdown (table of outlets with status and price), OOS & Availability charts, Price & Promotions charts, Competitors (presence and pricing), Evidence & Quality (photos/forms for this product).
Outlet Breakdown: Table of all outlets that carry/check the product, with last visit date, in-stock status, observed price, promotion, evidence icons. Outlet row opens quick drawer with history and photos. Filter by “not checked recently”.

OOS & Availability Dashboard:

KPIs: OOS %, In-stock %, Low stock %, Not checked %, OOS incidents.
Charts: OOS trend, OOS by region/channel, top OOS products.
Actions: “Create catch-up plan”, “Create alert rule”.
Table of recent OOS incidents (with outlet, product, date, severity).
Price & Promo Tracker: Charts of observed price vs RSP, price distribution, promo impact, compliance.

Table of price observations (outlet, observed price, RSP, promo flag).
“Create alert: price variance”.
Data Quality & Evidence:

KPIs: Photo capture %, GPS match %, Form completion %.
Table of low-confidence observations (with missing evidence reasons).
Photo gallery of product checks.
Actions: “Request revisit”, “Flag issue”.
Product Priorities: (Implied) A view listing high-priority SKUs, with their targets and current status, to drive focus.

Issues & Improvements:

Navigation: Ensure “Add to Priorities” flows to a maintained list or flags the product.
Pricing Data: Clarify units (currency, share). Add info tooltips for “Avg Price vs RSP”.
Empty States: E.g., “No observed price data” or “No promo data” should guide user to relax filters.
Charts: Consistent styling (color palette) with rest of app. Include legends and download icons.
Consistency: Use same table format (fonts, paddings) as other tables.
Help: Consider an “About this data” popover for OOS vs distribution definitions.
Competitor Insights
Purpose: Track competitor presence, pricing, promotions, and market dynamics observed during visits.
Features:

Overview:

Filters: date, region, channel, competitor brand (multi), our brand/product (for context), type of insight (presence/price/promo/visibility), evidence availability.
KPIs: Competitor Presence %, New Competitor Entries, Promo Activity Count, Avg Price Undercut %, Visibility Score (from photos), High Severity Alerts.
Trends Chart: Tabs (Presence, New Entries, Undercut, Promo, Visibility) with line charts.
Market Map: Heatmap by region showing intensity of competitor activity (toggle by type).
Top Competitors List: By presence %, new entries, promo count, avg undercut, with trend arrows. Row actions: View details, Compare, Alert, Export.
Hotspots & Alerts: List of recent high-impact insights (“Comp X entered 12 new outlets”), clickable to details.
Promotions Snapshot: Chart of promo types (discounts, bundles, displays).
Pricing Watchlist: Top products with competitor undercut % and outlets affected.
Evidence Quality: % of insights with photos, forms, and any missing evidence stats.
Competitor Detail (drill-down):

Competitor name & logo header.
Metrics: Presence %, New entries, Promo count, Avg undercut, Visibility score, Alert count.
Tabs: Overview (trends, map, top areas), Outlets (list where competitor seen, last seen dates), Promotions (timeline and examples), Pricing (observed price charts), Visibility (photos/displays summary), Alerts.
Outlet-Level Presence: Table of outlets for a selected competitor, with status (seen/not seen), last seen, frequency. Drawer shows visit evidence. Filter by “first seen”.

Promotions Tracker: Table of observed competitor promotions with date, type, evidence. Charts of promo trends. Button to “Log new activation” manually.

Price & Assortment: Charts and tables showing price comparisons (competitor vs our products) and competitor SKU counts by channel.

Visibility (Share of Shelf): Proxy scoring from photos. Table of outlets, competitor displays count, photos.

Alerts: List and rules for competitor-related alerts (e.g., undercut thresholds, new entry spikes, promo spikes). Manage rules and view triggered alerts.

Issues & Improvements:

Data Completeness: Indicate when data (prices, SKUs) is incomplete with info.
Chart Clarity: Ensure competitor charts label which product/category they refer to.
Modals: Fix any “small width” modals (e.g., add/promo logging) to full width with scroll.
Consistency: Use same timeline style as other alerts.
Help: Explain how “visibility score” is calculated from photos.
Exceptions & Alerts
Purpose: Central inbox for all exceptions (operational issues) and alert rules.
Features:

Overview:

Filters: date range, severity, status, type (visit/product/competitor/data-quality/etc), region, assignee, SLA status.
Actions: Create Alert Rule, Bulk Assign, Export.
KPIs: Open Exceptions count, Critical/High count, Breached SLA count, Avg Time to Resolve (MTTR), Unassigned count, New Today.
Trends Chart: Exceptions over time by status/severity, MTTR trends.
Hotspots Map: Where most exceptions occur (by region/route).
Top Exception Types: Bar chart of incident counts (missed, late, OOS, etc).
Priority Queue Card: List of highest-priority new items with quick actions (Acknowledge, Assign).
Recent Resolved: List of last resolved incidents with resolution time.
Rules Health: Summary of active rules, noisy rules.
Inbox Preview: Table of recent exceptions (similar to full inbox but truncated) with link to open full queue.
Exceptions Inbox:

Table with columns (Severity, Status, Type, Summary, Entity (outlet/route/product), Area, Triggered at, SLA due, Assigned to).
Supports grouping (by type, severity, etc) and bulk actions (Assign, Change status, Dismiss).
Search in exceptions.
Bulk selection triggers action toolbar.
Triage panel with quick filters (unassigned, critical, etc), suggested assignees, playbooks.
Exception Detail:

Header with title (e.g. “High OOS – SKU 123, Colombo South”), severity badge, status, SLA countdown.
Actions: Acknowledge, Assign, Status Change, Dismiss, Export.
Summary: Type, triggered rule (if any), trigger time, affected entity, link to relevant dashboards (open product page, route page, etc.).
Tabs:
Details: Exact metrics or values that triggered the exception (e.g. planned vs actual visits, OOS% vs threshold, price vs RSP).
Evidence: Photos thumbnails, form fields, GPS map if relevant.
History: Audit trail of actions, notes, status changes.
Related: Similar past exceptions.
Assignment Panel: Change assignee, set priority/SLA, create a follow-up task.
Notes Panel: Internal comments thread.
Resolution Panel: Mark resolved with reason and notify.
Alert Rules:

List of rules (with name, condition summary, scope, frequency, status).
Create/Edit rule wizard (type, thresholds, scope, severity mapping, notification settings, preview triggers).
Toggle enable/disable, duplicate, archive rules.
SLA & Escalations:

Work queues for breaches.
SLA configs (e.g. auto-escalation after X hours).
KPIs: breach rate, avg response time.
Analytics:

Trends by type, pareto of exception sources, time-of-day heatmap.
Compare periods.
Issues & Improvements:

Clarity: Define “Exception” vs “Alert Rule” via a tooltip on the page.
UI Fixes: Some popups may be too narrow – ensure standard width (e.g., make the Assign/Edit rule modals wide enough).
Consistency: Use same badge colors and status chips as elsewhere.
Missing Items: Add “SLA met vs breached” charts.
Playbooks: Include quick-reference guides for handling each type.
Help/Onboarding: Consider a help section explaining the workflow (exceptions come from which dashboards, etc.).
Approvals Queue
Purpose: Managers review and approve data submissions from riders (visits, evidence, corrections).
Features:

Overview:

KPIs: Pending approvals, Due soon, Overdue, Avg Review Time, Approved Today, Rejected/Changes count.
Charts: Pending/Approved over time; top rejection reasons.
Cards: “My Queue” (items assigned to me), “Unassigned” (urgent items), “Policy Health” (active rules, noisy rules).
Quick actions: “Create Approval Rule”, “Bulk Approve”, “Export”.
Approvals Queue:

Table/card list of submissions awaiting review. Columns include Priority, Status, Type (Visit/Product/Comp/Correction), Summary, Rider, Outlet, Area, Submitted at, SLA due, Assigned to.
View toggle: table or card. Grouping options (by type, rider, status).
Bulk actions: Claim, Assign, Approve, Reject, Request Changes, Add Note.
Filters: Status, priority, type, region, assigned to, evidence presence.
Approval Detail:

Header: Submission title (e.g. “Visit report – Outlet X”), status badge, priority, SLA timer.
Actions: Claim, Assign/Reassign, Export packet, Approve, Request Changes, Reject, Notify.
Content Tabs:
Details: Fields submitted (planned vs actual times, outlet, visit notes, product observations).
Evidence: Photo gallery, form fields, GPS map.
Validation: Automated checks (GPS in/out, duration threshold, required photos).
History: Audit log.
Decision Panel: Large buttons for Approve, Request Changes (with checklist + comment), Reject (with reason dropdown + comment).
Notes: Internal comments.
Related Items: Other pending approvals from same rider or outlet.
Bulk Review:

Wizard to process multiple items (select items, review summary, choose batch action, confirm).
Bulk approve all passing, send change requests to rest, etc.
Approval Rules/Policies:

Configure when approvals are required (e.g. any missing photo, GPS miss, abnormal duration).
Thresholds for automated flagging (durations, price variance).
Assignment rules (by region or round-robin).
Templates for change requests.
Audit & Compliance:

Log of all approval actions.
Filters by reviewer, date, result.
Metrics: approval rates, common rejection reasons, compliance over time.
Issues & Improvements:

Workflow Gaps: Ensure “Request Changes” captures items back to rider and shows status.
UI Tweaks: Some dialogs (e.g. Claim confirmation) should be full width modals, not tiny popups.
Templates: Provide placeholder text for common reasons.
Notification: Confirm that notifications (in-app/email) would trigger on actions.
Help: Add tooltips explaining each status (Pending, Approved, etc.).
Assignments & Scheduling
Purpose: Create and manage field visit plans and tasks for riders.
Features:

Assignments Overview:

KPIs: Active assignments count, Due Today, Overdue, Completion Rate, Unassigned tasks, Avg Completion Time.
Charts: Created vs Completed assignments, Upcoming schedule (calendar strip).
My Team Workload: List of riders with number of assignments and capacity bars.
At Risk: Outgoing alerts for overdue tasks (“3 overdue today – View”).
Preview table of upcoming assignments with “Open Assignments”.
Assignments List:

View toggle: Table / Calendar / Map.
Table columns: Title, Type (visit, follow-up, product check, etc.), Priority, Status, Rider, Route/Area, Due date/time, Outlets count, Origin (manual, from exception, from approval, etc.), Evidence required (icon). Actions: Edit, Assign, Cancel, Export.
Calendar view: Drag-and-drop assignments by date.
Map view: Outlets/pin clusters for a selected assignment.
Grouping: By Rider / Route / Due date / Status.
Bulk actions: Assign rider, change date, change priority, cancel, export.
Create/Edit Assignment Wizard:

Step 1: Title, description, type, priority.
Step 2: Scope – select region/area/route and outlets (by filter criteria or manual pick).
Step 3: Tasks – checklist of required tasks (photo, OOS, price, notes, form).
Step 4: Schedule – due date/time window, recurrence.
Step 5: Assign – select rider(s), show capacity.
Step 6: Review & Create.
Assignment Detail:

Header: Title, status pill, priority. Actions: Edit, Reassign, Duplicate, Cancel.
Progress: outlets completed vs total with progress bar.
Outlet Checklist: Table of assigned outlets with status (pending/done), last visit, evidence icons, link to outlet detail.
Activity Timeline: log of assignment creation, updates, rider check-ins.
Assignee Panel: Rider, contact button (message), due date, SLA.
Evidence Panel: Collected photos/forms, any notes.
Follow-Up: Button to “Create Follow-Up Action” for incomplete items (links to Actions Center).
Route Allocation & Scheduling:

Tool to assign routes/outlets to riders.
Planner Board: Left list of unassigned routes/outlets, middle columns for each rider (drag/drop assignments).
Auto-Allocate: Modal to automatically assign based on objectives (minimize travel, balance load, maximize coverage).
Publish Schedule: Button to finalize and notify riders.
Issues & Improvements:

Filter Clarity: Ensure filters like “Include weekends” or “Only unassigned” behave correctly.
UI Fixes: Ensure calendar/event popovers have full details and adequate width.
Notifications: After creating assignment, option to send in-app/email alerts to riders.
Help: Include an “Assignments Help” section (e.g. explain recurring tasks, linking tasks to exceptions).
Actions Center
Purpose: A centralized place for all follow-up tasks and recommended actions (from exceptions, product issues, etc.).
Features:

Overview:

Title: “Actions Center” with subtitle.
Filters: similar to assignments (date, region, type, priority, status).
Actions: Create Action, Bulk Assign, Export.
KPIs: Open Actions, High Priority, Due Today, Overdue, Completed This Week, Avg Completion Time.
Trend Chart: Open vs Completed actions.
Action Types: Bar chart by type (Revisit, Verify Price, Restock, Coaching, Evidence capture).
Recommended Actions: Card listing top 8 suggested tasks (e.g. follow-up for stockout) with “Start” button.
From Exceptions: Card summarizing actions created from exceptions with breakdown by severity.
Queue Preview: Table of open actions.
Actions Queue:

List or Kanban of actions.
Columns: Priority, Status, Title, Source (Exception/Product/etc.), Entity (Outlet/Product/Route), Area/Route, Due, Assignee.
View/Group toggles.
Bulk actions: Assign, due date, priority, mark done, export.
Action Detail:

Header: Title, priority badge, status, due date.
Actions: Assign, Change status, Note, Export.
Context: Description of why action exists (link to source exception/alert).
Checklist/Steps: Specific sub-tasks for the action (editable).
Updates: Timeline of progress.
Assignee Panel: Assignee info, quick message button.
Evidence Panel: Upload field for photos/forms, thumbnails of uploaded evidence.
Complete Action: Button (enabled when criteria met). Option to mark blocked with reason (auto-creates exception if needed).
Playbooks & Automation:

Playbooks: Predefined templates for common actions (e.g. “OOS Verification”).
Automation Rules: E.g. “When exception X triggers, create action Y.” Enable/disable rules.
Issues & Improvements:

Integration: Clicking “Create Action” from an exception should pre-fill details.
Kanban: If using Kanban, ensure cards show key info (priority, due).
UI Touches: Fix any truncated text in cards; ensure popover forms are full-size.
Notifications: Option to notify assignee when new action assigned.
Help: Provide examples of each action type.
Messages (Communication)
Purpose: Internal messaging between managers and riders (1:1, group, and broadcasts).
Features:

Inbox:

Three-pane layout (conversation list, chat, details).
Left Pane: Search conversations, tabs (All, Unread, Groups, Broadcasts). Conversations show avatar, name, snippet, time, unread count.
Actions: New Message (select user(s)), New Broadcast (create announcement).
Middle Pane (Conversation): Header with recipient name/status, action buttons (Assign Task, Create Action, View Profile). Message history with date separators, read receipts.
Composer: Text input with attach (images/files), send button, quick reply templates, emoji.
Right Pane: Details of conversation (participants list), shared attachments, related items links.
Templates: Quick-reply templates on side (e.g. “Reminder: please visit Outlet X”).
Conversation View:

Shows threaded chat. Group chats labeled.
Ability to “Create Assignment” or “Create Action” from within a chat (opens modal).
Ability to view rider profile or assignment from a link.
Compose/Broadcast:

Modal or page to compose.
Step 1: Audience (choose individuals, groups by region/role, or broadcast all).
Step 2: Message content (subject, body, attachments).
Step 3: Schedule (send now or later).
Confirmation: Send/Save and show success toast.
Notifications & Templates:

Templates: List automated message templates (e.g. assignment assigned, overdue warning). Editable text with placeholders.
Preferences: Toggle in-app/email/push (if supported).
History: Log of sent notifications (time, recipient, status).
Issues & Improvements:

UI Consistency: Use same avatar and bubble styles as any existing messages UI.
Channels: Indicate online status or last active time.
Error Handling: Offline warning if connection lost.
Empty States: e.g. “No messages yet” illustration.
Attachment Viewer: Full-screen view for attached photos.
Help: Quick FAQ on how to use broadcasts vs group messages.
Cross-Cutting Issues & Improvements
Styling Consistency: Verify all text sizes, button styles, colors, and paddings match the Merqo design guide. For example, ensure all primary actions use the same purple button, table headers and cards have consistent shadows, and modals have uniform header/footer styling.
Font & Icons: Use the same sans-serif font (Inter or similar) throughout. Ensure icon style (outline vs filled) is consistent across modules.
Filter Chips: All filter chips and pill dropdowns should use the same style (rounded, removable “x”, lavender highlight). Make sure the “Clear all” and “Save view” actions are aligned with filters.
Modals & Drawers: Fix any modals that appear too narrow (10–20px). Standardize modal widths to at least 500–600px for forms, and full-screen width for complex dialogs (wizards). Ensure all popovers (tooltips, date pickers) are correctly sized.
Validation & Tooltips: Add info tooltips for any metric or icon that may be unclear (e.g. “Efficiency Score”, SLA definitions). Provide inline validation messages on forms.
Data Consistency: Use realistic dummy data across sections (same region names, rider names, priorities, channels) to ensure coherence.
Help & Documentation: Include a global Help/Info section or icon. This could be a help page or contextual “?” icons that explain each dashboard’s purpose and terminology. Consider an onboarding modal for first-time users summarizing how to navigate and create alerts.
Responsive Variants: Ensure each frame has tablet/mobile variants (if already done, verify layout stacks, if not mention as needed).
Performance & Loading States: Provide skeleton loading states for tables and charts. Show friendly empty states with illustration and CTA (e.g. “No assignments – create one now”). Include error states with retry for data loading failures.
Accessibility: Check color contrast (especially status badges on light backgrounds). Ensure all actionable items are keyboard-focusable and have descriptive text.
Naming & Navigation: Confirm all frames and components are clearly named for handoff (e.g. “RP-xx Overview”, “EAA-xx Exceptions Inbox”). In the prompt, name them as needed.
Unused/Obsolete Elements: Remove any placeholder screens or partial stubs that aren’t connected (e.g. if “Time & Attendance” or “Feedback & Supervision” have no content, omit or mark for future work).
Integration Flows: Ensure proper linking:
Clicking a rider in Assignments should allow messaging or viewing their profile.
Creating an assignment or action should trigger notifications via the Messages/Templates system.
From Exceptions or Actions, there should be buttons to message or assign tasks to riders.
From Messages, ability to create an Assignment/Action quickly.
The “Compare” functions should load a side-by-side frame.
Filters & Views: Make “Save view” store filter sets (UI only). All filters should reset properly on “Clear all”.
Notifications: If notifications exist, ensure a consistent panel showing recent system alerts (not just user notifications).
Deployment: Update any alerts count badges on menu items (they should reflect actual data or be hidden if zero).


Database Design Documentation for Merqo Manager Portal
This document outlines the complete database schema and relationships for the Merqo Manager Portal application. It covers all entities implied by the Figma UI (merchandising and field sales management), including tables, fields, data types, relationships, role-based access, and relevant database constructs (views, triggers, ORM modeling). The design assumes a relational database (e.g. PostgreSQL) and a Node.js ORM (e.g. Sequelize or TypeORM).

Core Entities and Tables
Organizations (organization): Stores client companies (e.g. Merqo Foods).

Fields: id (UUID, PK), name (string), type_id (FK to organization_type), created_at, updated_at.
Each user belongs to one organization; organization types (retailer, distributor, corporate) can define behavior.
Organization Types (organization_type): Lookup of organization categories.

Fields: id (int, PK), name (string, e.g. "Retailer", "Distributor").
Users (user): All people using the system (managers, riders, admin).

Fields: id (UUID, PK), org_id (FK to organization), username, email, password_hash, first_name, last_name, phone, avatar_url, is_active (bool), created_at, updated_at.
A user can have multiple roles (via user_role).
Roles (role): Predefined roles for access control (e.g. Admin, Manager, Rider, Team Lead).

Fields: id (int, PK), org_id (FK to organization), name, description. (Roles can be scoped to an organization or global.)
User Roles (user_role): Join table linking users to roles (many-to-many).

Fields: user_id (FK), role_id (FK). Composite PK (user_id, role_id).
Permissions (permission): Specific permissions or capabilities (optional, for fine-grained control).

Fields: id (int, PK), name, description.
Role Permissions (role_permission): Assigns permissions to roles (many-to-many).

Fields: role_id (FK), permission_id (FK). Composite PK.
Regions and Areas (region, area): Geographical hierarchies.

Region: id (PK), name.
Area: id (PK), region_id (FK), name.
Routes (route): Field routes covering outlets.

Fields: id (UUID, PK), org_id (FK), name, area_id (FK), description, created_at, updated_at.
Outlets (outlet): Retail locations on routes.

Fields: id (UUID, PK), org_id (FK), name, address, channel (enum: GT/MT/HORECA), priority (enum: Gold/Silver/Standard), latitude, longitude, created_at, updated_at.
Route Stops (route_stop): Many-to-many linking table for which outlets are on a route (ordered sequence).

Fields: route_id (FK), outlet_id (FK), sequence (int). Composite PK (route_id, outlet_id).
Defines the planned stops for each route with an order.
Riders (rider): (Optional separate table if riders have extra fields) Usually a subset of user with role = Rider. Could just use user with rider role.

If separate, store rider-specific data (vehicle type, capacity).
Assignments (assignment): Field tasks or visit plans assigned to riders.

Fields: id (UUID, PK), org_id (FK), title, description, type (enum: VisitPlan/FollowUp/ProductCheck/PriceVerify/CompetitorCheck/Survey), priority (enum), status (enum: Draft/Assigned/InProgress/Completed/Cancelled), created_by (FK to user), rider_id (FK to user), route_id (FK), due_date (datetime), recurrence (text), notify (bool), created_at, updated_at.
A high-level task (e.g. “Visit outlets in Colombo South”).
Assignment Outlets (assignment_outlet): Outlets tied to an assignment (many-to-many).

Fields: assignment_id (FK), outlet_id (FK), status (enum: Pending/Done/Skipped), completed_at (datetime). Composite PK.
Tracks which outlets are part of an assignment and their completion status.
Visits (visit): Actual check-in events.

Fields: id (UUID, PK), assignment_id (FK, optional), route_id (FK), rider_id (FK), outlet_id (FK), visit_date (date), planned_start (time), planned_end (time), actual_start (datetime), actual_end (datetime), duration (int), status (enum: Completed/Missed/Partial/OutOfRoute), notes (text), has_photo (bool), created_at, updated_at.
A visit record may come from an assignment or spontaneous route stop.
Products (product): Items sold or checked.

Fields: id (UUID, PK), org_id (FK), name, brand_id (FK), category_id (FK), sku, unit_of_measure (enum), created_at, updated_at.
Product Categories (category) and Brands (brand):

Category: id, org_id, name.
Brand: id, org_id, name.
Order or Sale (order or sale) (optional): If tracking actual orders; likely out of scope since UI focuses on visits and tasks.

Could include if needed, with fields like outlet, rider, date, total.
Product Observations (product_observation): Stock/availability captured during visits.

Fields: id (UUID, PK), visit_id (FK), product_id (FK), status (enum: InStock/OOS/LowStock/NotChecked), observed_qty (int), observed_price (decimal), form_data (JSON), created_at.
Competitors (competitor): Competing brands.

Fields: id (UUID, PK), org_id (FK), name, created_at, updated_at.
Competitor Observations (competitor_observation): Details of competitor presence in visits.

Fields: id (UUID, PK), visit_id (FK), competitor_id (FK), promo_type (enum or text), observed_price (decimal), notes, created_at.
Exceptions (exception): Operational anomalies.

Fields: id (UUID, PK), org_id (FK), type (enum: VisitIssue/ProductIssue/PriceIssue/CompetitorIssue/DataQuality/Other), entity_id (UUID, FK to related table if applicable), entity_type (string, e.g. “visit”, “product”, “outlet”), description, severity (enum: Low/Medium/High/Critical), status (enum: New/Acknowledged/InProgress/Resolved/Dismissed), raised_at (datetime), resolved_at, assigned_to (FK to user), created_at, updated_at.
References the item that triggered it (via entity_type and entity_id).
Alert Rules (alert_rule): Conditions for generating exceptions.

Fields: id (UUID, PK), org_id (FK), name, type (enum matching exception types), threshold (JSON or structured fields), condition (text or JSON), severity (enum), is_enabled (bool), created_at, updated_at.
Contains configuration (e.g. “if OOS% > 20% for 3 days”).
Approval Submissions (approval): Data items awaiting manager approval.

Fields: id (UUID, PK), org_id (FK), type (enum: VisitSubmission/ProductSubmission/CompetitorSubmission/Correction), entity_id (UUID, id of submission), entity_type (text), submitted_by (FK to user), status (enum: Pending/Approved/Rejected/ChangesRequested), priority (enum), due_date, created_at, updated_at.
Stores generic approval metadata. Detailed data stored in related tables (visits, etc.).
Approval History (approval_history): Audit trail for approvals.

Fields: id, approval_id (FK), action (enum: Submitted/Approved/Rejected/Commented), performed_by (FK to user), timestamp, notes.
Conversations (conversation): Messaging threads (1:1 or group).

Fields: id (UUID, PK), org_id (FK), title (for groups), type (enum: direct/group/broadcast), created_by (FK), created_at.
Conversation Participants (conversation_user): Users in a conversation (many-to-many).

Fields: conversation_id (FK), user_id (FK), last_read_at (datetime). Composite PK.
Messages (message): Individual messages.

Fields: id (UUID, PK), conversation_id (FK), sender_id (FK to user), content (text), sent_at, is_edited (bool).
Message Attachments (attachment): Files or images in messages.

Fields: id (UUID, PK), message_id (FK), file_type, file_url, created_at.
Notification Templates (notification_template): Automated message templates.

Fields: id, name, trigger_event (enum), subject, body_template (text with placeholders), is_enabled.
Notifications (notification): Sent notification records (optional log).

Fields: id, template_id, recipient_id, sent_at, read_at, status.
Audit Logs (audit_log): (Optional) Generic change log for critical tables.

Fields: id, entity (table name), entity_id, action (enum: CREATE/UPDATE/DELETE), performed_by (FK user), timestamp, details (JSON).
Entity Relationships
Organization to User: One-to-many. (A user belongs to one org; an org has many users.)
Organization to Outlet/Product/Competitor/Route/etc.: One-to-many. (All data is scoped to an organization.)
Region to Area: One-to-many. (Each area belongs to a region.)
Area to Route: One-to-many. (Routes are grouped by area.)
Route to Route Stop to Outlet: Many-to-many via route_stop. (A route has many outlets; an outlet can be on multiple routes.)
Route to User (Rider): One-to-many. (Each route has one assigned rider at a time; a rider can have multiple routes historically.)
User (Rider) to Visit: One-to-many. (Each visit record is done by one rider.)
Assignment to Outlet: Many-to-many via assignment_outlet. (An assignment can cover many outlets; an outlet can appear in many assignments.)
Assignment to User (Rider): Many-to-one. (Each assignment is given to one rider; a rider has many assignments.)
Assignment to Route: Many-to-one (an assignment may target a route).
Visit to Assignment: Many-to-one (visits come from an assignment or can be ad-hoc without assignment).
Visit to Outlet: Many-to-one. (Each visit happens at one outlet.)
Visit to Route: Many-to-one (each visit is on one route).
Visit to ProductObservations: One-to-many (a visit can have many product observations).
Visit to CompetitorObservations: One-to-many.
Outlet to ProductObservations and CompetitorObservations: One-to-many (observations are tied to outlet visits).
User to Conversation: Many-to-many via conversation_user. (Users participate in multiple chats.)
Conversation to Message: One-to-many.
User to Message: One-to-many (sender).
Role to Permission: Many-to-many.
User to Role: Many-to-many.
Exception references various entities (via entity_type/entity_id): could be Visit, Product, Outlet, etc.
Approval references submissions (similarly via entity_type/entity_id).
Authentication and Authorization (RBAC)
Authentication uses the user table with hashed passwords. Implement login via email/username, verify against password_hash.
Authorization is role-based: each user has one or more roles (user_role). Roles grant permissions via the role_permission table.
Common roles: Admin (full access), Manager (manage analytics and tasks, moderate permissions), Team Lead (regional manager), Rider (limited to own data), etc.
Permissions might include: view dashboards, manage routes, assign tasks, review approvals, manage alerts, send messages, etc. These map to permission entries and are linked via roles.
Organizations: Data is partitioned by organization. A user’s org_id determines which data they can access.
Multi-tenant considerations: If supporting multiple client organizations, ensure queries filter by org_id for security.
User status (is_active): Inactive users cannot log in.
Audit: Sensitive actions (approve, assign, delete) should be logged via audit_log.
Example Table Definitions (with Data Types)
Below is a summary of key tables with example SQL-like definitions (PostgreSQL syntax).

user (UUID PK, FK org_id (UUID), VARCHAR, TEXT, TIMESTAMP):

id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
org_id UUID REFERENCES organization(id) ON DELETE CASCADE,
username VARCHAR(64) UNIQUE NOT NULL, email VARCHAR(255) UNIQUE NOT NULL,
password_hash TEXT NOT NULL, first_name VARCHAR(64), last_name VARCHAR(64),
phone VARCHAR(20), avatar_url TEXT, is_active BOOLEAN DEFAULT TRUE,
created_at TIMESTAMP, updated_at TIMESTAMP.
role (INTEGER PK, FK org_id (UUID)):

id SERIAL PRIMARY KEY, org_id UUID REFERENCES organization(id),
name VARCHAR(64) NOT NULL, description TEXT.
user_role:

user_id UUID REFERENCES user(id), role_id INTEGER REFERENCES role(id),
PRIMARY KEY (user_id, role_id).
region: id SERIAL PRIMARY KEY, name VARCHAR(64).

area: id SERIAL PRIMARY KEY, region_id INTEGER REFERENCES region(id), name VARCHAR(64).

route:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
area_id INTEGER REFERENCES area(id), name VARCHAR(128), description TEXT,
created_at TIMESTAMP, updated_at TIMESTAMP.
outlet:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
name VARCHAR(128), address TEXT, channel VARCHAR(16), priority VARCHAR(16),
latitude DOUBLE PRECISION, longitude DOUBLE PRECISION,
created_at TIMESTAMP, updated_at TIMESTAMP.
route_stop:

route_id UUID REFERENCES route(id), outlet_id UUID REFERENCES outlet(id),
sequence INTEGER, PRIMARY KEY (route_id, outlet_id).
assignment:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
title VARCHAR(128), description TEXT, type VARCHAR(32),
priority VARCHAR(16), status VARCHAR(16), created_by UUID REFERENCES user(id),
rider_id UUID REFERENCES user(id), route_id UUID REFERENCES route(id),
due_date DATE, recurrence VARCHAR(64), notify BOOLEAN,
created_at TIMESTAMP, updated_at TIMESTAMP.
assignment_outlet:

assignment_id UUID REFERENCES assignment(id), outlet_id UUID REFERENCES outlet(id),
status VARCHAR(16), completed_at TIMESTAMP, PRIMARY KEY (assignment_id, outlet_id).
visit:

id UUID PRIMARY KEY, assignment_id UUID REFERENCES assignment(id),
route_id UUID REFERENCES route(id), rider_id UUID REFERENCES user(id),
outlet_id UUID REFERENCES outlet(id), visit_date DATE,
planned_start TIME, planned_end TIME,
actual_start TIMESTAMP, actual_end TIMESTAMP, duration INTEGER,
status VARCHAR(16), notes TEXT, has_photo BOOLEAN,
created_at TIMESTAMP, updated_at TIMESTAMP.
product:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
name VARCHAR(128), brand_id UUID REFERENCES brand(id), category_id UUID REFERENCES category(id),
sku VARCHAR(64), unit_of_measure VARCHAR(32),
created_at TIMESTAMP, updated_at TIMESTAMP.
brand: id UUID PRIMARY KEY, org_id UUID, name VARCHAR(64).

category: id UUID PRIMARY KEY, org_id UUID, name VARCHAR(64).

product_observation:

id UUID PRIMARY KEY, visit_id UUID REFERENCES visit(id), product_id UUID REFERENCES product(id),
status VARCHAR(16), observed_qty INTEGER, observed_price DECIMAL(10,2), form_data JSONB,
created_at TIMESTAMP.
competitor: id UUID PRIMARY KEY, org_id UUID, name VARCHAR(64).

competitor_observation:

id UUID PRIMARY KEY, visit_id UUID REFERENCES visit(id), competitor_id UUID REFERENCES competitor(id),
promo_type VARCHAR(64), observed_price DECIMAL(10,2), notes TEXT, created_at TIMESTAMP.
exception:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
type VARCHAR(32), entity_type VARCHAR(32), entity_id UUID,
description TEXT, severity VARCHAR(8), status VARCHAR(16),
raised_at TIMESTAMP, resolved_at TIMESTAMP, assigned_to UUID REFERENCES user(id),
created_at TIMESTAMP, updated_at TIMESTAMP.
alert_rule:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
name VARCHAR(128), type VARCHAR(32), condition JSONB, severity VARCHAR(8), is_enabled BOOLEAN,
created_at TIMESTAMP, updated_at TIMESTAMP.
approval:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
type VARCHAR(32), entity_type VARCHAR(32), entity_id UUID,
submitted_by UUID REFERENCES user(id), status VARCHAR(16),
priority VARCHAR(16), due_date DATE, created_at TIMESTAMP, updated_at TIMESTAMP.
approval_history:

id UUID PRIMARY KEY, approval_id UUID REFERENCES approval(id),
action VARCHAR(32), performed_by UUID REFERENCES user(id), timestamp TIMESTAMP, notes TEXT.
conversation:

id UUID PRIMARY KEY, org_id UUID REFERENCES organization(id),
title VARCHAR(128), type VARCHAR(16), created_by UUID REFERENCES user(id), created_at TIMESTAMP.
conversation_user:

conversation_id UUID REFERENCES conversation(id), user_id UUID REFERENCES user(id), last_read_at TIMESTAMP, PRIMARY KEY (conversation_id, user_id).
message:

id UUID PRIMARY KEY, conversation_id UUID REFERENCES conversation(id),
sender_id UUID REFERENCES user(id), content TEXT, sent_at TIMESTAMP, is_edited BOOLEAN.
attachment:

id UUID PRIMARY KEY, message_id UUID REFERENCES message(id), file_type VARCHAR(16), file_url TEXT, created_at TIMESTAMP.
notification_template:

id UUID PRIMARY KEY, name VARCHAR(128), trigger_event VARCHAR(32), subject VARCHAR(128), body_template TEXT, is_enabled BOOLEAN.
notification:

id UUID PRIMARY KEY, template_id UUID REFERENCES notification_template(id),
recipient_id UUID REFERENCES user(id), sent_at TIMESTAMP, read_at TIMESTAMP, status VARCHAR(16).
audit_log: (Optional)

id UUID PRIMARY KEY, entity VARCHAR(64), entity_id UUID, action VARCHAR(16),
performed_by UUID REFERENCES user(id), timestamp TIMESTAMP, details JSONB.
Views and Triggers
Database Views: Create read-only views for common reports. For example, a view v_route_performance could join route, route_stop, visit, and compute aggregate stats (total stops, completed %, avg duration). Another view v_product_sales could summarize sales or orders per product/outlet.

Example View (v_rider_leaderboard):

sql
Copy
CREATE VIEW v_rider_leaderboard AS
SELECT r.id AS rider_id,
       r.first_name || ' ' || r.last_name AS rider_name,
       r.area_id,
       COUNT(v.id) FILTER (WHERE v.status = 'Completed') AS visits_completed,
       COUNT(v.id) AS visits_planned,
       AVG(v.duration) AS avg_duration,
       -- additional metrics
FROM "user" r
LEFT JOIN visit v ON v.rider_id = r.id
GROUP BY r.id;
Triggers: Automate updates or logs. For example, a trigger on visit could update an assignment_outlet status to Done when a visit is completed. Or on inserting an exception, notify the assigned user. Triggers can also maintain materialized summary tables for performance.

Example Trigger (psuedo-code): After a visit is saved, update assignment progress:

sql
Copy
CREATE OR REPLACE FUNCTION fn_update_assignment_progress() RETURNS TRIGGER AS $$
BEGIN
  IF (NEW.status = 'Completed') THEN
    UPDATE assignment_outlet
    SET status = 'Done', completed_at = NEW.actual_end
    WHERE outlet_id = NEW.outlet_id AND assignment_id = NEW.assignment_id;
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER tr_update_assignment
AFTER INSERT OR UPDATE ON visit
FOR EACH ROW EXECUTE FUNCTION fn_update_assignment_progress();
Data Integrity Triggers: Enforce business rules (e.g. prevent changes after finalization).

Node.js ORM (Database-First) Approach
Use a Node.js ORM (e.g. Sequelize, TypeORM, or Objection.js) to map tables to models. A database-first approach means generating model definitions from the existing database or writing migration scripts to create these tables.

Models: Define one model per table with fields and types matching the above schema. For example, with Sequelize (using DataTypes):

js
Copy
const User = sequelize.define('User', {
  id: { type: DataTypes.UUID, primaryKey: true },
  orgId: { type: DataTypes.UUID, allowNull: false },
  username: DataTypes.STRING,
  email: DataTypes.STRING,
  passwordHash: DataTypes.TEXT,
  firstName: DataTypes.STRING,
  lastName: DataTypes.STRING,
  isActive: { type: DataTypes.BOOLEAN, defaultValue: true }
  // ... other fields
}, { tableName: 'user', timestamps: true });
User.belongsTo(Organization, { foreignKey: 'orgId' });
Associations: Define relationships in the ORM: hasMany, belongsToMany, etc. E.g. User.belongsToMany(Role, { through: 'user_role', foreignKey: 'user_id' }).

Migrations: Write migration scripts to create each table with constraints. Example using Sequelize CLI:

js
Copy
module.exports = {
  up: async (queryInterface, DataTypes) => {
    await queryInterface.createTable('user', {
      id: { type: DataTypes.UUID, defaultValue: DataTypes.UUIDV4, primaryKey: true },
      org_id: { type: DataTypes.UUID, allowNull: false, references: { model: 'organization', key: 'id' } },
      username: { type: DataTypes.STRING, allowNull: false, unique: true },
      // ... other columns
    });
    // ... create other tables
  },
  down: async (qi) => { await qi.dropTable('user'); /* ... */ }
};
Seeder Data: Optionally create seeders for roles (Admin, Manager, Rider), sample users, default organization types.

Views/Triggers in ORM: ORM may not natively manage views/triggers, so document them to be applied via raw SQL migrations or DB maintenance scripts.

Summary of Key Relationships
One-to-Many:

Organization → Users, Routes, Outlets, Products, Competitors, Assignments, Alerts.
Route → Route Stops (and through that to Outlets), Visits (via route_id).
Rider (User) → Visits, Assignments, Messages, Exceptions (assigned_to).
Outlet → Visits, Observations.
Visit → ProductObservations, CompetitorObservations.
Conversation → Messages.
Many-to-Many:

User ↔ Role (user_role), Role ↔ Permission (role_permission).
Assignment ↔ Outlet (assignment_outlet).
Route ↔ Outlet (route_stop).
Conversation ↔ User (conversation_user).
If needed: Outlet ↔ Product (to store distribution; could be derived from observations).
Foreign Key Constraints: Enforce integrity (e.g. if an org_id is deleted, cascade or restrict deletion for its data).

Indexes: Add indexes on frequently queried columns (usernames, foreign keys, dates).

This database schema captures all data needed for the Merqo merchandising app: organizations and roles for security, routes and outlets for planning, visits and observations for execution data, products and competitors for performance metrics, exceptions and approvals for issue tracking, assignments/actions for tasks, and messaging for communication. It can be implemented using Node.js ORM (with migrations and model definitions) and enriched with views/triggers for reporting and automation. The schema is normalized but can be extended (e.g. adding denormalized summary tables or materialized views) as needed for performance.
