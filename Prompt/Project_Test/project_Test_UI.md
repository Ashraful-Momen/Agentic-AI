
*** ecommerce full flow UI test :
--------------------------------

*** Phase 1 : Flow Discovery
----------------------------

scan the full project (routes, controllers, models, middleware, roles/permissions) and find out every user role in this ecommerce system (e.g. customer, vendor, admin, delivery man, super admin etc.)

for each role, discover all possible flows step by step. example :
  - customer : register -> login -> browse product -> add to cart -> checkout -> place order -> track order -> review
  - vendor : register -> admin approval (if needed) -> add product -> manage order -> approve/reject order -> payout
  - admin : approve vendor -> approve order -> manage product/category -> refund -> reports

identify every cross-role flow where one user's action depends on another user's approval/action. example :
  - customer places an order -> admin needs to approve -> vendor needs to accept -> delivery man ships -> customer confirms delivery
  - vendor registers -> admin approves -> vendor can add products
  - customer requests refund -> vendor/admin approves -> money returned

write the complete role list + full flow map (step by step, in dependency order) and save to this path => ****

*** Phase 2 : UI Testing (browser based)
----------------------------------------

launch the project (frontend + backend) and test every flow through the actual UI (browser automation / manual UI interaction), NOT just API calls.

follow the flow map from Phase 1 strictly in dependency order :
  1. first test auth for every role (register, login, forgot password, logout)
  2. then test each role's own functionality one by one through the UI
  3. then test cross-role flows end-to-end in real order. example :
       - login as customer -> place an order
       - logout -> login as admin -> approve that order from admin panel
       - logout -> login as vendor -> accept/process that order
       - logout -> login as customer -> verify order status updated, confirm delivery
  4. for every approval step, verify the status change is reflected correctly on both sides (UI of the approver AND the requester)

for every step verify : correct page loads, correct data shows, buttons/forms work, validation errors show properly, status updates reflect everywhere, and no console errors.

save the full test report (flow by flow, step by step, PASS/FAIL with screenshot/description of each bug) to this path => ****

push the production branch with the current task as appropiate commit

*** Phase 3 : Bug Fixing
------------------------

now fix those bugs one by one from this file => ****

push the production branch with the current task as appropiate commit

*** Phase 4 : Final Regression
------------------------------

finally re-test all the flows end-to-end through the UI again and ensure everything is okay then -> push the production branch with the current task as appropiate commit

if again you find any error then fix one by one and then => push the production branch with the current task as appropiate commit
