# reliability-pack

One markdown file that gives an AI coding agent reliable working habits.

You hand [`reliability-pack.md`](reliability-pack.md) to the agent in your own environment. The agent reads it and sets up eleven always-on rules and six skills, fitted to the tools and the rules that it already has there.

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

## Why a hand-over file

The file does not hold finished skills. It holds the intent, the problem, and the parts that must hold. The agent in your environment writes the skills.

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

Limits of that test: the workspace had no ticket tracker, no wiki, and no pull requests, so those searches did not run. One session produced a correct Proof block but did not open the proof skill. The file now tells the agent to open the skill each time, and that sentence is not tested.

Test it in your own environment with one small real task before you rely on it.

## Change it

The file is plain text, so edit it before you hand it over.

- Remove rule 11 if you do not want Simplified Technical English.
- Remove a skill that you do not need. Precedent and proof are the core.
- Add your own always-on rules to the list. Keep each to one or two sentences.

## Origin

The ideas are condensed from [pstack](https://github.com/cursor/plugins/tree/main/pstack) by Lauren Tan, a much larger skill pack for rigorous AI-assisted engineering. This pack keeps a small part of it, rewrites that part in its own words, and adds the precedent check and the hand-over format. If you want a router, playbooks for each task type, and multi-model review panels, use pstack.
