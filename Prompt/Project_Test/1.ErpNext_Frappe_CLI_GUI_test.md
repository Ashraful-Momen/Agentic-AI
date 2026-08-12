Here are both scripts adapted for ERPNext/Frappe, covering CLI (bench/Frappe test framework) and GUI (Playwright browser-based) testing.

***ERPNext/Frappe Full Module UI + CLI Test:***
--------------------------------
***Phase 1: Flow Discovery***
----------------------------
Scan the full Frappe/ERPNext project (custom apps, DocTypes, Roles, Role Permissions Manager, Permission Rules, workflows/workflow states, server scripts, client scripts, hooks.py, scheduled jobs) and find out every user Role in this system (e.g. Guest, Customer/Portal user, Employee, Sales User, Sales Manager, Purchase User, Purchase Manager, Stock User, Accounts User, Accounts Manager, HR Manager, System Manager, Administrator, etc.)

For each role, discover all possible flows step by step. Example:
  - Portal user (customer): register/login via portal -> browse items/website -> add to cart -> checkout -> place Sales Order -> view order status -> pay against Sales Invoice -> raise support ticket (HD Ticket)
  - Sales User: create Quotation -> submit -> convert to Sales Order -> create Delivery Note -> create Sales Invoice
  - Sales Manager: approve discount/credit limit override -> approve Quotation -> submit Sales Order
  - Stock User: create Stock Entry -> validate Delivery Note -> handle Pick List -> Stock Reconciliation
  - Accounts User: submit Sales Invoice -> create Payment Entry -> reconcile -> submit Credit Note
  - Purchase User: create Material Request -> create RFQ -> create Purchase Order -> create Purchase Receipt -> Purchase Invoice
  - HR Manager: approve Leave Application -> approve Expense Claim -> process Payroll
  - System Manager/Administrator: manage Users, Roles, Permissions, Workflow, app installs

Identify every cross-role/cross-DocType flow where one user's action depends on another's approval or a workflow state transition. Example:
  - Employee submits Expense Claim -> Manager approves (workflow state Draft -> Approved) -> Accounts processes Payment Entry
  - Customer places Sales Order (portal) -> Sales User confirms -> Stock User delivers -> Accounts Invoices -> Customer pays online -> Payment Entry auto-reconciled
  - Purchase User raises Material Request -> Purchase Manager approves -> Purchase Order sent to Supplier -> Stock User receives via Purchase Receipt -> Accounts matches Purchase Invoice -> Payment Entry
  - Employee applies for Leave -> Reporting Manager approves/rejects -> HR updates Leave Ledger
  - Customer raises HD Ticket (portal) -> Agent responds -> escalated to Team Lead -> resolved -> customer notified

Write the complete Role list + full flow map (step by step, in dependency order, across all installed apps/DocTypes) and save to this path => ****

***Phase 2A: CLI Testing (bench / Frappe test framework)***
----------------------------------------
Using `bench` CLI on the site, write and run automated tests using the Frappe test framework (`FrappeTestCase`, `bench --site [site-name] run-tests --app [app-name]` or `run-tests --module [module]`) to validate:
  - DocType-level validations, mandatory fields, and server-side business logic (validate, before_save, on_submit hooks)
  - Permission checks per Role (use `frappe.set_user()` in tests to simulate each role and assert allowed/denied actions)
  - Workflow state transitions (assert correct `workflow_state` after each transition, and that only permitted roles can trigger transitions)
  - REST API endpoints for each flow (using `frappe.client.get/post` or `requests` against `/api/method` and `/api/resource`) to confirm cross-role flows work at the data layer
  - Scheduled jobs / background jobs relevant to the flow (e.g. auto payment reconciliation, recurring invoice generation) via `bench execute`

Save CLI test results (pass/fail, error tracebacks, DocType/method involved) to this path => ****

