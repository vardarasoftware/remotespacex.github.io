---
layout: post
title: 'Does "Done" From an AI Coding Agent Mean the Work Is Saved?'
description: '"Done" from an AI coding agent means it finished the work — not that the work is saved anywhere you can reach. What happened when ours reported a rebuild complete, and what we actually do about it.'
date: 2026-10-27
last_modified_at: 2026-10-27
author: Mahendra Singh
tags: [Engineering, AI Agents]
---

No. "Done" from an AI coding agent means it finished the work. It does not mean the work is anywhere you can reach. On 23 October ours reported a homepage rebuild complete — 12 files, 1,369 lines added, 415 removed, the branch and the commit named, every detail accurate. The only copy of it was sitting inside an execution workspace. A person noticed five minutes later. Nothing in our system noticed at all. Here is what that gap looked like, why it opened, and what we actually do about it.

## What happened when our agent said "done"?

On 23 October one of our coding agents was rebuilding the Vardara homepage — the editorial rebuild that went with our change of position, the same one we wrote up in [Is Ruby on Rails Still the Right Choice for Your MVP?](/blog/why-a-startup-choose-ruby-on-rails-to-build-mvp/). It committed at 17:08:53 UTC. At 17:11:12 it reported the task complete.

The report was good. It named the branch, `feat/editorial-lab-homepage`, and it named the commit. It listed 12 files, 1,369 lines added and 415 removed. It described the screenshots it had taken and the overflow pass it had run at the narrowest width we support. Every one of those details was accurate — I checked them all afterwards against the record. Nothing was padded and nothing was wrong.

At 17:16:32 someone opened the remote to review the branch the report had named, and the branch was not there. It had never been pushed. They pushed it by hand. The pull request opened at 17:21:32 and merged at 17:27:10. The work is live; you are reading a site that contains it.

So we lost nothing, and I would rather say that plainly than dress a near-miss as a disaster. The near-miss is the more useful story anyway. For roughly five minutes, the only copy of 1,369 lines of finished work existed in one place that nothing outside it could see — while the record said the task was complete. The report and the reality disagreed, and the only reason anyone found out is that a person went looking for a branch by hand.

## Why did "done" and "saved" come apart?

Because the contract was underspecified. This is the part that generalises, so read it closely.

On the morning of 23 October, our engineering agents were working to a standing instruction that said: *"Deliver every change as a git branch plus the diff posted on your task, so a person can review and push it."*

Read it again. It asks for a branch. It does not say where the branch has to exist, and it does not ask the agent to push anything — pushing is something a person does afterwards. The agent followed it exactly. It produced a branch, posted the diff, and reported what it had done.

When a person reads "deliver a branch" they picture a remote, because that is where their branches live. `git push` is so automatic in a human workflow that it has stopped registering as a step. An agent inside a container has no such habit. It had a branch. The instruction was satisfied.

This is not a story about agent reliability. The agent was reliable. It is a story about an unstated locative in a contract, written by someone who could not imagine needing to state it. Those are hard to find by reading, because the person who wrote the sentence reads it the same way every time.

## What check do we actually run?

One sentence. It reads, in full:

> Branch pushed to `origin` and verified with `git ls-remote` before this task closes. Agent workspaces are ephemeral; an unpushed branch is lost work.

That sentence lives in a task description, and in the hands of whoever reads it. That is the whole control. No hook in our harness fires when a task closes — the hooks directory is empty, I looked. Nothing refuses a completion report for naming a branch that does not exist.

It works, as far as it goes. On the next task that carried the sentence, the engineering agent reported back that it had verified the branch with `git ls-remote` — exactly the behaviour we wanted. But it complied because it was asked to, not because anything would have stopped it otherwise. An instruction is not a check. It is a request with a good track record.

Here is the part that does not flatter us. Across the engineering tasks raised after that morning, the requirement appears in four wordings — five tasks, four phrasings, one with no push requirement at all, inside four days. Nobody was careless; the sentence was retyped by people working quickly, each time in whatever shape the task seemed to need.

Worse, it has drifted inside the document I would have called safe. The instruction I quoted lives in our canonical standing document — the thing you read when you are unsure. Three days after that morning someone appended a workflow section to the bottom of it. One line reads: *"Raise PR against `master` branch and push."* Both sentences are live. One has a person pushing the branch afterwards; the other tells the agent to. The same edit refreshed the paragraph above and passed straight over the one in between. Not inattention — a true, useful line added without rereading what the paragraph above already promised.

That is not a fifth wording. It is a contradiction, in the artefact a reader would take as authoritative, unnoticed until someone went looking this week. Prose controls do not rot by being ignored. They rot by being added to.

## Can an AI coding agent lose finished work?

I can tell you what we documented, and I would rather give you that than the confident version.

What we documented is the state that precedes a loss: finished work whose only copy was somewhere nothing else could see, reported as complete, with nothing in the system aware of the difference. What we did not document is a loss. The branch was caught. We have never measured how long an execution workspace survives, so I am not going to tell you how close it was — that estimate would be the exact kind of claim this post is about.

One incident is also not a rate. I am not going to hand you a percentage of agent tasks that lose work, because we have one data point and it went the right way. What generalises is not a frequency. It is the shape: a report that is accurate about the work and silent about where the work is.

## What would make this an actual check?

Something that does not exist. The shape of it is not hard to describe: when a task closes, read the branch name out of the completion report, run `git ls-remote` against the remote, and refuse the close if the branch is not there. A report that cannot be corroborated does not get to stand as a report.

We have not built it, and the reasons are dull. The completion names the branch in prose rather than in a typed field, so something has to parse an English sentence to find it. The agent works from a local clone, so the check has to run somewhere that can reach the remote with credentials. And the plumbing between our task system and the harness is not connected in the direction this would need. None of that is a defence. It is where we are today.

If you are running coding agents on real work, the thing worth doing this week is not adopting our sentence. It is finding out what your own version of it is — and then finding out how many wordings it has quietly turned into.
