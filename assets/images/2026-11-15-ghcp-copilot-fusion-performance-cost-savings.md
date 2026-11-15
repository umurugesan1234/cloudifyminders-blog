---
title: "GHCP Copilot Fusion: Performance & Cost Savings"
date: 2026-11-15
categories:
  - GitHub
  - AI
tags:
  - GitHub Copilot
  - Copilot Fusion
  - AI Coding Agents
  - Developer Productivity
  - Benchmarks
  - Cost Optimization
author: Murugesan U
layout: single
toc: true
toc_sticky: true
---

# GHCP Copilot Fusion: Performance & Cost Savings

*A practitioner's reading of benchmarks.github.com, by Murugesan U*

In the last few weeks, many customers and colleagues have been asking me the same question: "What exactly is Copilot Fusion? Is it a new model?" GitHub has now published benchmark results for Fusion in GitHub Copilot (GHCP), so I thought I'd sit down, go through the numbers properly and put my understanding in simple words. I'll look at two things here: how well Fusion performs, and what it costs to get that result.

## First, what is Copilot Fusion?

Fusion is not a new model, and it is not the agent harness either. The easiest way to think about it is as a smart layer sitting between the two.

- At the bottom is the Copilot agent harness, the part that reads your repo, runs commands, edits files and runs tests.

- In the middle are the models: Claude, GPT, Grok and others.

- On top sits Fusion, which decides which model or models should handle a task and how they should work together.

Fusion works in three patterns. In **Single**, one model handles the task end to end. In **Cascade**, a cheaper model tries first and the task moves up to a stronger model only when needed. In **Critique**, one model does the work and another reviews it before the answer is final.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-01.png)

You can also pick an effort level (Standard, High or Max) depending on how much you want to spend for quality. Fusion became generally available on 28 October 2026 and came out of an internal effort called Project HydraFusion.

### Which effort level should you pick?

- Fusion Standard: for everyday work. It balances cost, quality and speed, and picks one model for each task. On DeepSWE it scored 69.2% at $0.61 per task.

- Fusion High: for substantial, well-scoped coding tasks, like fixing a complex bug or making a change across several files, where the extra model calls are worth the extra time and AI credits. On DeepSWE it scored 75.4% at $0.87 per task.

- Fusion Max: for the hardest tasks that need the deepest reasoning and most thorough analysis. On DeepSWE it scored 78.1% at $4.97 per task.

My simple rule: start with Standard, move to High when the task touches many files or the bug is tricky, and keep Max for the really hard problems. Going from Standard to High gave about 6.2 points for roughly 1.4 times the cost, while going from High to Max gave about 2.7 points for roughly 5.7 times the cost. So High is the sweet spot for most teams. If you don't see Fusion Max in the model picker, your admin may have disabled one of the models it needs.

## How was it tested?

GitHub tested it on two public benchmarks, and both are quite tough.

### DeepSWE 1.1 (built by Datacurve)

- 113 real software engineering tasks taken from 91 open-source repositories

- Languages covered: TypeScript, Go, Python, JavaScript and Rust

- Each task has a hand-written verifier, so the agent cannot game the result

- The agent runs in an offline sandbox with a 2.5-hour limit per task

### Terminal-Bench 4.0 (Laude Institute and Stanford)

- 66 hard command-line tasks across 7 domains

- Each task is simply pass or fail

- Time limit of up to 8 hours per task

One important point is that every configuration was run on the same Copilot harness. So the comparison is really about the model or routing choice, not about one tool being better engineered than another. Each configuration was run 5 times, with up to 2 retries per task.

## Cost savings at a glance

Here is the short version before we get into the full tables.

- DeepSWE 1.1, Fusion High vs Opus 5.5 xhigh: 0.9 points higher and about 82% cheaper per task ($0.87 vs $4.92).

- DeepSWE 1.1, Fusion Max vs Opus 5.5 Max: 8.0 points higher and about 54% cheaper per task ($4.97 vs $10.89).

- Terminal-Bench 4.0, Fusion Max vs Opus 5.5 Max: 5.5 points higher and about 36% cheaper per task ($7.53 vs $11.71).

- The other side: GPT-6.1 Sol is still cheaper than Fusion High. Fusion High costs about 1.6 times more on DeepSWE and 2.35 times more on Terminal-Bench, for a gain of only 1 to 2 points.

## Performance & cost: the numbers

### DeepSWE 1.1

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-02.png)

| Configuration | Pass rate | Cost per task |
| --- | --- | --- |
| Fusion Max | 78.1% | $4.97 |
| Fusion High | 75.4% | $0.87 |
| Claude Opus 5.5 (xhigh) | 74.5% | $4.92 |
| GPT-6.1 Sol (medium) | 74.3% | $0.54 |
| GPT-6 Astra (medium) | 73.8% | $3.20 |
| Claude Opus 5.5 (max) | 70.1% | $10.89 |
| Fusion Standard | 69.2% | $0.61 |
| Grok 4.7 (medium) | 69.0% | $5.88 |

### Terminal-Bench 4.0 (top entries)

| Configuration | Pass rate | Cost per task |
| --- | --- | --- |
| Fusion Max | 67.9% | $7.53 |
| Claude Opus 5.5 (max) | 62.4% | $11.71 |
| Claude Opus 5.5 (xhigh) | 61.5% | $6.74 |
| Claude Opus 5.5 (high) | 57.6% | $4.10 |
| Fable 5.1 (max) | 55.1% | $16.39 |
| Fusion High | 53.0% | $2.54 |
| GPT-6.1 Sol (max) | 50.9% | $1.08 |

