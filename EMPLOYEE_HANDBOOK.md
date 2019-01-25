# Bangazon Tips, Tricks, and Requirements

## The Mindset
Orientation was about seeing what can be done with Python. The Bangazon sprints are about learning how to do what you saw. They are not a test of whether you mastered the last three weeks of material. Instead, they are where and when you will find that mastery. These are not projects with a goal of finishing. They are not about perfection, or about beating a deadline. Think deep, not wide.

We did not cover everything you need to know to complete these projects. You will have to figure a lot of stuff out as a team. The requirements are vague. The pitfalls are not mapped out for you.

Support your teammates. Encourage them. Help them. Laugh with  them. You get back what you give.

**Oh, and did we mention…ask for help!**

## Prep Work
— Define standards for your team (PR rules, naming conventions, break schedules, etc)

— Read the tickets - all of them. Ask questions.

— There are no awards for pulling the most tickets. Truly choose what you estimate your team can complete.

— Sprint planning means reviewing tickets, pulling the ones you want to do, committing to them, and creating a Milestones doc with the learning team. It's not project planning. Save the architecting and whiteboarding your master plan for after the sprint planning ceremony

— Don't skimp on mapping out the relationships and the consequences of those relationships when adding and changing data - especially what it means to delete something that exists as a [foreign key](https://stackoverflow.com/questions/8042845/django-how-to-enable-foreign-keys-in-sqlite3-backend) somewhere else

But also consider not allowing certain things to be deleted:
https://github.com/makinacorpus/django-safedelete

## Setup
— Each team needs to make its own [ORGANIZATION]([https://help.github.com/articles/creating-a-new-organization-from-scratch/), and add all of your instructors as *owners*. Name your org after your group moniker.

— `.gitignore` your db. Everyone maintain their own local copy

— Make your env directory outside of your project directory. Then you don't have to gitignore it

```
my-env/
myProject/
     |__myProject/
     |__myApp/
     |__.gitignore
```

Speaking of your virtual env, you probably noticed there's no handy package.json file to enable easy dependency documentation/installation, BUT there's something close: the requirements.txt file.

For example, a teammate can activate their virtual environment and then run the following command to install the dependencies of the project:
```
pip install -r requirements.txt
```
How do you get that file? To generate the dependencies file of your project, you run the following command.
```
pip freeze > requirements.txt
```
Then you push that up with your code and anyone who pulls down the code can install the same packages and Django version. REMEMBER, document that process in your README for potential contributors to your project.

## The Workflow
— If you try to maintain migrations, and a new migration needs to happen, make sure there's only one that takes place before everyone pulls and merges the new master. Otherwise your migration timeline gets out of whack. This is another reason to just nuke them and the db from space when the schema is changed ( when models are updated )

— When you begin a task, pull the related ticket. Move it from backlog/todo to 'doing'. When you've made a PR, move it to a "ready for review" column. When it's merged, move it to done.

— The projects list is a living document. Update it when needed with new tasks or bugfixes. Update cards. Split them into smaller cards, etc, as the need arises.

— When working on a task, stay within the confines of its ticket. Make a branch named for that ticket/task. ( I like to think of a ticket as the big picture item, with the potential of having multiple project cards associated with it. But that isn't a rule. You team should define how you want to handle making project cards in relation to the issue tickets )

— Commits should be meaningful, not just a "time to save my work" mechanism. If you can't think of a clear. short commit message to describe the work you're committing, rethink why you're committing this particular batch of code.

Using `git add -p` can be super helpful in helping you laser focus your commits by allowing you to only stage the code changes you want to bundle together. If you need help using [patch add](http://codefoster.com/addpatch/), as it's called, come see an instructor.

### Definition of Done

There is a very clear [Definition of Done](https://www.agilealliance.org/glossary/definition-of-done/) that you teams should agree to and adhere to.

1. The project must be fully documented. This includes the following:
    1. Complete README that documents the steps to install the code, how to install any dependencies, or system configuration needed.
    2. Every class must be documented with purpose, author, and methods.
    3. Every method must be documented with purpose and argument list - which itself must contain a short purpose for each argument.
1. The project must be able to run fully, and without errors, on each teammate's system.
1. Fulfills every requirement of the given ticket(s).
1. Every line of code has been peer reviewed.
1. For projects that require testing, core functionality must be identified and have at least one test for each.

— Remember, when looking at the definition of done, our goal is that you learn, not finish. "Done", in this context, means a ticket or a feature; the code you want to merge to master.

Consider adding to the README as you go, supplementing with info when needed. This can be part of the definition of done when applied to a feature. Some teams have made updating the README be a condition of a thumbs up on your PR. Then somebody just needs to go in and make a final draft when you're wrapping up the sprint.

— Link to a great [Readme](https://github.com/Phat-Taupe-Pests/BangazonAPI)

-- When you submit a pull request to the project repository, it should provide all of the information necessary for one of your teammates to verify its completeness:

#### Descriptions

The description that you provide should be comprehensive enough to...

1. Provide clarity to any potentially complex code.
1. Explain reasons behind organizational or architectual decisions you made.
1. Give context to what feature you were completing so that your teammate has a mental model before looking at the code.

#### Steps to Test

You must provide clear steps for any teammate to test the code.

1. System configuration.
1. 3rd party libraries that need to be installed.
1. Command line utilities to run.
1. If there is a UI component, give clear instructions for steps to perform in the UI, and what they should expect to see as the outcome of those steps.

#### Link to Feature Ticket

At the end of the PR description, you must provide a hyperlink to the ticket that contains a description of the feature you are working on.

### Daily Stand Up
-- Every morning, your manager ( one of your instructors ) will attend a 15 minute meeting with your entire team so that you may discuss progress that you are making, provide guidance on what you plan to complete in the next 24 hours, and raise concerns about any obstacles that you are encountering.

Your manager is responsible for making sure you have the resources that you need to complete the work, and it also responsible for removing any obstacles in your way.

### Retrospective
-- Once your team completes a sprint your team, along with your manager, will conduct a [Retrospective](https://www.mountaingoatsoftware.com/agile/scrum/sprint-retrospective). Be sure to read the description of the retrospective before attending your first one.

_Have fun! Enjoy the ride!_

> If you think of other items to add to this list as you go through the sprints, I would love to hear them. Future NSSers will thank you!
