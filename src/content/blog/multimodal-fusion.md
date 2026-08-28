---
title: Multimodal_Fusion
date: 2026-08-28
tags:
  - AI
pinned: false
---
**Numbers Don't Lie, But They Don't Tell the Whole Story Either**

**What I've Learned About Combining Data and Language in Recommendation Systems**

*Muhammad Hasham&nbsp; —&nbsp; GetInvestage Project*

There's a moment in building any AI system that touches both numbers and text where you hit the same wall: the numbers are precise but incomplete, and the text is rich but messy. A stock's P/E ratio is exact. A headline saying “investors grow cautious ahead of earnings” is not exact at all — and yet it might matter more.

This is the problem I kept running into while building GetInvestage, and it turns out it has a name, a body of research behind it, and — refreshingly — no settled answer. That last part is actually the interesting bit.

# **The Problem, Stated Plainly**

Every recommendation system, whether it's picking a stock or a product, eventually has to combine two very different kinds of information: structured data (prices, ratings, specs — things you can put in a spreadsheet) and unstructured data (reviews, news, sentiment — things you can only really represent as language). The question that decides almost everything downstream is: where in the pipeline do you combine them, and how?

There are three honest answers, and each one costs you something.

***Combine early.***

Turn the text into a number as fast as possible — run a sentiment classifier, get a score like 0.2 or -0.7, then throw that number in with your other numeric features and let one model handle everything. It's simple, and it's also lossy. The moment you collapse a sentence into a single number, you've thrown away the nuance that made the sentence worth reading in the first place. A headline like “record revenue, but management warns of a soft outlook” isn't a 0.2 or a -0.7. It's two true things at once, and squashing it into one number picks a side that wasn't really there.

***Combine late.***

Keep the numeric model and the text model completely separate, and only merge their outputs at the very end — average two scores, or let a small combiner model blend them. This preserves more of the nuance in each modality individually, but it can't learn interactions between them. It can't easily represent “this specific number, in the presence of this specific kind of news, is what actually matters” — because by the time the two signals meet, neither one knows the other exists.

***Combine in the middle.***

Keep both modalities rich — numbers as numbers, text as something closer to its original form — and let a single model reason over both together, learning which parts of the text matter for which numbers as it goes. This is where most modern LLM-based systems live now, and it's the direction I ended up building toward with GetInvestage: numbers and sentiment checking each other, rather than one collapsing into the other.

# **This Isn't a New Problem — It Just Has a New Tool**

What's interesting is that people have been trying to solve exactly this for a long time, in a domain that has nothing to do with finance: e-commerce reviews. Models like DeepCoNN and NARRE, both from around 2017–2018, were built to jointly use star ratings and review text to predict how someone would rate a product — using attention mechanisms to figure out which parts of a review actually mattered. Swap “star rating” for “P/E ratio” and “review text” for “earnings call transcript,” and you've got almost exactly the same architecture problem I was trying to solve for stocks. That's not a coincidence. Fusion is fusion, whether the object being ranked is a blender or a company.

Finance also has its own domain-specific tools for the text side. General-purpose sentiment classifiers are notoriously bad at financial language, because financial English is deceptive on purpose — “the company beat expectations despite missing guidance” reads as neutral-to-negative to a generic sentiment model, but any actual investor reads it as good news. FinBERT exists specifically because of this — a model fine-tuned on financial text so it stops getting fooled by financial phrasing.

And there's a more granular version of sentiment worth knowing about too: aspect-based sentiment analysis (ABSA). Instead of one sentiment number per article, ABSA tries to extract sentiment per topic — sentiment toward a company's earnings, separately from sentiment toward its leadership, separately from sentiment toward regulatory risk. This is basically the technical name for something I'd already started arguing informally: that flattening sentiment into one score throws away the structure that actually makes it useful.

# **Where This Has Gone Wrong Before**

It's worth being honest about the cautionary tales here too, because this space has a real history of overclaiming. Around 2011, a well-known paper argued that aggregate Twitter mood could predict stock market movements. It got a lot of attention, and a lot of follow-up scrutiny — some replications found weaker effects, some found none. It's a good example of exactly the trap I wrote about in an earlier piece on naive stock prediction: an impressive-looking correlation that doesn't hold up once you test it properly, with honest baselines and leak-free validation. Sentiment-driven prediction is not immune to the same mistakes that plain price-based prediction makes. If anything, it's more tempting to overclaim, because “the crowd's mood predicts the market” is a much better story than “we found a small, unstable correlation in one specific time window.”

# **What LLMs Actually Change**

Before LLMs, fusion mostly happened numerically — you extracted a sentiment score, then fed it alongside other numbers into a traditional model like a regression or random forest. The LLM era changes one specific thing: a language model can hold the actual text in its context and reason over it jointly with structured data at the moment it's asked a question, instead of pre-compressing everything into a number beforehand. That's the real reason a language-first approach (the kind of thing frameworks like P5 argue for) is interesting — the fusion happens inside the model's reasoning process itself, not in a separate feature-engineering step someone did earlier.

But here's the catch, and it's the one I keep coming back to: holding text in context is not the same as reasoning correctly about it. A model can have the right information sitting right in front of it and still produce an explanation that doesn't actually track that information — sounding confident and plausible while quietly not being grounded in anything real. This is exactly what I designed an experiment to test on GetInvestage: if you change one input number, does the model's explanation actually shift to match, or does it just keep sounding equally plausible regardless of what changed underneath it? Nobody has a clean, agreed-upon answer to this yet, across any domain — which, from a research standpoint, is the good news. It means there's an actual open question here, not just a known result waiting to be re-derived.

# **Where This Is Heading**

A few directions are emerging that feel worth watching, even if none of them are settled yet:

***Multi-agent systems***

Instead of one LLM doing everything, separate models specialize (one purely for sentiment extraction, one for quantitative reasoning, one for synthesizing the two), on the theory that specialization and structured disagreement between models produce more reliable outputs than one model trying to do it all at once. Early days, unproven at scale.

***Causal disentanglement***

Trying to separate sentiment that reflects real, durable information from sentiment that's just noise or hype that will reverse itself. This connects to something that seems obviously true once you say it out loud: the same piece of news means something different a week after it breaks than it does six months later, once the market has had time to actually find out if the story was true.

***Uncertainty quantification***

Getting models to express calibrated confidence instead of uniformly confident-sounding text regardless of how solid the underlying evidence actually is. Given how easily fluent language can disguise weak grounding, this feels like one of the more important open problems, not a minor add-on.

\
