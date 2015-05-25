# Add features with ease

## Overview

1. Understand the problem
2. Understand the system
3. Create acceptance-level tests for the feature
4. Plan your implementation
5. Repeat cycle from chapter 3 until acceptance-level tests pass
6. Get a code review
7. Commit

### Understand the problem
* Similar to the equivalent step in chapter 3
* More emphasis on talking to a stakeholder
* Understand the documentation before meeting the stakeholder
* If you don't have UI mockups, schedule another meeting for that explicitly

#### Roadblocks
* **Stakeholders not accessible** - Find a proxy. Change design approach to be more conservative, account for likely changes later.

### Understand the system

* Develop a _system view_ of the application. Practice _system thinking_. Consider unintended consequences of your change.

#### Questions
* What domain objects will this feature need to interact with?
* What business rules exist around those objects?
* What structures are in place to manage those objects?
* What new objects will you need to create?
* What is the test coverage like? Will you need to add test coverage anywhere before making changes?
* Are there any offline tasks related to these objects?
* Is there special reporting or auditing on the changes related to these objects, and should your new objects be part of this?
* Are there non-functional requirements to consider, such as performance or security?

Also

* Consider documenting the answers in some way, for yourself, to ensure you understand

#### Roadblocks
* **Just start coding** - Explain yourself, include team members to speed things up, avoid conflict, improve understanding

### Create acceptance-level tests
* An area to itself
* Use existing tests as reference to add your own

#### Roadblocks
* **No acceptance tests present in the system** - Add them. If that's impossible, write scripts for manual testing.
* **QA handles tests** - Request to include a QA member now, rather than later.

### Plan your implementation
* It's not _big design_. 
* Plot out a solution within the system. 
* Discard avenues that won't work.
* Discuss it with a coworker
* Resist the urge to publish your plan. It's to help you, not something public.

#### Roadblocks

* **Just start coding** - Ignore. Explain.
* **Coders block - can't create a mental plan** - Go back to previous steps to improve understanding. Start from the UI perspective. Sketch out a solution in code to understand it better.

### Repeat the test/fix/refactor cycle
* Frequently, several times an hour or more
* Work in small steps
* Work clean
  * If tests started passing, refactor for those tests
  * Refactor in the context of the entire change, not just the current TFR cycle

#### Roadblocks

* **Overwhelmed with complexity**
  * If you made a mistake, undo. Time is the only cost.
  * Take a break.
  * Take the longer way if it means you can do it in smaller steps.

### Get a code review
* Provide guidance on where to start with the review
  * Summarize feature
  * Summarize general solution approach
  * List files/classes that need reviewing
  * If unsure, start from UI and go deeper
* If you need specific feedback on specific parts, ask for it
* Respond to feedback
  * If code is wrong (bug, doesn't follow application architecture, etc.) - fix it
  * If code is confusing - take advice if any, refactor
  * Style issue - if "house style" wasn't followed, correct it. If it's a personal phylosophy of the reviewer, consider the immplications, decide who "wins".
* If feedback is going in circles, consider a "face-to-face" talk.

#### Roadblocks
* **No one to review** - General issue, far from ideal. If no other choice, use some time after writing solution to go through it again. 
* **Persistent style/approach discussions** - Re-think the change. Ask the reviewer for technical explanation. Explain yourself. If no strong arguments for or against, lean towards your own approach - you're maintaining it. If all else fails, make the changes, even if you disagree. Better to ship than "be right".

### Commit your changes
* Similar to chapter 3

## Reporting progress
* More information needed than for a simple bug
* Steps 1-4 status is "understanding the problem"
* Steps 4-5 status is "coding" or "in progress"
* Steps 6-7 status is "in review"

* No need to provide much more information than that
  