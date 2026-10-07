---
name: fatstack-tdd
description: Build behaviour test-first in red-green cycles, one vertical slice at a time, at agreed seams. Use when the user wants to build a feature or fix a bug test-first, mentions red-green or TDD, or when fatstack-implement works on a ticket.
---

# Fatstack TDD

Work in a red → green loop: write one failing test, then the least code that makes it pass, then the next test. The rules below apply on every cycle, so keep them in mind throughout, not just at the start.

Before writing tests, take the behaviour, domain terms, and decisions from the current conversation first. If the work is for a ticket, read the ticket and its linked requirements document for acceptance criteria and constraints, using the tracker operations in `docs/fatstack/tracker.md`; if there is no `tracker.md`, ask the user for the ticket's path or content. Then read the glossary and decision records (see the `## Fatstack` section in `AGENTS.md` or `CLAUDE.md`) to fill gaps. Test names use the project's domain terms. Where the conversation and a saved file disagree, ask the user which is right before writing a test that depends on it.

## Good tests

A good test checks behaviour through a public interface and says nothing about how the code inside works. It reads like a line of the requirements ("a saved basket can be shared by link") and keeps passing when the internals are rewritten, because it only cares about what callers can observe.

Read [tests.md](tests.md) for examples of good and bad tests, and [mocking.md](mocking.md) before mocking anything.

## Seams

A **seam** is the public boundary a test works through: the place where behaviour is observable without reaching inside. Tests live at seams.

Agree the seams before writing the first test. List the seams you intend to test and confirm them with the user, unless the ticket, requirements, or the conversation already agreed them. Write tests only at agreed seams. Agreeing them up front puts the testing effort on the important paths and the complex logic rather than on every detail.

Ask: "What is the public interface here, and which seams should we test?" Prefer the highest seam that exercises the behaviour, and as few seams as possible.

## Traps

- **Coupled to the implementation.** The test mocks the code's own collaborators, calls private functions, or checks results through a back door (reading the database directly instead of through the interface). Sign: a refactor that keeps behaviour the same breaks the test.
- **Tautological.** The expected value is calculated the same way the code calculates it, so the test cannot fail. Take expected values from an independent source: a literal worked out by hand, an example from the requirements, a known-good result.
- **Horizontal.** All the tests are written first, then all the code. Tests written in bulk describe imagined behaviour and lock in a structure before you understand the problem. Work in **vertical slices** instead: each test is a **tracer bullet**, and what you learn from one cycle shapes the next test.

## The loop

1. **Red.** Write one test at an agreed seam for the next small piece of behaviour. Run it and watch it fail for the reason you expect.
2. **Green.** Write only enough code to make that test pass. Leave out anything a future test might need. Run the test, then the related tests, and confirm they pass.
3. **Repeat** with the next slice until the behaviour is complete.
4. **Check everything.** Run the full test suite and the project's other checks, such as linting and type checks. The work is complete only when they all pass; if something unrelated was already failing, tell the user rather than fixing it silently.

Keep to one seam, one test, and one minimal change per cycle. Leave refactoring out of the loop: it belongs to review (`fatstack-review`), once the behaviour is in place and covered.
