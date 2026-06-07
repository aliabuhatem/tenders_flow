# TenderFlow - Fixes & UI updates

## Cloud modal close actions bug
- [ ] Locate cloud modal markup (was commented out in tender_detail.html)
- [ ] Re-enable modal markup and ensure Alpine state scopes correctly
- [ ] Implement working close behavior:
  - [ ] X close button closes
  - [ ] Cancel closes
  - [ ] Backdrop click closes
  - [ ] Escape closes
  - [ ] Click inside does NOT close (stop propagation)
- [ ] Ensure Alpine.js loaded on tender_detail page
- [ ] Verify z-index / overlay does not block buttons
- [ ] Run any available dev/test command

## UI/UX table form for tender_details page
- [ ] Convert the following sections into table/form style UI:
  - [ ] Status changer
  - [ ] Proposal stages
  - [ ] Stage documents
  - [ ] Stage tasks
  - [ ] Tender-level documents
- [ ] Preserve all existing form POST actions and inputs
- [ ] Keep Alpine behaviors (stage expand, cloud attach triggers)
- [ ] Manual verification checklist
  - [ ] Stage expand/collapse still works
  - [ ] All submit actions still post correct fields
  - [ ] Cloud attach modal still functions

