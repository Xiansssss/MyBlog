title: Some remarks on Large Language Models
date: 2023-6-3
tags:

- LLM

categories: 
- LLM

---

# Some remarks on Large Language Models

#### Yoav Goldberg, January 2023

**Audience:** I assume you heard of chatGPT, maybe played with it a little, and was imressed by it (or tried very hard not to be). And that you also heard that it is "a large language model". And maybe that it "solved natural language understanding". Here is a short personal perspective of my thoughts of this (and similar) models, and where we stand with respect to language understanding.

## Intro

Around 2014-2017, right within the rise of neural-network based methods for NLP, I was giving a semi-academic-semi-popsci lecture, revolving around the story that **achieving perfect language modeling is equivalent to being as intelligent as a human**. Somewhere around the same time I was also asked in an academic panel "what would you do if you were given infinite compute and no need to worry about labour costs" to which I cockily responded **"I would train a really huge language model, just to show that it doesn't solve everything!"**. Well, this response aged badly! or did it? And how does it co-exist with my perfect-language-modeling-as-intelligence story I was also telling at the same time?

## Perfect Language Modeling is AI-Complete

My pop-sci-meets-intro-NLP talk ("teaching computers to understand language") was centered around Claude Shannon's "guessing game" and the idea of language modeling. It started with AI-for-games, then quickly switched to "a different kind of game" invented by Shannon in 1951: the game of "guessing the next letter". The game operator chooses some text and a cutting point within the text, and hides the end. The players need to guess the first hidden letter in the smallest number of gueses. 

I gave a few examples of this game, that demonstrate various kinds of linguistic knowledge that are needed in order to perform well in it, at different levels of linguistic understanding (from morphology through various levels of syntax, smantics, pragmatics and sociolinguistics).
And then I said that humans are great at this game without even practicing, and that's it is hard for them to get better at it, which is why they find it to be a not-great game. 

I then said that computers kinda suck at this game compared to humans, but that by teaching them to play it, we gain a lot of implicit knowledge of language. And that there is a long way to go, but that there was some steady progress: this is how machine translation works today! 

I also said that computers are still not very good, and that this is understandable: the game is "AI-complete": really playing the game "at human level" would mean solving every other problem of AI, and exhibiting human-like intelligence. To see why this is true, consider that the game entails completing *any* text prefix, including very long ones, including dialogs, including every possible conversation prefix, including every description of experience that can be expressed in human language, including every answer to every question that can be asked on any topic or situation, including advanced mathmatics, including philosophy, and so on. In short, to play it well, you need to *understand* the text, understand the situation described in the text, *imagine* yourself in the situation, and then to *respond*. It really mimicks the human experience and thought. (Yes, there could be several objections to this argument, for example humans may also need to ask questions about images or scenes or other perceptual inputs that the model cannot see. But I think you get the point.)

So that was the the story I told about Shannon's guessing game (aka "language modeling") and how playing it at human level entails human level intelligence.

## Building a large language model won't solve everything / anything

Now, if obtaining the ability of perfect language modeling entails intelligence ("AI-complete"), why did I maintain that building the largest possible language model won't "solve everything"? and was I wrong? 

The answer is that I didn't think building a very large language model based on the then-existing tech (which was then just shifting between RNNs/LSTMs and the Transformer) will get us nowhere even close to having "perfect language modeling".

Was I wrong? sort of. I was definitely surprised by the abilities demonstrated by large language models. There turned out to be a phase shift somewhere between 60B parameters and 175B parameters, that made language models super impressive. They do a lot more than what I thought a language model trained on text and based on RNNs/LSTMs/Transformers could ever do. They certainly do all the things I had in mind when I cockily said they will "not solve everything". 

Yes, current-day language models (first release of chatGPT) did "solve" all of the things in the set of language understanding problems I was implicitly considering back then. So in that sense, I was wrong. But in another sense, no, it did not solve *everything*. Not yet, at least. Also, the performance of current day language-models is not obtained only by language modeling in the sense I had in mind then. I think this is important, and I will elaborate on it a bit soon.

In what follows I will briefly discuss the difference I see between current-day-LMs and what was then perceived to be an LM, and then briefly go through some of the things I think are not yet "solved" by the large LMs. I will also mention some arguments that I find to be correct but irrelevant / uninteresting.

## Natural vs Currated Lanagueg Modeling

