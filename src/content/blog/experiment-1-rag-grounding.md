---
title: 'experiment 1  RAG Grounding '
date: 2026-08-28
tags:
  - AI
pinned: false
---
Many people in the tech industry claim that giving an AI this cheat sheet stops it from "hallucinating" (making up fake facts). They assume that if the AI has the news right in front of it, it won't lie.

We decided not to just trust this claim. We wanted to actually test it to see if it was true for your specific system.

### **How We Ran the Experiment (The A/B Test)**

We picked 8 different stocks (Apple, Nvidia, Tesla, etc.) and gathered their financial math (like their P/E ratios and growth scores). Then, we asked the AI to rank and explain these 8 stocks in two different ways:

1. **Condition A ("Grounded"):** We gave the AI the financial math **PLUS** the recent news cheat sheet.
1. **Condition B ("Ungrounded"):** We gave the AI **ONLY** the financial math. We hid the news from it completely.

We kept absolutely everything else the same.

### **What We Were Looking For**

We wanted to see what the AI would do in Condition B (when it didn't have the news). Would it panic and start inventing fake news events to make its explanations sound smart? If it made things up in Condition B but was honest in Condition A, it would prove that the RAG cheat sheet stops hallucinations.

### **The Surprising Result**

The big finding was that **the AI didn't make things up in either case.**

Even when we hid the news from it, the AI was completely honest. It didn't invent fake events; it just wrote slightly boring explanations that focused only on the math.

**Why did this happen?** Because we had also given the AI a very strict system instruction: *"Do not invent any facts. Only use the data provided."* Our experiment proved that this strict rule was strong enough to keep the AI completely honest all by itself.

### **The Real Value of the News**

So, if the news cheat sheet didn't stop hallucinations (because there weren't any to begin with), was it useless?

No! While it didn't stop lies, we found it did two very important things:

1. **It made the explanations sound professional.** With the news, the AI sounded like a real Wall Street analyst citing specific world events. Without it, the AI sounded like a robot reading a spreadsheet.
1. **It changed the rankings.** As mentioned in the previous message, having the news allowed the AI to make smarter choices, like bumping Google up the list because Warren Buffett was buying it.
