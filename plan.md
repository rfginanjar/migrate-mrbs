# migrate-mrbs — Technical Plan

## North Star

Add a front-end enforcement that prevents users from creating bookings that exceed 2 hours.

## Scope

- Client-side validation only (front-end lock)
- Block form submission when selected time range > 2 hours
- Show clear user-facing error message
- No backend/database changes required

## Target Files (from genealogy)

The booking form lives in:
- `web/edit_entry.php` — main booking form (59.2kb, the entry creation UI)
- `web/js/edit_entry.js.php` — JS for the booking form
- `web/edit_entry_handler.php` — form handler (for awareness, not modifying)

## Constraints

- PHP legacy codebase, jQuery-based frontend
- No build system — inline JS in `.js.php` files
- Must not break existing booking flow
- 2-hour limit applies to single bookings (not recurring series)

## Implementation Approach

1. Identify where start/end time fields are rendered in `edit_entry.php`
2. Add JS validation in `edit_entry.js.php` that calculates duration on form submit
3. If duration > 120 minutes, prevent submission and display inline error
4. Optionally disable the submit button while duration is invalid

## Out of Scope

- Backend enforcement (server-side validation)
- Admin override capability
- Configurable time limit (hardcoded 2h for now)
- Recurring booking validation