What do I mean by "The performance of current days language models are not obtained by language modeling"? The first demonstration of large language models (let's say at the 170B parameters level, a-la GPT-3) was (to the best of our knowledge) trained on naturally occuring text data: text found in books, crawled from the internet, found in social networks, etc. Later replications (BLOOM, OPT) also used similar data. This is very close to Shannon's game, and also what most people in the past few decades thought of as 'language modeling'. These models already brought remarkable performance. But chatGPT is different.

What's different in chatGPT? There are three conceptual steps between GPT-3 and chatGPT:
Instructions, code, RLHF. The last one is, I think, the least interesting despite getting the most attention, but all are interesting. Here's my hand-wavy explanation. Maybe some day I will turn it into a more formal argument. I hope you get an intuition out of it though.

Training on "text alone" like a "traditional language model" does have some clear theoretical limitations. Most notably, it doesn't have a connection to anything "external to the text", and hence cannot have access to "meaning" or to "communicative intent". Another way to say it is that the model is "not grounded". The symbols the model operates on are just symbols, and while they can stand in relation to one another, they do not "ground" to any real-world item. So we can know the *symbol* "blue", but there is no real-world concept behind it.

In **instruction tuning** the model trainers stopped training on just "found" data, and started trainign on also on specific human-created data (this is known in machine-learning circles as "supervised learning", e.g. learning from annotated examples), in addition to the found one. For example, the human annotators would write something like "please summarize this text", followed by some text they got, followed by a summary they produced of this text. Or, they may write "translate this text into a formal language", followed by some text, followed by formal language. They would create many instructions of these kind (many summaries, many translations, etc), for many different "tasks". And then these will be added to the model's training data. 
**Why is this significant?** At the core the model is still doing language modeling, right? learning to predict the next word, based on text alone? Sure, but here the human annotators inject some level of grounding to the text. Some symbols ("summarize", "translate", "formal") are used in a consistent way together with the concept/task they denote. And they always appear in the beginning of the text. This make these symbols (or the "instructions") in some loose sense external to the rest of the data, making the act of producing a summary grounded to the human concept of "summary". Or in other words, this helps the model learn the *communicative intent* of the a user who asks for a "summary" in its "instruction". 
**An objection** here would be that such cases likely naturally occur already in large text collections, and the model already learned from them, so what is new here? **I argue** that it might be much easier to learn from direct instructions like these than it is to learn from non-instruction data (think of a direct statement like "this is a dog" vs needing to infer from over-hearing people talk about dogs). And that by shifting the distribution of the training data towards these annotated cases, substantially alter how the model acts, and the amount of "grounding" it has. And that maybe with explicit instructions data, we can use much less training text compared to what was needed without them. (I promised you hand waving didn't I?)

Additionally, the latest wave of models is also trained on **programming language code** data, and specifically data that contains both natural language instructions or descriptions (in the form of code comments) and the corresponding programming language code. 
**Why is this significant?** This produced another very direct form of *grounding*. Here, we have two separate systems in the stream of text: one of them is the human language, and the other is the programming language. And we obsere the direct interaction between these two systems: the human language describes concepts (or *intents*), which are then *realized* in the form of the corresponding programs. This is now a quite explicit "form to meaning pairing". We can certainly learn more from it than what we could learn "from form alone". (Also, I hypothesize that latest models are also trained on *execution*: pairs of programs and their outputs. This is an even stronger form of grounding: denotations). This is now very far from "just" language modeling.

Finally, **RLHF, or "RL with Human Feedback".** This is a fancy way of saying that the model now observes two humans in a conversation, one playing the role of a user, and another playing the role of "the AI", demonstrating how the AI should respond in different situations. This clearly helps the model learn how dialogs work, and how to keep track of information across dialog states (something that is very hard to learn from just "found" data). And the instructions to the humans are also the source of all the "It is not appropriate to..." and other formulaic / templatic responses we observe from the model. It is a way to train to "behave nicely" by demonstration.

ChatGPT has all three of these, if not more. This is why I find it to be very different from "traditional" language models, why it may not "obey" some of the limitations we (or I) expect to from language models, and why it performs so much better on many tasks: it is a *supervised model*, with *access to an external modality*, which is also trained explicitly by demonstration to *follow a large set of instructions* given in dialog form.

## What's still missing?

### Common-yet-boring arguments

There are a bunch of commonly occuring arguments about language models, which I find to be true but uninspiring / irrelevant to my discussion here:

- They are **wasetful**, training them is very expensive, using them is very expensive. 
  - Yes, this is certainly true today. But things get cheaper over time. Also, let's put things in perspective: yes, it is enviromentally costly, but we aren't training that many of them, and the total cost is miniscule compared to all the other energy consumptions we humans do. And, I am also not sure what the environmental argument has to to with the questions of "are these things interesting", "are these things useful", etc. It's an economic question.
- The models encode many **biases** and **stereotypes**. 
  - Well, sure they do. They model observed human's language, and we humans are terrible beings, we are biased and are constantly stereotyping. This means we need to be careful when applying these models to real-world tasks, but it doesn't make them less valid, useful or interesting from a scientiic perspective.
- The models **don't *really* understand** language. 
  - Sure. They don't. So what? Let's focus on what they do manage to do, and maybe try to improve where they don't?
- These models **will never *really* understand language.** 
  - Again, so what? There are parts they clearly cover very well. Let's look at those? Or don't look at them if you don't care about these aspects. Those who want to *really* *understand* language may indeed prefer to look elsewhere. I am happy with approximate understanding.
- The models **do not understand language like humans do**. 
  - Duh? they are not humans? Of course they differ in some of their mechanisms. They still can tell us a lot about language structure. And for what they don't tell us, we can look elsewhere.
- You **cannot learn anything meaningful based only on form**:
  - But it is not trained only on form, see section above.
- It **only connects pieces its seen before** according to some statistics.
  - ...And isn't it **magnificent** that "statistics" can get you so far? The large models connect things in a very powerful way. Also, consider how many *terribly wrong* ways there are to connect words and phrases from the corpus according to statistics. And how many such ways the models manage to avoid, and somehow choose "meaningful" ones. I find this utterly remarkable.
- We **do not know the effects these things may have on society:**
  - This is true about any new tech / new discovery. Let's find out. We can try and be careful about it. But that doesn't make the thing less interesting / less effective / less worthy of study. It just adds one additional aspect worth studying.
- **The models don't cite their sources:** 
  - Indeed they don't. But... so what? I can see why you would like that in certain kinds of applications, and you certainly want the models to not bulshit you, and maybe you want to be able to *verify* that they don't bulshit you, but these are all not really related to the core of what language models are / this is not the right question to ask, in my opinion. After all, humans don't really "cite their sources" in the real sense, we rarely attribute our knowledge to a specific single source, and if we do, we very often do it as a rationalization, or in a very deliberate process of first finding a source and then citing it. This can be replicated. From an application perspective (say, if we want to develop a search system, or a paper writing system, or a general-purpose question answering system), people can certainly work on linking utterances to sources, either through the generation process or in a post-processing step, or on setups where you first retrieve and then generate. And many people do. But this is not really related to language understanding. What **is** interesting though, and what I find to be a more constructive thing to ask for, is (a) how do we separate "core" knowledge about language and reasoning, from specific factual knowledge about "things"; and (b) how do we enable knowledge of knowledge (see below).

## So what's missing / what are some real limitations?

Here is an informal and incomplete list of things that I think are currently challenging in current "large language models", including the latest chatGPT, and which hinders them from "fully understanding" language is some real sense. These are a bunch of things the models still cannot do, or are at least very ill-equipped to do.

- **Relating multiple texts to each other**. In their training, the models consume text either as one large stream, or as independent pieces of information. They may pick up patterns of commonalities in the text, but it has no notion of how the texts relate to "events" in the real world. In particular, if the model is trained on multiple news stories about the same event, it has no way of knowing that these texts all describe the same thing, and it cannot differentiate it from several texts describing similar but unrelated events. In this sense, the models cannot really form (or are really not equipped to form) a coherent and complete world view from all the text they "read". 

- **A notion of time**. Similarly, the models don't have a notion of which events *follow* other events in their training stream. They don't really have a notion of time at all, besides maybe explicit mentions of time. So it may learn the local meaning of expressions like "Obama became president in 2009", and "reason" about other explicitly dated things that happened before or after this. But it cannot understand the flow of time in the sense that if it reads (in a separate text) that "Obama is the current president of the united state" and yet in a third text that "Obame is no longer the president", which of these things follow one another, and what is true *now*. It can concurrently "believe" that both "Obame is the current president of the US", "Trump is the current president of the US" and "Biden is the current president of the US" are all valid statements. Similarly, it really has no practical way of interpreting statments like "X is the latest album by Y" and how they stand in relation to each other.

- **Knowledge of Knowledge** The models don't really "know what they know". They don't even know what "knowing" is. All they do is guess the next token in a stream, and this next token guess may be based on either well founded acuired knowledge, or it may be a complete guess. The models' training and training data have no explicit mechanism for distingushing these two cases, and certainly don't have explicit mechanisms to act differently according to them. This is manifested in the well documented tendency to "confidently make stuff up". The learning-from-demonstraiton (RLHF) made the models "aware" that some answers should be treated with caution, and maybe the models even learned to associate this level of caution with the extent to which some fact, entity or topic were covered in their training data, or the extent to which the data is reflected in their internal weights. So in that sense they exhibit *some* knowledge of knowledge. But when they get over this initial stage of refuding to answer, and go into "text generation mode", they "lose" all such knowledge of knowledge, and, very quickly tranistion into "making stuff up" mode, also on things that it clearly stated (in a different stage) that is has no knowledge of.

- **Numbers and math** The models are really ill-equipped to perform math. Their basic building blocks are "word pieces", which don't really correspond to numbers in any convenient base. They also don't have any appropriate way of learning the relations between different numebrs (such as the +1 or "greater than" relations) in any meaningful and consistent way. LLMs manage to perform semi-adequately on some questions involving numbers, but really there are **so much better** ways to go about representing numbers and math than the mechanisms we gave LLM, that it is surprising they can do anything at all. But I suspect they will not get very far without some more explicit modeling.

- **Rare events, high recall setups, high coverage setups:** By their nature, the models focus on the common and probable cases. This makes me immediately suspicious about their ability to learn from rare events in the data, or to recall rare occurances, or to recall all occurances. Here I am less certain than in the other points: they might be able to do it. But I am currently skeptical.

- **Data hunger** This is perhaps **the biggest technical issue** I see with current large language models: they are extremely data hungry. To achieve their impressive performance, they were trained on trillions of words. The obvious "**...and humans learn from a tiny fraction of this**"  is of course true, but not very interesting to me on its own: so what? we don't have to mimick humans to be useful. There are other implications though, that I find to be very disturbing: **Most human languages don't have so much data**, certainly not data vailable in digital form. 

  **Why is this significant?** Because it means that we will be hard-pressed to replicate the incredible English understanding results that we have now for **other languages**, such as my native language Hebrew, or even to more common ones like German, French or Arabic, or even Chinese or Hindi (I don't even consider so called "low resource" language like the many african and phillipinian ones). 

  We can get a lot of data in these language, but not *so much* data. Yes, with "instruct training" we may need less data. But then the instruction data needs to be created: this is a huge undertaking for each new language we want to add. Additionally, if we believe (and I do) that training on code + language is significant, this is another **huge** barrirer for achieving similar models for languages other than English.

  **Can't this be solved by translation?** after all, we have great progress also in machine translation. We can translate to English, run the model there, and then translate back. Well, yes, we could. **But this will work only at a very superficial level**. Different languages come from different geographical regions, and these regions have their local cultures, norms, stories, events, and so on. These differ from the cultures, norms, stories and events of English speaking geographies in various ways. Even a simple concept such as "a city" differs across communities and geographies, not to mention concepts such as "politeness" or "violence". Or "just" "factual" knowledge about certain people, historic events, significant places, plants, customs, etc. These will not be reflected in the English training data, and cannot be covered by translation.

  So, data hunger *is* a real problem, if we consider that we may want to have language understanding and "AI" technologies also outside of English.

  For those of us who want to worry about social implications, this combination of data-hunger and English/US-centrality is definitely a huge issue to consider.

- **Modularity**  At the end of the "common yet boring arguments" section above, I asked "**how do we separate "core" knowledge about language and reasoning, from specific factual knowledge about "things"**". I think this is a major question to ask, and that solving it will go a long way towards making progress (if not "solving") many of the other issues. If we can modularize and separate the "core language understanding and reasoning" component from the "knowledge" component, we may be able to do much better w.r.t to the data-hunger problem and the resulting cultural knowledge gaps, we may be able to better deal with and control biases and stereotypes, we may get knowledge-of-knowledge almost "for free". (Many people are working on "retrieval augmented language models". This may or may not be the right way to approach this problem. I tend to suspect there is a more fundamental approach to be found. But history has proven I don't have great intuitions about such things.)

## Conclusion

Large language models are amazing. Language modeling is not enough, but "current language models" are actually more than language models, and they can do much more than we expected.
This is still "not enough", though, if we care about "inclusive" language understanding, and also if we don't.  

Transship from Prof. Yoav Goldberg https://gist.github.com/yoavg/59d174608e92e845c8994ac2e234c8a9#file-llms-md



















































































