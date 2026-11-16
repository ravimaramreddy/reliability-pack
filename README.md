# reliability-pack

One markdown file that gives an AI coding agent reliable working habits.

You hand [`reliability-pack.md`](reliability-pack.md) to the agent in your own environment. The agent reads it and sets up eleven always-on rules and six skills, fitted to the tools and the rules that it already has there.

The ideas are taken from [pstack](https://github.com/cursor/plugins/tree/main/pstack). This repo distills them, generalizes them for any agent, and extends them. The sections below show how.

## The problem

Coding agents are capable but not consistent.

- Sometimes the agent reads how a kind of work was done before. Sometimes it starts from zero and invents a process.
- Sometimes it proves a result. Sometimes it only says "done".

In most teams, the reliable path already exists in the record. A past pull request, a commit, a ticket, or a wiki page shows the local process, and each repo can have a different one. An agent that reads that record first makes fewer wrong moves. An agent that shows its evidence lets you trust the result without a second check.

The pack makes both habits happen on every task, and it makes a skipped check visible.

## How it works

```
 your task
     |
     v
 restate the goal ---> precedent check ---> the work ---> proof ---> "done"
                        |    ^                              |
                        |    | read first                   | Proof block:
     Precedent block: <-+    |                              | check, result,
     past work, steps,    lessons file <--- you correct or  | labelled claims
     differences,                           confirm the agent
     where it searched
```

The first reply on a task shows a **Precedent block**. The reply that says "done" shows a **Proof block**. If a block is missing, the check was skipped, and you can see that at once.

## What the pack sets up

### Always-on rules

| # | Rule | In short |
|---|---|---|
| 1 | Precedent before action | Find how this was done here before, and show what you found. |
| 2 | Proof before "done" | Show evidence from the real target. |
| 3 | Restate first | Restate the goal and name the smallest change that meets it. |
| 4 | Smallest change | Ship only what the evidence supports. No extras that "might help". |
| 5 | Bugs | Reproduce first, fix the first place that goes wrong, run the same reproduction again. |
| 6 | Look, then ask | Answer a question by running or reading when possible. Ask for preferences, approvals, and steps that cannot be undone. |
| 7 | Say no | When the record or the evidence shows that the request is wrong, show it first. |
| 8 | Two failed fixes | After two failed fixes with one shared assumption, test the assumption. |
| 9 | Pull requests | Each pull request names the past work that it follows. |
| 10 | Lessons | Offer to save a lesson after a correction or a confirmed approach. |
| 11 | Reply style | Replies in ASD-STE100 Simplified Technical English. Remove this rule if you do not want it. |

### Six skills

| Skill | When it runs | What it does |
|---|---|---|
| precedent | every task | Searches saved lessons, repo instructions, merged pull requests and their review comments, file history, tickets, and wiki pages. Follows the closest matches and cites them. |
| proof | every task | Chooses a check that can fail, runs it on the real target, shows the output, and labels each claim as measured, inferred, or guess. Has rules for numbers: same conditions, run count, spread. |
| break-it review | on request | A fresh agent attacks the change in its own copy. A finding stays only with a command and its output. Report only, no fixes. |
| lessons | on offer | Saves a correction or a confirmed approach to one lessons file, only on your yes. A repeated correction moves to a stronger place, such as a check that fails. |
| blast radius | on request | Finds what a change can break outside the lines that it touches. Names the single reason why the change is safe, and proves it. |
| pause and pickup | on request | Parks a task with a resume note. A later session continues from the note and does not repeat proven work. |

## Use

1. Open [`reliability-pack.md`](reliability-pack.md) and copy it. The **Raw** button gives the plain text.
2. Save it in the folder where you start your agent sessions, or paste it into the chat.
3. Say: `Read this file and set it up here.`
4. If the agent asks where to install, answer. That is the only question to expect.
5. Read its report: what it created, what it changed or moved, and what it could not do.
6. Start a new session. New skills usually load only in a new session.

After that, precedent and proof need no command. The other skills start on a request in plain words, for example "do a break-it review", "what is the blast radius", "pause this", or "pick up the login task".

## What you see afterwards

A first reply on a task looks like this:

```
Goal: give the audit team read access to the payments resource.
The smallest change is one new reader line, plus the log entry and the
version bump that each past access change also had.

Precedent
- Closest: 9a7b88c "Give analytics team read access to invoices"
           73c0223 "Give reporting team read access to orders"
- Steps they followed: 1. add the team to the access list
  2. bump the config version  3. add a line to the access log
- My plan differs: the approval column. I cannot open the ticket,
  so I wrote "PENDING" and did not commit.
- Searched: lessons (empty), repo instructions, pull requests (none),
  commit history (2 matches), tickets (not reachable)
```

The reply that says "done" ends like this:

```
Proof
- Check: ./check.sh on the task branch -> "OK", exit 0 (measured)
- Can it fail: yes. With the log line removed, it printed "FAIL",
  exit 1 (measured)
- The diff touches the same three files as the past changes (measured)
- Not verified: the approval in the ticket. The tracker is not reachable.
```

In that example, the version bump is in no document and no check. Only the commit history shows it. The agent found the step because it read the record first.

## The ideas behind it

pstack is a large skill pack for careful engineering with AI agents. It has one entry skill that routes a task to a playbook, more than twenty playbooks, two dozen named principles, and many helper skills. Under the detail, it rests on a small set of ideas. This is a distillation of them.

| # | Idea | In one or two sentences | In this pack |
|---|---|---|---|
| 1 | Depth before speed | More output is not the goal. Write less, and be able to defend each line. | rules 3, 4 |
| 2 | Evidence, not assertion | "It compiles" is not evidence. Say what "done" means as checks, run them on the real thing, and show the output. A check that you never saw fail proves little. Vet a number before you trust it. | rule 2, proof |
| 3 | Reproduce, then find the cause | Reproduce a bug before you touch it. Fix the first place where it goes wrong, not the place where it shows. When two fixes fail on one assumption, the assumption is the suspect. | rules 5, 8 |
| 4 | The record explains why | Answer "why is it built this way" from the commits and the review discussion, not from a reading of the code. Mark what you inferred, and name the gaps in the record. | precedent |
| 5 | Plan through code | Do not take the first design. Settle open questions with throwaway prototypes and independent attempts, and compare what you can see. Write the plan last. | add-on |
| 6 | Ask only what a run cannot answer | Work that can be undone goes ahead. A person decides preferences and steps that cannot be undone. Honest disagreement is better than easy agreement. | rules 6, 7 |
| 7 | Independent eyes | The one that judges a change is not the one that wrote it. Different reviewers find different failures, and only a finding that a second run confirms stays. The lead still owns each helper's work. | break-it review, add-on |
| 8 | Structure beats reminders | When you give the same correction twice, text has failed. Move the rule into the design, the type system, an automatic check, or a test, and use documentation last. | lessons |
| 9 | Build the tool | For work that is not trivial, write the script that does it or checks it. A reviewer can run a script again. A reviewer cannot rerun hand edits. A repeatable way to exercise the real application is worth keeping. | add-on |
| 10 | Unattended work must earn trust | A long run needs a finish condition that can pass or fail, work cut into small pieces with a check after each piece, its own workspace, and a decision log that a different reviewer audits. | pause and pickup, add-on |
| 11 | A router and shared words | One entry point matches the task to a named playbook and keeps a skipped step visible, with its reason. Short principle names let one phrase steer the agent. | replaced by the always-on rules |

## How this pack generalizes the ideas

pstack is built for one tool, for code, and for a person who types commands. This pack changes four things so that the ideas work in any environment.

1. **From one tool to any agent.** The file names no product, no model, and no tool. The agent maps "pull requests", "tickets", and "wiki" to the tools that it has, and writes those names into the skills.
2. **From finished skills to intent.** The file holds the problem and the parts that must hold. The agent writes the skills and fits them to the rules that are already there. A finished skill from another environment fits badly. An intent travels.
3. **From commands to habits with a visible output.** A skill that you must remember to call is used sometimes. The two core checks run on every task, and each one leaves a block in the reply. A missing block is a visible skip.
4. **From code to every change that has a record.** Configuration, permissions, infrastructure, data, and documents have a history too. The checks apply to all of them: a dry run before, a read-back after, a count before and after.

## What this pack adds

- **Precedent on every task.** pstack reads the record to answer "why". This pack reads it before every change and every "how do we do X here" question, in a fixed order, and treats "none found, searched here" as a valid result.
- **A lessons file that learns from praise.** A confirmed approach is saved as well as a correction, only on your yes. The precedent check reads that file first.
- **Visible skips.** The Precedent block and the Proof block are the contract. You do not have to trust that a check ran.
- **The hand-over format.** One file sets up everything, asks at most one question, keeps your current rules, and reports what it changed.

## Extend it

A new habit works when it has three parts. Write those three parts and add them to the file.

1. **A trigger.** When does it run: on every task, on a request, or on an event such as a correction?
2. **What must hold.** Three to eight short points. Leave the wording and the tools to the agent.
3. **A visible output.** A block, a file, or a line in the reply that shows that it ran.

These ideas from the distillation are not in the pack. Each one is ready to paste.

| Add-on | Kind | Text |
|---|---|---|
| Helpers | always-on rule | You own the work of each helper agent. Read its diff and its evidence yourself. Do not give me its summary as a fact. |
| Finish condition | always-on rule | A long or unattended run starts with a finish condition that can pass or fail. A time limit does not count as one. Do not relax the condition to finish. |
| Small units | always-on rule | Split multi-step work into small units. Each unit ends in a check. Run the check before you start the next unit. |
| Prototype first | always-on rule | When a choice depends on how something behaves, build a throwaway version and observe it. Show me the options with what you saw. |
| Bulk changes | always-on rule | For a change that repeats in many places, write a small script that makes the change or checks it, and show me the script. |
| Verification recipe | point for the proof skill | When no check exists for a kind of change, find a way to exercise the real thing. Then write the recipe where the next session will find it: how to start it, how to drive it, what output proves it, and how to clean up. |
| Decision trail | skill, for long runs | Keep one log, append only, with one row for each decision: time, decision, reason, a pointer to the evidence, and the result. At the end, a fresh agent compares the log with what really happened and lists what needs my attention. |
| Design contest | skill, on request | For a design that is costly to reverse, make two or three independent attempts in separate copies. Choose one as the base, move the best parts of the others into it by hand, and verify the result. If the attempts agree, the agreement is the answer. |

You can also make the pack smaller.

- Remove rule 11 if you do not want Simplified Technical English.
- Remove a skill that you do not need. Precedent and proof are the core.

## Why a hand-over file

- **The agent knows your environment better than this file can.** It sees your instruction file, your tools, your task list, and your repo conventions. It names the real tools in the skills and merges the rules with the ones that you already have.
- **One file is easy to move.** You can copy it to a machine where you cannot install anything.
- **Markdown only.** No scripts, hooks, plugins, or downloads. The file tells the agent to add none.
- **Your current rules stay.** The agent can merge or reword a rule to make the set coherent, but it must not drop one silently, and it reports each change.

## How it was tested

The file was given to a fresh agent session in a scratch workspace with a small instruction file and a demo repo. The history of the demo repo held one process step that no document and no check mentioned.

- The install session wrote the six skills with real tool names and working searches. It merged three existing rules and removed none.
- A second fresh session got a task in the demo repo. It ran the precedent check without a command and followed the hidden step. It did not invent an approval that it could not verify. It stopped and asked.
- Its proof showed that the check can fail, not only that it passed.
- Each on-request skill loaded on a plain-words request. Praise produced a lesson offer, and the lesson was saved only after a yes.

Limits of that test: the workspace had no ticket tracker, no wiki, and no pull requests, so those searches did not run. One session produced a correct Proof block but did not open the proof skill. The file now tells the agent to open the skill each time, and that sentence is not tested. The add-ons in the table above are not tested.

Test it in your own environment with one small real task before you rely on it.
