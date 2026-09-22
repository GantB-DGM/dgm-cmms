# DGM Maintenance Requests

Repair and maintenance request tracking for Do Good Multnomah. It's built as a SharePoint Framework (SPFx) web part that also runs in Microsoft Teams.

**Status:** prototype. [`index.html`](index.html) is a clickable mockup with sample data. The SPFx build hasn't started.

## View the prototype

Download [`index.html`](index.html) and open it in a browser. It runs locally and loads fonts from Google Fonts and a QR-code library from cdnjs.

The dark bar at the top controls the demo. It isn't part of the product.

- **View as**: Staff, Program Manager, Senior Program Manager, Operations, Admin
- **Host**: SharePoint page or Teams
- **Modules**: *Core* (request tracking) or *Core + optional* (compliance dates, equipment, kitchens, recurring services)
- **Explain buttons**: each action shows what it does in the real build before it runs
- **Build notes**: the SharePoint lists and flows behind each screen

All people, vendors, and figures in the prototype are sample data.

## Scope

**Core (v1)**

- Anyone at DGM can report a problem: site, exact location (pod, room, or unit), category, guided urgency questions, and photos.
- Requesters track their own requests. PMs see their sites, SPMs see the sites they oversee, and Operations sees and manages everything. Admin can also change settings. PM and SPM assignments come from the Sites list shared with the Incident Reporting app.
- Operations assigns vendors, books site visits, records costs, and closes requests. The app never contacts vendors.
- Each notification can go by any mix of Teams channel post, Teams chat, and email. Admins configure this in Settings.

**Optional modules** (keep or cut after stakeholder review): compliance dates, equipment with QR labels, kitchens, recurring vendor services.

## Planned architecture

This follows the Incident Reporting app's pattern: the logic lives in the SPFx app, and the Power Automate flows stay thin.

| Piece | Job |
|---|---|
| SPFx web part | UI, request numbers (`MR-2026-0001`), response targets, site scoping, notification rules, message content, recurring requests |
| SharePoint lists | `MaintenanceRequests`, `RequestVisits`, `Vendors`, `Locations`, `NotificationRules`, `NotificationQueue`, `MaintenanceSettings`; optional: `RecurringServices`, `ComplianceItems`, `Assets` |
| Flow 1: relay | Sends each queued message: email from a shared mailbox, and Teams posts as the Flow bot. It contains no logic. |
| Flow 2: hourly check | Queues "past response target" alerts |
| Hosts | SharePoint page, Teams tab, Teams personal app (all the same web part) |

## Open questions

- Which SharePoint site hosts the app
- Which optional modules make v1
