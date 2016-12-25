---
title: "A Case for Database Migrations?"
author: Robert Stiff
layout: post
tags: ["databases", "SQL", "Software Development"]
date: 2016-12-24
---

## What are database migrations?

Migration is about getting from one place to another. Database migrations are about getting our database from one state to another. As we maintain and develop a database application we will want to add and remove new tables and columns, create new stored procedures and modify reference data.

SQL database engines facilitate all of these operations with imperative statements so when playing with the database in your Dec environment we issue a series of these statements to get the database in to the state we want. When we're happy that we know what we want, we write all of these statements down together (either directly in SQL or using a DSL like ruby or asp migrations) and declare them as a migration from the previous state to the new.

<!--more-->

## Why do we use them?

The natural parity between making the changes ourselves and writing a migration to make them for us is obvious.

It also fits neatly in to any change control process that demands a list of all changes prior to the change being deployed.

Thirdly in an environment where databases are maintained by developers but owned by DBAs, it gives the DBAs a perfect opportunity to review the exact changes that are being applied.

## Similarities

Managing your production environment by managing the transition between different states is reminiscent of version control, where a Version Control System keeps a record of the changes between versions, rather than of the individual states.

## Alternatives

In the opening section I mentioned that SQL database engines provide an imperative language for modifying a database's state (in contrast to the declarative language used to retrieve data). Declarative approaches are useful as they make the primary purpose of the code to communicate the desired behaviour or state, rather than how this should be achieved. Think HTML, CSS or even XML. 

The technique of database modelling (using tools such as Microsoft's database projects, Redgate's SQL Source Control or Toad Data Modeller) let us work in this way. By defining how you want your database to look rather than how it should be changed, your source code will always tell you what your database looks like at a glance. While these tools provide a UI and maybe even a visual language for describing your database, their real power is in implementing your database changes.

If you have a version controlled model of your database it becomes a complex but conceptually neat task for generate the difference between any database at version and any source code at any other version. This means that, using one of these tools will allow you to move a database easily from any one state, to any other state.

## Disadvantages of Migrations

Migrations only allow you to move one step at a time and, (due to the developer mindset of always looking forwards) can be irreversible if you are not vigilant.

Working with a long established project results in a need to apply hundred, if not thousands, of migrations to construct a new database. This can be mitigated by taking a snapshot from your production, but this means you are replicating production back in to test and dev, rather than your source code being authoritative. 

Your source code does not describe your database, only how to construct your database. Drupal patches,  for examples are code which is distributed this way; however, I have seen this cause confusion as looking at the code and the patches can never give you a full picture. Applying your migrations is mandatory to get an idea of the true nature of your database.

Because migrations must necessarily have an order, this order is usually defined by file name convention. This makes managing migrations in a very active project with Pull Requests a tiresome task. Every time a PR is merged ahead of you, you must rename your migration to ensure that it is applied after the newly merged migration. This is essentially duplicating the synchronisation work done inherently by your Version Control System.

## In Conclusion

While migrations appear to offer a solution which matches our ways of working and sometimes our change process; they could be considered counterproductive when it comes to communicating intent in our code. 

There are a great many tools available to work with migrations but it seems as if these tools are merely automating a process which becomes too unwieldy when we try to do it at any scale. They are not changing the way we work for the better but making an antipattern continue to be viable for longer, while the adoption of these tools can make it harder to change our approach in future.

While simple migration tools which work directly with SQL files or other raw data files offer us little, other migrations tools, e.g. Rails migrations, offer a layer of abstraction away from the dataset underneath. This can be used to provide portability and improve readability, do these tools can help is progress.

Fundamentally though, it appears that we need to think about describing our desired state and using tools to help us get there, as we do in many other areas of our discipline.

We should think long and hard before adopting migrations to manage a database, and we should put serious thought in to modelling as an alternative approach.
