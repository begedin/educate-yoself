# Deal with technical dept and slop

* Code will _rot_ and get worse, even with best efforts and intentions
* Differentiate **technical debt** (stuff that is near-impossible to avoid) and **slop** (stuff that can easily be avoided when proper process is followed)

## Slop: Code to fix NOW

* Slop is stuff that needs to get fixed in the refactor phase of bug fixing or feature implementation
* Issues that are fixable, or even quickly fixable
* **Just poorly written**
* Don't let slop happen

## Technical debt: Code to fix LATER or NEVER

* Compromises in implementation to save cost/time now, at a larger future cost later
* Case A: Code was written with assumptions. New features caused those assumptions to no longer be walid
* Case B: Code was written with consiously incorrect assmuptions. _We know we're gonna need it_, but we need to ignore it to meet deadlines.
* **Written with a trade-off**
* Document technical debt when it happens