This is mostly a reference for myself, of what I consider to be good and bad
habits when it comes to programming or anything software development related.

## Coding Standards

Coding Standards can have a high impact in improving the readability of an
application, by bringing every developer in the team to a common style and
standard.

As a consultant: you should identify if a team is using proper standards, and
whether each developer may be following their own style without communicating
with each other.

It can be frustrating to have to read different styles of code when we could be
having one single style.

The best way is to use an external automated tool with its own style guide.

## Patterns

Document the patterns that you are using, that you have agreed. What they are,
how they should be used, et c.

Review new patterns early; when new patterns emerge naturally, formalise them
when it makes sense, and in the case a new pattern is added intentionally, make
sure you have a team discussion about it with documentation as an output.

Ask for feedback when adding a new pattern, do a team presentation.

## Documentation

Be consistent. Besides code there are other things that are more important to
document, such as patterns, standards, practices, discussions and decisions
made.

## Dependencies

Try to avoid them as much as possible. Discuss with the team before using them,
you have to have a protocol on discussing them (it cannot be intimidating).
Ask for feedback, do a team presentation.

## Refactoring

Never expose refactoring. Never create user stories about refactoring. Never
agree to map pull requests one to one with Tickets/User Stories.

Do incremental refactoring. Agree what needs to be done and do it as that code
is being used and changed. Use Continuous Integration. Don't call these things
as separate items. These are steps between getting other things done, as part of
regular maintenance.

## Estimating

Over-estimate, over-estimate, over-estimate. Include documentation, testing,
meetings, et c. whenever estimating and commiting to deadlines.
