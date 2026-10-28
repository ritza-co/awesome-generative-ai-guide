# Make Claude Code 10x Better and Cheaper: 16 Research-Backed Tips

**Published YouTube description:** [Read the full description](descriptions/claude-code-5-levels.txt)

[Watch on YouTube](https://www.youtube.com/watch?v=quU2MM2k45A) · 2026-09-03

![The 5 levels of agentic workflows, 16 practical tips for Claude Code and Codex](images/claude-code-5-levels.png)

<!-- cheat sheet -->

## In this video

- **Level 1, context audit and setup**: what you put in place before asking for anything, so the agent stops guessing
- **Level 2, prompt optimization**: how to ask, including the habit that replaces most prompt-wording technique
- **Level 3, task isolation**: keeping the agent sharp through a long task instead of watching it drift
- **Level 4, failure recovery**: the 2 moves that fix a rotted thread in seconds
- **Level 5, loop engineering**: the 4 parts of a loop that runs on its own without spinning or running up a bill
- **16 tips across the 5 levels**, each with a prompt you can copy, and the research behind the ones that are counterintuitive

## The 16 tips, with prompts

Every tip from the video, with its prompt, is in one file:
**[claude-code-16-tips.md](claude-code-16-tips.md)**

Do not take notes. Paste this into your agent and it will read the whole thing and
apply it with you:

```
Read https://raw.githubusercontent.com/aishwaryanr/awesome-generative-ai-guide/main/youtube/claude-code-16-tips.md

Go through all 16 tips. Tell me which ones we already do, which ones apply to this
project, and which ones do not. For the ones that apply and we are not doing,
propose the specific change and wait for me to approve each one before you make it.
```

It covers all 5 levels: context audit and setup, prompt optimization, task
isolation, failure recovery, and loop engineering.

## Resources

- [LevelUp Labs](https://levelup-labs.ai/)
- [The Nuanced Perspective (newsletter)](https://thenuancedperspective.substack.com)
- [LevelUp Labs education](https://levelup-labs.ai/education)
- [Awesome Generative AI Guide](https://github.com/aishwaryanr/awesome-generative-ai-guide)
- [My courses on Maven](https://maven.com/aishwarya-kiriti)

## Sources

- Gloaguen et al., *"Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?"*, ETH Zurich and LogicStar.ai, 13 February 2026. Across 138 real Python tasks, LLM-generated context files lowered task success by roughly 2 to 3% while raising inference cost by over 20%. The cost increase held for developer-written files too, so the case for writing your own is control over what the agent does, not a free win. [arxiv.org](https://arxiv.org/abs/2602.11988)
- Chroma, *"Context Rot: How Increasing Input Tokens Impacts LLM Performance"*, July 2025, for the finding across 18 models that the same fact in a short prompt beats the same fact inside a large one. [research.trychroma.com](https://research.trychroma.com/context-rot)
- Liu et al., *"Lost in the Middle: How Language Models Use Long Contexts"*, 2023, for why position inside the context window changes what gets used. [arxiv.org](https://arxiv.org/abs/2307.03172)
- Drew Breunig, *"How Long Contexts Fail"*, June 2025, for the tool-overload result: a quantized Llama 3.1 8b failed a task on the GeoEngine benchmark with 46 tools available and passed the same task with 19, well inside its context window. [dbreunig.com](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)
- Anthropic, *"Effective context engineering for AI agents"*, for the smallest-set-of-high-signal-tokens framing behind Level 3. [anthropic.com](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

## Transcript

_Auto-generated captions from YouTube, lightly cleaned._

If you're using agents like Claude Code or Codex Daily and it still feels like you're not getting the best bang for your buck, like you're burning through your token costs, it takes several back and forth to get one thing done and you just seem to be not getting the 10x gains that everybody's talking about, then this video is for you. And if you're thinking that I'm going to hand you a list of better prompts that will magically fix everything, then no, we're not doing that today. The people who are getting real work out of these tools are operating at five distinct levels that's not just about prompting. Today I'm going to give you all five with 16 practical tips across those levels. And remember that every one of them is drawn from real AI research around how these agents work under the hood.

By the end, you'll know how to set up your agents, right? Cut your token costs by at least 50% and also make them improve over time. So here's everything we'll cover. You can take a screenshot and keep it or the HD version is also available on my GitHub repository that's linked in the description. And it's 2026, so don't really worry about taking notes.

I've turned every tip in this video into an open- source checklist on my GitHub that you can directly hand to your agent and it just follows those instructions for you. So, just give this video your undivided attention. And if you're new here, I'm Ash Reganti. I've been working in AI for about 10 years now. previously as an AI scientist and researcher at AWS and now I'm building my own AI native startup in San Francisco.

It's called Level Up Labs and we help companies and teams go AI native through education, engineering and strategy. All right, so let's get started at the bottom which is level one context audit and setup. Now context is pretty much the stuff you hand to your agent, your docs, your data, your files, your notes and all of this, right? And this whole level is getting that part right, giving it the good stuff, and also making sure that you're leaving the junk out. Now, if you nail this, honestly, everything on top of that gets much more easier.

We'll cover four research back tips here. Now, the first tip is for you to audit your context. And this idea is pretty simple. If all of the data that you're giving to your agent is a mess, it's out of date, they're contradictory facts and all of that, it'll just confidently give you wrong answers or it'll probably hallucinate. So even before you ask your agent to look at your data, you want to set up an audit step.

Now let me show you how that looks like with a real example. Now here I have a working folder with a bunch of my company documents. I want to make sure that the data in my working folder doesn't have any discrepancies. So I'm just going to go to my Claude Code. Again, remember that you can work with pretty much any agent.

It doesn't have to be Claude Code. This is just an example. And this is what I'll be typing or speaking because I'm using Whisper Flow. Audit everything in my company docs folder before I rely on any of it. Read every file and list every place where the files conflict with each other.

There are duplicates or look out of date or if there's any wig or missing information. Group the issues together and let's fix them. So that's pretty much the prompt. Now you can see that the agent has come up with multiple groups of issues which are basically data inconsistencies. There are inconsistencies in terms of data conflicts.

There are duplicates and each of them are saying different things. There are promises with nothing behind them which are essentially gaps in my data and there are also wake terms. Now doing this audit will help you understand where there are data inconsistencies even before you move to the next step. Now let's go to tip number two which is setting up your identity file and writing it yourself keeping it crisp. Now the identity file is a short brief that your agent reads every session.

So it's actually using up tokens every session to read the identity file. So you want to keep only the most important things in your identity file. Now two things you want to remember when you write your identity file and both of them are backed by research. One is to write it by yourself or at least review them. A study that was done by ETH Zurich identified that AI written identity files actually do worse and cost 20% more to users rather than writing them yourself.

I've also written best practices about how to set up your identity file in my GitHub repository so you can directly learn from it. And the second and more important thing here is to keep it short because remember the agent is seeing it every time you have a conversation with it. There was an article that Anthropic recently released saying that most people bloat their identity files. Tip number three in this level is to show your agent what good looks like and codify that. So whenever you're getting your agent to do a task, instead of describing what you want, just show a few historical examples.

For instance, let's say you're writing a couple of emails to potential clients. Get few historical examples of what you think good emails look like instead of explaining that. Make it professional, make it bold and all of that, right? Because agents understand much better from examples rather than explaining them in subjective terms. And the power move here is also to codify that into templates.

Let me show you how that works with a simple email example. Read the three cold emails that exist in my examples folder. That's my style. Usually, it's short, casual, specific. and I want you to write a new cold email in the same style to the head of growth of a 20 person e-commerce brand pitching our company to them.

So I have a bunch of examples of emails that I've written and the agent figures out from those examples instead of me using a long prompt to explain what I expect. You can see the patterns it's picked up from my previous mails. It says that there's a trigger value and a soft ask and it's almost like codified it. " That's pretty much it. Every time you write a new email, your agent will use the same template.

So, you've also codified the behavior. Okay. The last tip in this level is to turn off the tools that you don't actually use. And I've seen a lot of people connect to hundreds of tools that they don't use, and that's slowly eating up your token cost. Here's why, right?

Every tool you connect to your agent loads its whole description into the context on every message of your agent whether you use it or not. So if you've got so many of them hooked up but rarely use say 90% of them, the rest of them are just adding empty token costs. Not just that, there's also research that shows accuracy of agents reduces significantly when there are tons of tools attached to them. So make sure you turn off connections to tools that you rarely use to save on token costs. And it's pretty simple to do in most agents.

Okay, now you've done a great job at your context level or level one. So, let's go to the second level, prompt optimization. Now, remember that previously you made sure that the agent has access to the right information. This level is really about how you communicate well with the agent. So, this is pretty much where everybody spends their time.

And weirdly enough, this is also where most people do the worst. We're going to cover three tips here. Now, tip number one is to let the agent interview you. Whenever you're trying to do a task, stop trying to write the perfect prompt in the first go. Instead, let the agent interview you first and all you need to do is to tell it to ask you questions so that it gets all of the information from you even before you get started.

Here's a simple example, right? Let's say I'm planning a product launch for my company with a hypothetical name. Let's call it Acme Analytics. And I want to come up with a good launch plan. So, here's how I talk with my agent.

I want to plan our upcoming product launch. Before you write anything or give me a plan, first interview me. Ask me questions and then I'll answer each one of them and keep going until you have everything you need to build a genuinely good launch plan. Don't start the build until we've decided on all of the details. You can see how the agent is coming up with so many questions for me and some of these are things that even I did not probably think of.

It helps me nail down details in a much better way rather than me trying to anticipate everything. All right, with that done, the second tip in this level is to make your agents site their sources. This is the single best way to reduce hallucination in agents. Whenever you're working with context documentation or anything that relies on external context, always ask your agent to site its sources before coming up with response and you'll see how quickly hallucination starts reducing. The third and final tip at this level is you can slash literally 90% of your costs if you have your context first and questions last in a long conversation.

Let's say you've got a very large document and you have a bunch of questions. Always space that document first and then start the questions underneath. And why am I saying this now? This is because of a concept called prefix caching or prompt caching. So all of these Asians cache your prompt from the top.

So whatever stays put up there gets reused and gets picked up from the cash and the price for cached prompts is much lesser up to 90% lesser as compared to new prompts. So the rule of thumb is always to keep the stable stuff on the top or use it as a prefix and your questions or any conversations at the bottom so that you can get all of the benefits from prefix caching. All right, that's level one and level two. You know how to set up your context and you also know how to communicate with these agents so that you can slash your costs. Now, let's go to the annoying part, right?

The longer you work with these agents, the more their quality starts ripping apart. And this is backed by research. That's what we're going to solve in level three, which is task isolation. And this level is all about how you keep your agent sharp through long, messy tasks instead of watching it get dumber as the chat starts becoming longer. I'm going to cover three tips here.

The first and the simplest tip is to make sure that you use a fresh chat for every new task you start. If you're working between different tasks, the moment you switch context, make sure you open a new chat instead of trying to fit everything in the same long conversation. Now, there are two benefits of this. Now, the first benefit is that it gives you a huge bump in quality because the agent is not looking at context that is unrelated. The second important benefit is that by clearing out context, you're not paying for those extra tokens that were part of some other task.

So just remember that one task, one chat, it costs you pretty much nothing but brings in a huge jump in quality and cuts down on your cost. Now the second tip here is for you to prune your context. Don't dump large context on these agents. And this is backed by the entire idea of context rot. There's literally a study on it that was conducted which says that the more you stuff into the context of your agents, the worse it gets and it loses the thread, gets confused and misses important information.

So whenever you're doing a particular task, always pull data only relevant to that task. Maybe you can restrict the folder that the agent is using or you can build a wiki. I've covered how you can build a wiki in my previous videos, so make sure you check them out. But always prune your context. The idea in the third tip is to compact your progress into a single file.

So when you're working on longunning projects, instead of carrying conversations in the same thread, you can decide when to compact that into a progress file and then load it back up to continue conversation. This helps you cut costs and also gives you control on how to save this file. Here's a prompt on how you can simply do it. We've covered quite a lot today. md with exactly this and nothing else.

the goal, the key decisions we made in this conversation, what is still open for tomorrow, and the most important next steps for us. Keep it typed, no filler, so I can open a fresh chat tomorrow and continue and pick up where I left off. Remember that agents themselves come with compaction procedures, but manually doing it gives you a lot of control on how you want to structure the compaction and how you want to save your progress. Because if you allow agents to do it on their own, you pretty much have no control on how they're compacting. Now let's go to level four which is failure recovery.

Now the tips in this level are very important when you're running long chats with your agent and it starts rotting mid task. It keeps forgetting stuff or probably repeating mistakes that you already fixed. Now two important tips here. The first step is that you can rewind to the last good point or fork your chat from a particular point. This is how it exactly works.

I'm going to use the rewind command and you can see that I can rewind to any previous point in the conversation and continue my conversation from there. So if I feel like my conversation started rotting from a particular place, I can go back up there. And there's also a specific button here where you can fork the conversation so that you can fork the chat with all of the context you have until there to start a new conversation. Right? So these two are super important whenever you feel like you have really long conversations and you want to take a time stamp and you want to start a fresh from a particular point or you want to rewind a few chats so that you can get to a point where you feel comfortable.

Now these are the most underused capabilities of modern agents make sure you use them as you see fit. The second tip in this level is if your whole thread is rotting if you feel like that you're going nowhere just hand off and restart. Anthropic themselves wrote this in a recent blog. So when a conversation is headed nowhere, you feel like the agent is getting confused and really not understanding you, just save the progress like we mentioned in the previous level and just move to a new conversation. Don't try to fight it.

All right. So if you incorporate everything from the past four levels, you're already operating like a pro. But let's get to one level higher, which is something that everybody's been discussing about these days, which is loop engineering. Now, this is where you literally stop babysitting your agents turn by turn. You can set them up and run on their own.

and also let them improve over time. If you get this right, a lot of boring stuff can get automated while you're away. If you just strip down all the hype around loop engineering, a loop essentially is just a prompt that the agent keeps repeating. It essentially does the work, checks itself, tries again, all without you having to lift a finger. Now, let's take a real world example to make sure you understand this right now.

Say you want an agent to check your competitors every morning and tells you what's changed so that you can understand how your product road map should look like. Now, in order to set up a loop for this, it requires four things. And these four things are necessary for any loop you set up. So, pay attention. One is a very specific goal that can check on its own.

For instance, go through five competitors and tell me what changed in terms of X, Y, and Z dimensions. Don't say something vague here. Two is pretty much a check to understand if the job or the goal that it's achieving has been completed. It's almost like an evaluation or a test to make sure that the goal is being reached. Three is pretty much a cap.

and a gate set in the tool so that it stops after a certain period right so it's almost like the stopping criteria before it reports back to you and four is pretty much memory that it can learn from in order to make it very concrete for you let me show you an example let's say acme analytics is a fictitious company that I'm running and the competitors for acme include folks like mix panel amplitude post hog heap and static right all of all in the same domain now this is how I set up a loop every morning check the five competitors we have for my company and compare each one against the snapshot that I have from yesterday and flag only what changed since then. Save the updated snapshot back to the file. Then post a short summary to my Slack. What changed and for which competitor? Stop once you've been through all five.

And don't take any action beyond reading and posting that summary. So make sure that you look at your own use case and think of how you can set up loops that can solve goals for you instead of writing prompts manually. Always remember that it helps to set up loops for conditions that you know very well about and you know how to evaluate well. If they're loops that you set up for tasks that you don't understand well enough, you don't know what the definition of done is. You don't know what the gate or the stopping criteria should be.

This doesn't really work well. It only works for use cases where you understand the process well enough so that you can set up an automated loop. Anyway, those are the five levels of operating so that you can get the best benefits from agents like Claude Code, Codex or anything that will come up in the future. So that's pretty much the whole climb. In level one, you set up the context so that it stops guessing.

In level two, you learned how to optimize prompts with agents. In level three, you understood how you can keep it sharp and prevent it from context rotting. In level four, you understood how you could recover failures either through reind or through forking and all of this. And in level five, you set it loose so that it can run on its own. And most people never get past level two.

And congratulations, you did all five. I really hope this video was useful for you. And like I mentioned, all of these tips are available as a checklist on my GitHub repository. You can directly hand it to your agent and work with your agent to make sure that it applies all of these best practices. If you enjoyed the video and would like to get more such battle tested researchback tips, make sure you subscribe.

All the very best.
