---
layout: post
title: Intro to LLM's
---

The large language models (LLM's) revolution is currently led by OpenAI, and 
rightfully so. It was their release of ChatGPT that really gave us a sneak peek
at how interactions between humans and computers will be in the future.

Up and until recently, we (humans) gave computers instructions in a way that
was easy for computers to understand. This was inefficient and burdensome;
something we could express in a sentence translated to a computer-friendly
language would take several instructions. Now computers have come to meet us 
where we are: they understand our messy and nuanced natural language.

The architecture behind these models is relatively simple yet powerful. The
transformer (i.e., the name of the neural network) was originally conceived by 
Google as a way to produce language-to-language translations. This is a very
difficult problem to solve because some words have different meanings depending 
on the context. For example, the word "bank" a place where we deposit our money,
or the edge of a river. To solve this problem, researchers made the model
"aware" of the context by adding a layer to the network called the
"self-attention". This was the real breakthrough that made computers capable of
understanding our language better and producing accurate translations.

Here's the crazy part: when you ask any question to an LLM, the output it is
just one token (a fragment of a word). Then that token is added back to our
original question, and sent again to produce the next token. To illustrate 
this, let's say we ask ChatGPT "Who was the first president?".

1. **Prompt**: "Who was the first president?" → **Output**: "*The*"
2. "Who was the first president? *The*" → " *first*"
3. "Who was the first president? *The first*" → " *president*"
4. "Who was the first president? *The first president*" → " *was*"
5. "Who was the first president? *The first president was*" → " *George*"
6. "Who was the first president? *The first president was George*" → " *Washington*"
7. "Who was the first president? *The first president was George Washington*" → "\<end\>"
8. **Response**: "*The first president was George Washington*"

If you have played with ChatGPT, you may have noticed that the same question
doesn't get the same answer. The is because for every input, there are several
possible tokens that can be returned with different probabilities. Let's say 
that for our original question the token "The" had a probability of 40%, the 
token "George" had 20%, and so on. The language model would pick a random token
respecting the probabilities of each. That means that if we ask the same 
question a 100 times, we would expect the first token to be "The" on 40 out of 
the 100 answers. This process is repeated for every token, and because almost
every answer is made of lots tokens, it is almost impossible to have the same 
answer twice.

These probabilities are calculated during the training phase. The idea
is that the model is given fragments of text and asked to predict the next 
token, which is then compared to the next token in the example that we gave. The
closer the output is to the example, the less adjustments made to the network
weights (a fancy word for numbers). Most of the current models are trained with
a dataset called "The Pile", which is 800GB of text scrapped from the web.
Training is slow and expensive, it is estimated that it took more than a month
(of GPU time) to train ChatGPT on top-of-the-line hardware.

Once the training phase is completed, you end up with a model capable of 
generating an output for any given input. However, most of its output is not
super useful, as the model may not follow instructions, say things that are 
incorrect, ramble, hallucinate, etc. To make the model ready for use it goes 
through a phase called fine-tuning. It is at this stage that the model is 
"aligned" to the types of behaviors we expect, which also includes making the
model less biased and "safe".

There you have it, a simple idea that will change the way we interact with
computers forever.