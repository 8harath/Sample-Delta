# Sample Delta — Audit Employee Tracker

Sample Delta is a proposed employee and work-tracking platform for teams involved in auditing. It aims to give audit managers and team members a clear view of assignments, progress, deadlines, and outstanding review items.

## Partner Engagement Tracker prototype

A browser-only portfolio tracker prototype is available in [engagement-tracker](./engagement-tracker/). Open `index.html` directly in a browser. It includes a dashboard, partner directory, engagement search and details, JSON import/export, and optional direct local JSON-file saving in compatible browsers. The included records are fictional demo data.

See the [prototype README](./engagement-tracker/README.md) for local storage behavior, architecture, email alert limitations, and suggested next steps.

## Purpose

Audit engagements often involve multiple employees, overlapping tasks, review cycles, and time-sensitive deliverables. Sample Delta is intended to bring this information into one organized workspace, improving coordination and accountability.

## Planned Features

- **Employee directory:** Maintain team member profiles, roles, and assigned engagements.
- **Engagement tracking:** Organize audit projects by client, period, team, and status.
- **Task assignments:** Assign responsibilities, priorities, due dates, and owners.
- **Progress dashboard:** Track pending, in-progress, under-review, and completed work.
- **Timesheets:** Record time spent on audit tasks and engagements.
- **Review workflow:** Track reviewer feedback, open queries, and approvals.
- **Deadline reminders:** Highlight upcoming and overdue deliverables.
- **Activity history:** Keep a record of task updates for accountability.
- **Role-based access:** Separate permissions for employees, reviewers, and administrators.

## Intended Workflow

1. An administrator creates an audit engagement and adds team members.
2. A manager assigns audit tasks and deadlines.
3. Employees update their task progress and record time spent.
4. Reviewers provide feedback and mark work as reviewed.
5. Managers monitor completion, workloads, and outstanding items.

## Project Status

**Prototype available.** The partner engagement portfolio is a static local MVP under `/engagement-tracker`. The broader employee and audit workflow described above remains planned and is not implemented by this prototype.

## Data Privacy

Audit work can involve confidential client and employee information. The application should follow appropriate access controls, secure authentication, and data-retention policies. Avoid committing real client records, credentials, or sensitive audit evidence to this repository.

## License

No license has been selected yet.
