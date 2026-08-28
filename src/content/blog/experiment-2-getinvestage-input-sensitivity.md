---
title: Experiment#2 Getinvestage Input_sensitivity
date: 2026-08-28
tags:
  - AI
pinned: false
---
Imagine you have hired a human financial analyst to look at some stock data and write a report. You want to make sure this analyst isn't lying, making things up, or ignoring the numbers you gave them.

The "analyst" in our case is the AI (Gemini). The experiments we just ran were designed to "audit" this AI to prove it is doing its job correctly.

Here are the three things we did:

### **1. The "Claim-Reconciliation" Tool (The Automated Proofreader)**

**The Problem:** AI models sometimes hallucinate (make things up). If our AI recommends a stock and says its price grew by 15%, we need to be 100% sure that number is real, not invented. Reading every single AI explanation by hand to check the numbers takes too long. **What we built:** We wrote a Python script (`reconciliation.py`) that acts as an automated proofreader.

- It reads the paragraph the AI wrote.
- It extracts every single number the AI mentioned.
- It checks a "ledger" (a frozen file containing the exact true data we gave the AI in the first place).
- It verifies that every number the AI claimed actually exists in the true data. **The Result:** The tool successfully proved that across 16 different explanations, the AI **never made up a single number**. It was completely honest. This specific tool is also highly relevant to your MITACS application, as Professor Higgins is trying to solve a very similar "checking claims against a ledger" problem.

### **2. The Rank-Shift Check (Did the AI overreact to the news?)**

**The Problem:** In a previous test, we asked the AI to rank stocks twice: once *with* news articles, and once *without* news articles. We noticed that giving the AI news caused it to dramatically change its rankings. For example, Microsoft dropped 4 places when we took the news away. We needed to manually check if the AI was making smart decisions, or if it was overreacting to silly headlines. **What we did:** We read the specific news articles the AI was given for Microsoft, Meta, and Google, and judged how the AI reacted to them. **The Result:**

- **Google:** The AI saw major news (Warren Buffett buying Google stock) and bumped Google up 5 ranks. This was a **smart** decision.
- **Microsoft:** The AI saw a minor, unimportant news headline about a stock chart and bumped Microsoft up 4 ranks. This was an **overreaction**.
- This tells us the AI is good at reading news, but sometimes gives too much weight to minor headlines.

### **3. The Input-Sensitivity Experiment (Is the AI actually paying attention?)**

**The Problem:** Sometimes, AI models just spit out generic, smart-sounding finance words based on a company's name. If we ask it about Apple, it might say "Apple is a great company with strong growth" even if the actual math we provided shows Apple is losing money. We needed to test if the AI is *actually* reading the specific numbers we give it. **What we did:** We played a trick on the AI. We took three stocks (Apple, Nvidia, Microsoft) and secretly changed one important number for each before giving it to the AI.

- For Apple, we took a bad number and secretly changed it to an incredibly good number.
- For Microsoft, we took a great number and secretly changed it to a terrible number. **The Result:** The AI passed with flying colors. When we gave Microsoft the fake terrible number, the AI immediately noticed, complained about the terrible number, and dropped Microsoft to the bottom of the ranking. It didn't just blindly praise Microsoft because of its name; it actually reacted to the specific data it was fed.