## What stood out for me

### 1. Same quality, much lower cost

On DeepSWE, Fusion High scored 75.4% against 74.5% for Opus 5.5 xhigh, which is practically the same. But the cost is $0.87 per task against $4.92, roughly 82% cheaper. For teams running agents at scale, this matters much more than the top score.

### 2. Fusion Max leads both boards

On DeepSWE it is 8 percentage points ahead of Opus 5.5 Max (78.1% vs 70.1%) at about 54% lower cost. On Terminal-Bench it is 5.5 points ahead of Opus 5.5 Max (67.9% vs 62.4%) at about 36% lower cost.

### 3. It is not a clean sweep, and it is good to be honest about that

On DeepSWE, GPT-6.1 Sol medium gets 74.3% at just $0.54, so Fusion High is only 1.1 points better while costing about 1.6 times more. On Terminal-Bench, Opus 5.5 xhigh is a bit cheaper than Fusion Max ($6.74 vs $7.53), though Fusion scores higher. And Fusion High on Terminal-Bench is 2.1 points ahead of GPT-6.1 Sol max but costs about 2.35 times more.

So the story is not "Fusion wins everything". It is more that Fusion gives you a good default without you having to pick the right model for every task yourself.

### 4. Independent results are a little different

Artificial Analysis evaluated Fusion separately in its Coding Agent Index v2.0, as reported by XenoSpectrum. There, Fusion Max scored 60 against 62 for Claude Code with Opus 5.5 Max, at roughly half the cost per task ($5.59 vs $11.42). Fusion Max also scored 71 on Terminal-Bench 4.0 in that evaluation. Different harness and a different setup, but the cost story still holds.

## How to use Fusion: VS Code, CLI and GHCP app

Fusion is available in VS Code, the GitHub Copilot CLI and the GitHub Copilot app. In all three the idea is the same: open the model picker, pick Copilot Fusion and choose an effort level. Here is how it looks in each, with screenshots from my own setup.

### In VS Code

Step 1: Open VS Code with an existing project or a new one, and open the Copilot Chat window.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-03.png)

Step 2: In the chat window, click the model picker and select Copilot Fusion.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-04.png)

Step 3: Choose the effort level: Standard, High or Max. Standard is selected by default. In my setup Max is greyed out; as mentioned earlier, this happens when an admin has disabled one of the models Fusion Max needs.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-05.png)

### In the Copilot CLI

Step 1: Open a terminal or command prompt in your project folder.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-06.png)

Step 2: Type copilot and press Enter to launch the GitHub Copilot CLI.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-07.png)

Step 3: In the chat, type the /model command.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-08.png)

Step 4: Press Enter to see all the models, with Copilot Fusion at the top. Use the left and right arrow keys to choose the effort level (Standard, High or Max), then press Enter.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-09.png)

Tip: for a one-off command you can skip the picker and run: copilot --model fusion --fusion-tier high -p "your prompt". To make Fusion your default for future sessions, use /config model.

### In the GitHub Copilot app

Step 1: Open the GitHub Copilot app.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-10.png)

Step 2: Click the model shown below the prompt box, then switch on the Copilot Fusion toggle.

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-11.png)

Step 3: Choose the effort level: Standard, High or Max. The model shown below the prompt box then changes to Copilot Fusion · Standard (or whichever level you picked).

![GitHub Copilot Fusion screenshot](/assets/images/copilot-fusion/copilot-fusion-12.png)

If you tried the earlier research preview, you may still see a "HydraFusion" toggle under Settings > Agents. After updating the app, just use the Copilot Fusion switch in the model picker; your preview choice moves across automatically.

## A few things to keep in mind

- Compare scores only within the same benchmark. GitHub's own page says this clearly; 70% on DeepSWE and 70% on Terminal-Bench do not mean the same thing.

- Small gaps may not mean much. GitHub notes that similar scores may not be a meaningful difference, so I would treat a gap of under 1 point as a tie.

- "Cost" here is the per-task cost shown on the benchmark page, that is, what the agent spends on model usage to finish one task. It is not your GitHub Copilot licence or subscription price, so please don't read these savings as a change in your Copilot bill.

- Fusion is not on the official Terminal-Bench leaderboard at tbench.ai. The Terminal-Bench numbers above come from GitHub's own runs on the same harness.

## My take

For most of us, choosing between models is becoming a bit of a headache. Every month there is a new model, a new effort setting and a new price. What Fusion is really trying to do is take that decision off the developer's plate, and on these two benchmarks it does that well, especially on cost.

My suggestion is not to switch blindly based on a leaderboard. Try Fusion High on your own repos for a week or two, keep an eye on the cost and the pull-request quality, and then decide. Benchmarks are a good starting point, but your own codebase is the real test.

Would love to hear from those who have already tried it. Is it working for your team?

## References

- GitHub Copilot benchmarks (DeepSWE 1.1 and Terminal-Bench 4.0 tabs): https://benchmarks.github.com/

- Terminal-Bench official site and leaderboard: https://www.tbench.ai/

- Artificial Analysis Coding Agent Index v2.0, as reported by XenoSpectrum (xenospectrum.com)

*Views are my own. All figures are from the sources above as of 15 November 2026. Percentage-point gaps and cost savings are my own calculations from the published numbers.*
