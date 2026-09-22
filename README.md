# DGM Maintenance Requests

Repair and maintenance request tracking for Do Good Multnomah. It's built as a SharePoint Framework (SPFx) web part that also runs in Microsoft Teams.

**Status:** prototype. [`index.html`](index.html) is a clickable mockup with sample data. The SPFx build hasn't started.

## View the prototype

Download [`index.html`](index.html) and open it in a browser. It runs locally and loads fonts from Google Fonts and a QR-code library from cdnjs.

The dark bar at the top controls the demo. It isn't part of the product.

- **View as**: Staff, Program Manager, Senior Program Manager, Operations, Admin
- **Host**: SharePoint page or Teams
- **Modules**: *Core* (request tracking), *+ Optional* (compliance dates, equipment, kitchens, recurring services), or *+ Ideas* (features considered but not planned, shown for discussion)
- **Explain buttons**: each action shows what it does in the real build before it runs
- **Build notes**: the SharePoint lists and flows behind each screen

All people, vendors, and figures in the prototype are sample data.

## Scope

**Core (v1)**

- Staff report problems for their own site: exact location (pod, room, or unit), category, guided urgency questions, and photos. An "Already reported?" check shows open requests at that site first, so people follow an existing request instead of filing a duplicate. There is no coverage mode in v1. PMs and SPMs can report for any of their sites and Operations for any site. On-call staff have no home site, so they can't submit.
- All staff, including Team Leads, see every request at their own site, but not costs, vendor contacts, or internal notes. PMs see their sites, SPMs see the sites they oversee, and Operations sees and manages everything. Admin can also change settings. PM and SPM assignments come from the Sites list shared with the Incident Reporting app.
- SharePoint can't hide individual columns, so costs, internal notes, and vendor contacts live in separate lists that staff can't read. Site scoping is done by the app, so staff can technically read other sites' requests. That's a known trade-off: as with IR, a provisioning script hides every list from site navigation and search.
- Operations assigns vendors, books site visits, records costs, and closes requests. The app never contacts vendors.
- Each notification can go by any mix of Teams channel post, Teams chat, and email. Admins configure this in Settings.
- **Add to my calendar**: any booked vendor visit (or all upcoming visits) can be saved as an Outlook calendar file (`.ics`), built in the browser with no flow.

**Optional modules** (keep or cut after stakeholder review): compliance dates, equipment with QR labels, kitchens, recurring vendor services.

**Admin (Settings)**, built so a non-IT admin can run the app:
- System health and a delivery log with Resend
- People and roles managed in the app (no Entra portal), and "Check a person" with site overrides
- Sites read from the IR Sites list, with one editor per field. Maintenance keeps its own Locations list, with bulk-add for units.
- Categories and waiting reasons
- Notification rules, and message wording with a live preview
- Form wording
- Response targets
- Module on/off switches
- An announcement banner
- Help articles and "What's new"
- Test data, archiving, and Excel import and export
- An audit log, plus settings history with undo
- Bulk actions and merging duplicates on requests
- Full vendor management

**Ideas** (not planned; each shows a rough build cost in the prototype): warranty tracking, vendor insurance, owner vs. DGM responsibility, charging costs to funding sources, unit turnover checklists, repeat-problem flags, site emergency info, checks done by site staff.

## Planned architecture

This follows the Incident Reporting app's pattern: the logic lives in the SPFx app, and the Power Automate flows stay thin.

| Piece | Job |
|---|---|
| SPFx web part | UI, request numbers (`MR-2026-0001`), response targets, site scoping, notification rules, message content, recurring requests |
| SharePoint lists | `MaintenanceRequests`, `RequestVisits`, `RequestFollowers`, `Vendors`, `Locations`, `NotificationRules`, `NotificationQueue`, `MaintenanceSettings`; restricted to Operations, PMs, SPMs, and Admin: `RequestCosts`, `RequestNotes`; optional: `RecurringServices`, `ComplianceItems`, `Assets` |
| Flow 1: relay | Sends each queued message: email from a shared mailbox, and Teams posts as the Flow bot. It contains no logic. |
| Flow 2: hourly check | Queues "past response target" alerts |
| Hosts | SharePoint page, Teams tab, Teams personal app (all the same web part) |

## Open questions

- Which SharePoint site hosts the app
- Which optional modules make v1
