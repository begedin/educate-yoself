# Fix bugs efficiently and cleanly

* TDD (test-driven development)

## Overview

1. Understand the problem
2. Write tests that fail
3. Solve the problem as quickly as you can, using tests to know you solved it
4. Modify solution for readability, conciseness and safety, using tests to make sure it still works
5. Commit

Don't forget

* Think before coding
* Understand the difference between _getting it to work_ and _getting it right_

### Understand the problem
* Don't _just start coding_
* If it's a bug, use the software to reproduce it
* If it's a small feature, use the application so it reveals the missing feature
* Talk to the people who want the feature if necessary, even if you have documentation
* Go through the source code and plot out the solution
* Resist the urge to fix everything along the way

#### Roadblocks
* **Misunderstanding the problem** - playback your understanding with the stakeholders
* **No access to production** - find someone who does, let them show you, recreate the issue in development if no other choice
* **No access to stakeholders** - talk to someone else that's close enough to a stakeholder
* **Actual emergency or business priority** - _stop the bleading_, then go through the whole process properly
* **Preasure to _just start coding_** - ignore it, explain yourself if you can't just ignore it

### Write tests that fail

* Write at the highest level posssible to connect the code with problem understanding

### Roadblocks
* **No test suite** - add it! If it's really impossible (worth the time to make it possible), then compromise
* **No test suite at the required level** - same thing, add it, compromise if **really** no other option

### Solve the problem
* Get the tests to pass as fast as you can
* "I'm done, just need to clean it up." - **don't say this**. You're making a promise you might not be able to keep. You **don't know what _cleaning it up_ actually involves**.
### Improve the solution
* An area to itself
* Common refactor tips
  * No copy and paste code
  * Descriptive variable names
  * Coding style should match remaining code in the file
* If you see a comment explaining the code, that code is probably something that needs refactoring
* **Don't overengineer and know when to quit**
### Commit
* The commit should have the proper information
  * Make the message as brief but also as specific as possible
  * Description should have information you need, but not at first glance
    * Link to ticket
    * Conversation details
    * Explanations for solution choices
    * There should be no need to explain the code
    
## Is all this proces really necessary?

Yes. In fact, it's not a lot of process. It's a simple series of steps you can follow which generally produces quality code/work/results. Give it a try. Once you get the hang of it, it's great.