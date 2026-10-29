# IT Help Desk Ticket Tracker with SLA Monitoring

A ticketing tracker built in **Excel** to practise the day-to-day workflow of an L1 service desk: log an incident, prioritise it, assign it, track it to resolution, and monitor whether the SLA was met.

> This is a personal learning project, not a production system. The SLA targets and sample tickets are examples I chose for practice.

## Screenshots

<!-- Replace these with your own screenshots after uploading them to the repo -->
![Tickets sheet](screenshots/tickets.png)
![Dashboard](screenshots/dashboard.png)

## Features

- Ticket fields: ticket ID, title, priority (P1-P4), status, assigned agent, created time, resolved time
- Status workflow: **Open -> In Progress -> Resolved**
- Dropdown lists for priority, status and agent to keep data consistent
- **Automatic calculation** of SLA hours, SLA due time, time taken and an **SLA breach flag**
- Breached tickets highlighted in red
- **Dashboard** with ticket counts, SLA breaches, SLA compliance %, average resolution time per priority and active tickets per agent
- Editable **Settings** sheet for SLA targets and agent names

## SLA targets used

| Priority | Meaning | Resolve within |
|---|---|---|
| P1 | Critical outage | 1 hour |
| P2 | Major impact | 4 hours |
| P3 | Minor issue | 8 hours |
| P4 | Request / low impact | 24 hours |

These can be changed in the **Settings** sheet and every calculation updates.

## How it works

| Column | Logic |
|---|---|
| SLA Hours | Looks up the ticket's priority in Settings (`INDEX` / `MATCH`) |
| SLA Due | Created time + SLA hours |
| Time Taken (hrs) | (Resolved time, or the current time if still open) - Created time |
| SLA Breach? | "Breached" if the resolved time (or current time for open tickets) is later than SLA Due, otherwise "Within SLA" |

Because open tickets are compared against the current time, their SLA status changes as time passes.

## How to use

1. Download `Helpdesk_Ticket_Tracker.xlsx` and open it in Excel.
2. In the **Tickets** sheet, fill the yellow columns (ID to Created Time) for each new ticket.
3. When a ticket is fixed, set the status to **Resolved** and enter the Resolved Time.
4. Open the **Dashboard** sheet to see the summary.
5. Replace the 6 sample tickets with your own.

## SQL practice

The same data can be stored as a database table named `tickets`. These queries show how the reports would work in SQL:

```sql
-- Active P1 tickets
SELECT * FROM tickets WHERE status <> 'Resolved' AND priority = 'P1';

-- Number of tickets per priority
SELECT priority, COUNT(*) FROM tickets GROUP BY priority;

-- Active workload per agent
SELECT assigned_to, COUNT(*) FROM tickets
WHERE status <> 'Resolved' GROUP BY assigned_to;

-- Average resolution time (hours) per priority
SELECT priority, AVG(time_taken_hrs) FROM tickets
WHERE status = 'Resolved' GROUP BY priority;
```

## What I learned

- How incident priority drives SLA targets and escalation
- The ticket lifecycle and why clear documentation matters
- Using lookup and date formulas to automate SLA tracking
- Basic reporting on support data, in Excel and SQL

## Possible improvements

- Add a ticket category column (Network, Software, Access, Hardware)
- Add a chart for tickets by priority and SLA compliance
- Move the data into a real database and practise the SQL queries on it
- Try the same workflow in a free ticketing tool such as Freshdesk or Zoho Desk

## Tools

Microsoft Excel (formulas, data validation, conditional formatting), SQL (practice queries)
