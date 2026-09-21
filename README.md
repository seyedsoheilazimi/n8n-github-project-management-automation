# n8n GitHub Project Management Automation

An automated project-management workflow built with **n8n**, **GitHub Issues**, and **Google Sheets**.

The workflow classifies new GitHub issues using predefined keywords, routes each issue to the appropriate team, assigns the issue to a predefined GitHub user, logs the task in Google Sheets, and keeps the task status synchronized when the GitHub issue is closed.

## Overview

This project automates a simple issue-triage and task-tracking process.

When a new GitHub issue is created, the workflow:

1. Receives the issue event from GitHub.
2. Extracts the issue title, description, issue number, and URL.
3. Combines the title and description into a normalized text field.
4. Classifies the issue using keyword-based routing.
5. Routes the issue to one of three teams:
   - Bug / QA
   - Frontend
   - Backend
6. Assigns the issue to the predefined GitHub assignee for that team.
7. Stores the task in Google Sheets with its metadata and status.
8. Listens for future `closed` issue events.
9. Finds the matching task in Google Sheets using the GitHub issue number.
10. Updates the task status to `Done`.

## Workflow

```text
GitHub Trigger
      ↓
Issue Action Switch
   ├── opened
   │      ↓
   │  Extract Issue Data
   │      ↓
   │  Keyword Routing
   │   ├── Bug / QA
   │   ├── Frontend
   │   └── Backend
   │      ↓
   │  Set Team + Assignee
   │      ↓
   │  Assign GitHub Issue
   │      ↓
   │  Append Task to Google Sheets
   │
   └── closed
          ↓
      Match by Issue Number
          ↓
      Update Status = Done
```

## Keyword Routing

The workflow uses predefined keywords to determine the responsible team.

### Bug / QA

Example keywords:

```text
bug
error
crash
broken
issue
```

### Frontend

Example keywords:

```text
login
button
css
ui
responsive
mobile
```

### Backend

Example keywords:

```text
api
database
server
auth
backend
```

The issue title and body are combined and converted to lowercase before routing so keyword matching is consistent.

## Example

### GitHub Issue

**Title**

```text
Login button is broken on mobile
```

**Description**

```text
The login button is not responsive on small screens.
Users cannot log in from mobile devices.
```

The workflow detects matching keywords and routes the issue to the configured team and assignee.

## Google Sheets Task Tracker

Each new issue is stored in Google Sheets with fields such as:

| Issue Number | Title | Description | Team | Assignee | Status | GitHub URL | Created At |
|---|---|---|---|---|---|---|---|
| 2 | Login button is broken on mobile | The login button is not responsive... | Bug / QA | GitHub username | To Do | GitHub issue link | YYYY-MM-DD |

When the GitHub issue is closed, the workflow finds the matching row using **Issue Number** and changes:

```text
Status: To Do
→
Status: Done
```

## Tools Used

- **n8n**
- **GitHub Issues**
- **GitHub Webhooks / GitHub Trigger**
- **Google Sheets**
- **Keyword-based routing**
- **Switch nodes**
- **n8n expressions**

## Key n8n Concepts Used

- Event-driven workflows
- GitHub Trigger
- Data extraction and normalization
- Edit Fields
- Switch-based routing
- Regex / keyword matching
- Dynamic GitHub issue assignment
- Google Sheets append operations
- Google Sheets update operations
- Matching rows by a unique identifier
- Multi-branch workflow design
- Status synchronization

## What I Learned

This project helped me practice:

- Designing an automation around a real project-management process
- Routing tasks based on issue content
- Assigning GitHub issues automatically
- Using issue numbers as stable identifiers between systems
- Creating multiple workflow branches
- Synchronizing GitHub issue state with an external task tracker
- Working with GitHub webhook event actions such as `opened` and `closed`
- Testing end-to-end automation scenarios

## Tested Scenarios

The workflow has been tested for:

- ✅ Bug / QA issue routing
- ✅ Frontend issue routing
- ✅ Backend issue routing
- ✅ GitHub issue assignment
- ✅ New task creation in Google Sheets
- ✅ Closed issue detection
- ✅ Google Sheets status synchronization

## Possible Improvements

Future improvements could include:

- Automatically adding GitHub labels based on the detected team
- Priority detection using keywords such as `urgent` or `critical`
- Slack or Telegram notifications for newly assigned tasks
- Reassignment rules based on workload
- Due-date automation
- Reopened issue synchronization
- Better classification using an AI model instead of fixed keywords
- A dashboard for project status and team workload

## Repository Files

This repository can also include:

```text
README.md
workflow.json
workflow.png
```

- `workflow.json` — exported n8n workflow for importing into another n8n instance
- `workflow.png` — screenshot of the complete workflow

## Project Status

✅ Working prototype completed
