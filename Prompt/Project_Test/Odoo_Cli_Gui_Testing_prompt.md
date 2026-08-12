Odoo CLi:
---------------------

***Odoo Single-Role User Test:***
---------------
Find a (user_type/group: e.g. Portal user, Sales user, Purchase user, Inventory user, Accounting user, Sales Manager, Administrator, etc.) user's login and password from the database/config (or create one via Settings > Users if none exists).

Write a complete user details (login, password, associated groups/access rights) & functionality list according to the menus/routes accessible to that user (based on their assigned Odoo groups — e.g. for a Sales user: Sales > Quotations, Orders, Customers, Products; for a Portal user: My Account, My Orders, My Invoices, My Tickets) and save to this path => ****

According to this file path => **** test all functionality one by one. Firstly check the login, portal signup/registration (if applicable), and forgot password flow, then test all functionality one by one (every menu, list/kanban/form view, button, wizard, filter, and smart button accessible to that user/group, including verifying access rights correctly block anything outside their group). Then save the test report to this path => **** (PASS/FAIL with screenshot/description of each bug, including module, model/view name, and menu path).

Push the production branch with the current task as appropriate commit.

Now fix those bugs one by one from this file => ****

Push the production branch with the current task as appropriate commit.

Finally test all the functionality again and ensure everything is okay, then -> push the production branch with the current task as appropriate commit.

If again you find any error then fix one by one and then => push the production branch with the current task as appropriate commit.



Odoo GUI: 
---------


Here's the same testing script adapted for Odoo projects:

***Odoo Full Module UI Test:***
--------------------------------
***Phase 1: Flow Discovery***
----------------------------
Scan the full Odoo project (custom modules, models, views, controllers, security/access rules in ir.model.access.csv and record rules, workflows/state fields, automated actions, server actions) and find out every user role/group in this system (e.g. Portal user, Internal user, Sales/Purchase/Inventory/Accounting team member, Team Leader, Manager, Administrator, System Admin, etc.)

For each role/group, discover all possible flows step by step. Example:
  - Portal user (customer): register/login on portal -> browse products -> add to cart -> checkout -> place order -> view order status -> pay invoice -> submit ticket
  - Sales user: create quotation -> send to customer -> confirm sales order -> create delivery -> create invoice
  - Sales Manager: approve discount above limit -> approve quotation -> confirm order -> validate
  - Inventory user: process delivery order -> validate stock move -> handle backorder
  - Accounting user: validate invoice -> register payment -> reconcile -> issue credit note
  - Purchase user: create RFQ -> confirm PO -> receive goods -> validate vendor bill
  - Admin/System Admin: manage users, groups, access rights, module installation/config

Identify every cross-role/cross-app flow where one user's action depends on another user's approval or on a state transition. Example:
  - Sales user creates quotation -> Sales Manager approves (if above approval limit) -> order confirmed -> Inventory validates delivery -> Accounting validates invoice -> Accounting registers payment
  - Portal customer places order -> Sales team confirms -> Inventory ships -> Portal customer sees delivery status -> Portal customer downloads invoice
  - Purchase user creates RFQ -> Purchase Manager approves -> vendor confirms -> Inventory receives -> Accounting validates vendor bill -> payment
  - Employee submits expense -> Manager approves -> Accounting posts entry -> payment processed
  - Helpdesk ticket created by portal user -> assigned to agent -> escalated to manager -> resolved -> customer notified

Write the complete role/group list + full flow map (step by step, in dependency order, across all installed apps/modules) and save to this path => ****

***Phase 2: UI Testing (browser based)***
----------------------------------------
Launch the Odoo instance (with all relevant modules installed) and test every flow through the actual UI (browser automation / manual UI interaction in both backend web client and portal/website frontend), NOT just via ORM/API calls or shell scripts.

Follow the flow map from Phase 1 strictly in dependency order:
  1. First test auth for every role (portal signup, login, password reset, logout, internal user login, access rights denial for unauthorized actions)
  2. Then test each role's own functionality one by one through the UI (list/kanban/form/calendar views, filters, smart buttons, wizards)
  3. Then test cross-role/cross-app flows end-to-end in real order. Example:
       - login as Sales user -> create and send quotation
       - logout -> login as customer (portal) -> accept quotation online -> pay
       - logout -> login as Sales/Inventory user -> confirm order -> validate delivery
       - logout -> login as Accounting user -> validate invoice -> register payment
       - logout -> login as portal customer -> verify order/invoice/payment status updated correctly
  4. For every approval/state-transition step, verify the status change (e.g. Draft -> Confirmed -> Done, or Quotation -> Sales Order -> Invoiced -> Paid) is reflected correctly in the UI for all involved roles, including chatter/log notes, activities, and notifications/emails.

For every step verify: correct page/view loads, correct data shows, buttons/menus/smart buttons work, form validation errors show properly, required field checks work, status bar/state updates reflect everywhere, access rights are correctly enforced (no unauthorized access), and no console/JS errors or server tracebacks.

Save the full test report (flow by flow, step by step, PASS/FAIL with screenshot/description of each bug, including module name and view/model involved) to this path => ****

Push the production branch with the current task as appropriate commit.

***Phase 3: Bug Fixing***
------------------------
Now fix those bugs one by one from this file => ****

Push the production branch with the current task as appropriate commit.

***Phase 4: Final Regression***
------------------------------
Finally re-test all the flows end-to-end through the UI again (backend + portal) and ensure everything is okay, then -> push the production branch with the current task as appropriate commit.

If again you find any error then fix one by one and then => push the production branch with the current task as appropriate commit.