***Phase 2B: GUI Testing (Playwright browser based)***
----------------------------------------
Launch the ERPNext/Frappe site (bench start, or the target URL) and write Playwright test scripts (TypeScript/JavaScript or Python — match project convention) to test every flow through the actual UI (Desk UI and Portal/Website), NOT just API/CLI calls.

Follow the flow map from Phase 1 strictly in dependency order:
  1. First test auth for every role (Desk login, Portal login/signup, forgot password/reset link flow, logout, invalid credential handling)
  2. Then test each role's own functionality one by one through the UI (List View, Report View, Kanban, Form View, Workflow buttons, child tables, Quick Entry, filters, dashboards)
  3. Then test cross-role flows end-to-end in real browser sessions using Playwright's `browserContext`/storageState per role. Example:
       - login as Sales User -> create + submit Quotation
       - logout -> login as Sales Manager -> approve/submit Sales Order from the workflow action button
       - logout -> login as Stock User -> create Delivery Note, submit
       - logout -> login as Accounts User -> create Sales Invoice, Payment Entry
       - logout -> login as Portal customer -> verify order/invoice/payment status updated correctly on "My Account" page
  4. For every approval/workflow transition, verify the `workflow_state`/status field, document indicator color, and Timeline/Comments are correctly reflected in the UI for all involved roles, and that email/system notifications are triggered where configured.

For every step verify: correct page/DocType form loads, correct data shows, buttons/menu items/workflow actions work, mandatory field validation errors show properly, permission errors correctly block unauthorized roles (403/permission denied UI), status/indicator updates reflect everywhere, and no browser console errors or Frappe server tracebacks (check `bench` logs / `frappe.log`).

Use Playwright's screenshot-on-failure and trace viewer for debugging; save the full test report (flow by flow, step by step, PASS/FAIL with screenshots, console logs, and Playwright trace links) to this path => ****

Push the production branch with the current task as appropriate commit.

***Phase 3: Bug Fixing***
------------------------
Now fix those bugs one by one from this file => ****

Push the production branch with the current task as appropriate commit.

***Phase 4: Final Regression***
------------------------------
Finally re-run both the CLI test suite (`bench run-tests`) and the full Playwright UI suite end-to-end, and ensure everything is okay, then -> push the production branch with the current task as appropriate commit.

If again you find any error then fix one by one and then => push the production branch with the current task as appropriate commit.

---

***ERPNext/Frappe Single-Role User Test (CLI + Playwright):***
---------------
Find a (Role: e.g. Sales User, Purchase Manager, Stock User, Accounts User, Portal/Website User, HR User, System Manager, etc.) user's username/email and password from the site (or create one via Desk > User List, or `bench --site [site-name] add-user`, or `bench --site [site-name] set-user-password`).

Write complete user details (email/username, password, assigned Roles, Role Profile if used) & functionality list according to that role's accessible Workspaces/Modules/DocTypes (per Role Permissions Manager) and save to this path => ****

CLI Testing:
Using `bench --site [site-name] console` or a `FrappeTestCase` with `frappe.set_user(<user_email>)`, test the user's permitted DocType operations (create/read/write/submit/cancel/delete as applicable) and confirm permission-denied errors occur for anything outside their Role. Save results to this path => ****

GUI Testing (Playwright):
According to the functionality list file => ****, write Playwright scripts to test all functionality one by one via the browser. Firstly check Desk/Portal login, then (if applicable) self-signup on portal and forgot-password/reset-link flow, then test all functionality one by one — every Workspace shortcut, List View, Form View, workflow action button, report, and filter accessible to that user, including verifying permission errors correctly appear for restricted DocTypes/actions. Then save the test report to this path => **** (PASS/FAIL with screenshots, Playwright trace, DocType/page involved).

Push the production branch with the current task as appropriate commit.

Now fix those bugs one by one from this file => ****

Push the production branch with the current task as appropriate commit.

Finally re-run both CLI and Playwright tests for this role and ensure everything is okay, then -> push the production branch with the current task as appropriate commit.

If again you find any error then fix one by one and then => push the production branch with the current task as appropriate commit.
