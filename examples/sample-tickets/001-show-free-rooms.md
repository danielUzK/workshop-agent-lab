# Show which rooms are free right now

**Labels:** feature

## Context
Employees cannot see at a glance which meeting rooms are free. They walk around or ask facility staff.

## User story
As an employee, I want to see which rooms are free right now, so that I can grab one for a quick meeting.

## Acceptance criteria
- Given I open RoomBuddy, when the page loads, then I see all active rooms with "free" or "busy".
- Given a room is booked for the current time, when I look at the list, then it shows "busy until <time>".
- Given I am on a phone, when I open the page, then the list is readable without zooming.

## Open questions
- Should rooms marked "out of service" be hidden or shown greyed out?
