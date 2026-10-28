# AI system design: a 5-layer framework

**Published YouTube description:** [Read the full description](descriptions/ai-system-design.txt)

**Watch:** [2,000+ Hours of Learning Agentic AI System Design in 20 Minutes](https://www.youtube.com/watch?v=Mv7eiQFQQ1E)
**Published:** October 22, 2026

Design an e-commerce support agent that answers help-center questions, looks up orders, creates tickets, and escalates uncertain requests to a person. Each architecture choice follows from a stated business requirement.

## Use the framework

| Layer | Questions to answer | Video chapter |
|---|---|---|
| 1. Scope and assumptions | Who needs help? What outcome matters? What does AI handle? How much agency is appropriate? | [01:23](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=83s) |
| 2. Architecture | What does the system need to know, act on, remember, and coordinate? Use KARC to connect those needs to components. | [04:32](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=272s) |
| 3. Observability | What do you log? Which failures do you evaluate? Which checks must run before an action? | [11:10](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=670s) |
| 4. Production and ops | What changes with traffic, latency targets, failures, security, and data freshness? | [14:11](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=851s) |
| 5. Optimization | Which measured cost, quality, or latency problem warrants a change? | [17:03](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1023s) |

## Practice with the worked cases

Start with the [customer support agent](../interview_prep/system-design/customer-support-agent/README.md) and its [runnable code](../interview_prep/system-design/customer-support-agent/code/README.md). Then apply the questions to the [full collection of 10 system-design cases](../interview_prep/system-design/README.md).

The case collection includes an enterprise research assistant, Text-to-SQL analytics, financial and operations decisioning, security and compliance, coding, document decisioning, voice support, SDR sales development, and a clinical scribe. Each case contains a design and a code example.

The worked cases use their own section layouts. Use this page's 5-layer checklist to relate them to the video's Scope, Architecture, Observability, Production and ops, and Optimization sequence.

The video also links to the [AI system-design book](https://maven.com/aishwarya-kiriti/o/a05048).

## Transcript

_English captions from the published video. Automatic caption wording is retained; timestamps link to the video._

**[00:00](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=0s)**

I have interviewed hundreds of people for AI roles. First, when I was a tech lead at AWS, and now for my own company. And the candidates who don't get hired often have the same thing in common. Yes, they know what rag is, MCPs, or even tool calling is, but struggle to apply those concepts in a real business problem, or explain what it takes to scale an AI system to production. That's the skill of AI system design. So, in this video, we're going to learn a five-layer framework for AI system design that you can apply to pretty much

**[00:31](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=31s)**

any real production AI system, all the way from scoping the problem to optimizing it in production. And by the end, you'll know how to approach a real-world AI problem and explain your decisions, helping you stand out in interviews instead of all of those people who are just throwing around terms. We'll use the example of a customer support agent, one of the most common AI system design problems, in order to walk through the framework. I've also put together a book where I use the exact framework across nine other common AI system design problems, which you can practice after you watch

**[01:01](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=61s)**

this video. So, let's get started. Let's say we have an e-commerce company that wants to build an AI customer support system. It should answer questions from the help center, look up orders, create support tickets, and all of that. And most importantly, it should also be able to escalate [music] to a human when it's not sure. Our job is to pretty much design this system and think through what it would actually take to scale it to production. Now, when most beginners hear a problem like this, they immediately start throwing terms like rag models tools agents and sometimes even maybe multi-agent systems. But before you do any of that,

**[01:33](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=93s)**

remember that you need to understand the scope of the problem very well before going into solutioning. A lot of people get confused in this, but a huge part of system design is simply scoping the problem and making your assumptions explicit. That is our first layer, which is scope and assumptions. Now, you want to start by answering four questions here. Who is the user and what problem are they having? What is the outcome that we're trying to improve? Which part of this problem or workflow should the AI handle? And how much agency should

**[02:03](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=123s)**

the system have? Now, for our example, the user is a customer who needs support, and the main problem is that a lot of these requests are repetitive and still [music] require a human to answer them. The outcome we want is to resolve more of those routine requests without needing a support agent every time. And the part AI should handle is answering common questions for the help center. It should also look up orders, create support tickets, and escalate to humans when it's not confident. And the fourth point is something a lot of beginners don't think of at all, which is what I call the agency control trade-off, which

**[02:35](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=155s)**

is we need to decide how much autonomy do we [music] actually want to give our system? Should it only draft responses for a human to approve, or can it answer to the customer directly? Can it also take actions on their behalf? Now, thinking through and working through all of these questions will directly shape the architecture that you build around it. For this use case, let's assume the system can autonomously answer routine questions and take lower risk actions like looking up an order or creating a ticket. And when it isn't confident, it escalates to a human. Higher risk actions like issuing a refund should

**[03:06](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=186s)**

require a policy check or a human approval. Now, the second part of scoping is making your assumptions explicit. Now, you can think about assumptions in four different buckets for an AI system. It's basically data, the types of inputs, the operating constraints, and the risk of actions. And if you're answering a system design interview question, this is where you want to ask your interviewer as many questions as possible, because the more assumptions you make up front, the better your system design or your architecture design will get. And you're not really designing against something that exists only in your head, you're making it very explicit. Okay, we spoke

**[03:37](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=217s)**

about four buckets, right? Let's go through those four buckets. Now, remember that these are the assumptions I'm making for my product, and we'll walk through it, but you'll have to translate it into your own use case. For our system, the data is probably a few thousand help center articles, mostly in English. The requests are usually short. Some are semantic questions. For instance, a customer might say something like, "Can I return this product?" While others contain explicit things like say order IDs or error codes. And let's also assume that usually one article is enough to answer one question. And

**[04:08](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=248s)**

operationally, we need to be serving about thousands of customers per day. And users tend to expect an answer within a few seconds. And the actions have different levels of risk. For instance, looking up an order can be pretty safe, but issuing a refund is not. Now, these are the assumptions that we'll use for the rest of the design decisions. Now that you understand the scope and you've made a bunch of assumptions about this problem, you want to go to the next layer, which is designing the architecture. And in order to do this, you can use a simple framework that I created called CARC, K A R C, which is know, act, remember, and

**[04:40](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=280s)**

coordinate. What that means is you want to design what the system has to know, what should it be able to act on, what does it need to remember, and how should all of these pieces coordinate with each other so that I can build an architecture. All of this put together should give you the starting architecture. Now, remember a lot of this will be determined by how you made the assumptions in the first layer. For our use case, let's start with the know, which is what does the customer support agent actually need to know. Now, given the problem we scoped, a lot of the information this system needs probably

**[05:11](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=311s)**

lives in unstructured documents like return policies, shipping rules, cancellation policies, troubleshooting steps, and all of that. So, think of this as a set of documents that have the standard operating procedures that the agent has to be aware of in order to complete its task. Now, the first job here is to make sure that the system can retrieve the right information before it answers anything. Now, given the data that we described earlier, a reasonable first version is a simple naive rack pipeline. We pass and chunk the documents that we have, create embeddings, and store them in a vector

**[05:41](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=341s)**

database. Now, here's where you have to think a bit based on the assumptions we made, right? Some questions are semantic. For instance, someone might say, "Can I send this back?" While the document can say something like a return policy, which means I need vector-based retrieval here. But I also know that in my data, some questions can contain exact [music] things like, for instance, SKUs, right? Or unique IDs given to the products, in which case a BM25-like keyword search might help. So, I need to probably think of something like a hybrid retriever to make sure that both

**[06:11](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=371s)**

these buckets of questions get answered. Then we can use a reranker on top results and pass only relevant chunks in order to get the right answer from the model. And remember that we made this assumption that one article is usually enough to answer a question. Because of that, I wouldn't start with a graph rag or a complicated retrieval flow. If our evals later show that many questions actually need information across several documents, then we can revisit that decision, but do not overcomplicate your solution. Now, next piece of the CARK framework is act, right? What should

**[06:41](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=401s)**

this system actually be allowed to do? Now, the way an agent can take action is through tools or functions that can let it interact with external systems. Now, for our customer support system, we probably need four core tools, which is look up order, create ticket, check refund eligibility, and escalate [music] to human. Notice that here I'm specifically saying that we need a tool called refund eligibility, but not issuing a refund, because based on the agency decision we made earlier, the agent can only gather information, but for a high-risk action, it needs more

**[07:11](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=431s)**

control. Now, once you know what kind of tools are required, think about four things for each of these tools, right? One is permissions, two is blast radius, three is failure handling, and four is trust boundaries. So, for one, permissions, pretty much follow least privilege. Every tool should get only the access to the information that it requires. For instance, look up order only needs read access to the database, and it should only be able to access orders belonging to the authenticated customer. Second, for blast radius, ask what happens if the tool does the wrong

**[07:42](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=462s)**

thing, right? For instance, looking up an order is pretty low risk, but issuing a refund is not. So, higher consequence actions like refunds or cancellations should sit behind a deterministic policy and should require human approvals. Now, the third thing is failure handling, which is tools will time out and APIs will probably go down and you need to find a way to handle failures. So, you'll have to think about fallback policies for a particular tool does not function in the way it was designed. For instance, if the lookup order service keeps failing, we have to retry it within a certain limit and then [music]

**[08:12](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=492s)**

escalate to a human. So, you have to design the right failure handling techniques. And finally, [music] designing trust boundaries. Now, user messages, retrieve documents, and tool outputs should all be treated as untrusted data. For instance, just because your system sees an instruction inside a document, it shouldn't just go and execute it, especially once the agent has access to private data and tools that can take actions. Now, these are all aspects you want to focus on when you're designing tools. Now, let's go to the third piece, which is remember. What does the system need to actually

**[08:42](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=522s)**

remember and how would you design it? Now, this is usually called as memory in AI systems and there are four useful types of memory that you want to think through and contextualize it to your use case. The four types are working memory, semantic memory, episodic memory, and procedural memory. Now, working memory is what the system needs for the current task. That includes, say, conversation history, retrieved information, any intermediate tool results that your system needs, and all of that. And semantic memory is persistent facts about the user or the account. For instance, their preferences or relevant

**[09:13](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=553s)**

customer information, transaction information that is required for the support agent, etc. And episodic memory is pretty much what happened before. Maybe this customer contacted support yesterday and the order was already escalated, or maybe this is a customer with the history of refunds, so you want to handle the case well. Procedural memory, which is information about how the system should behave based on previous patterns or preferences. Now, remember that just because these four exist doesn't mean you have to use all of them. You want to be mindful about what is useful based on the assumptions

**[09:44](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=584s)**

that you're fixed. Now, working memory is clearly required for our use case so that we can follow the current conversation. Episodic memory can also be useful in this context so that we don't repeat the same actions that have already happened. Some semantic memory might help with relevant customer information, and procedural memory is probably not very necessary in the first version. Now, finally, the last part of the framework is coordinate, which is how do knowledge, tools, and memory actually work together in order to build the system. Now, based on everything we saw, the most simple starting version of

**[10:15](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=615s)**

this system is a single agent that has access to help center articles through hybrid rag, and also has access to a small set of scope tools for things like order look up, ticket creation, and for memory, it needs to keep track of the current conversation and relevant past actions. So, here's what our system looks like. When a request comes in, the user is authenticated, the retrieval knowledge and memory are pulled in, and the agent gets access only to the tools that it's allowed to use, and the agent can then answer, call a tool, ask for more information, or escalate to human.

**[10:45](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=645s)**

And the agent should be specifically prompted that high-risk actions should go through a policy check or human approval, and the whole loop has limits around retries, steps, and timeouts so the agent cannot keep running indefinitely. Now, that's our first iteration that we designed for this problem, but we really need to build the observability layer around it in order to understand how this is working and how we can keep improving it over time, which brings us to layer three, which is observability. So, like I mentioned, observability is how you understand what your system is doing after you've built

**[11:16](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=676s)**

it. If something goes wrong in production, you shouldn't just know the final answer was bad, you should be able to trace where it went wrong and have enough information to actually fix it in the next go. Now, you want to use three questions here, which is what should be logged, what evals should be built on top of those logs and which of those evals should run online versus offline? Let's start with logging, right? At a high level, you need to log all of the important inputs, outputs, and decisions across every component of the system. And for our use case, that includes what

**[11:46](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=706s)**

went into the model, what came out, what was retrieved, which tools were called, and what arguments were passed to these tools, how many retries happened, how long every step took, and how much it cost. Now, once you've decided what to log, the next question is what evals should you build on top of it? And the most important thing about evals is they need to be actionable. So, just for the record, evals or evaluations are essentially metrics that tell you how well your system is performing in a particular dimension so that you can actionably improve that metric. It's

**[12:16](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=736s)**

almost like testing in software, but the way it's done in AI systems is a little different. Now, designing evals in itself is a skill that's expected out of good AI engineers. I've made a previous video on evals, do check it out. But, remember that you need to be building evals that can give you actionable insights on how to improve your system. So, let's go back to the customer support agent that we designed, right? And try to ask what can go wrong in each one of these components. For instance, in the knowledge layer, it's possible that we retrieved irrelevant context or

**[12:46](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=766s)**

missed information required in order to answer the question. Now, metrics like context precision and context recall can help us measure that. Or maybe retrieval was good, but the final answer still wasn't satisfactory, in which case metrics like groundedness or faithfulness can help us catch it. For tools, maybe the agent selected the wrong tool or passed the wrong arguments or the tool failed. So, we need metrics like tool selection accuracy, argument accuracy, tool success rate, and also retry rate. And for the overall agent

**[13:16](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=796s)**

flow, you want to be looking at whether it escalated when it should have and whether the task was actually successfully completed. So, you're probably looking at metrics like resolution rate, latency, cost per resolution, and even customer satisfaction. There are different ways to implement these evals. If something has an objective answer, you want to use code-based metrics. For instance, did we call the right tool? Were the arguments valid? Did the return schema make sense? And these kind of things. They [music] can be checked programmatically. But for something more subjective, like whether

**[13:47](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=827s)**

an answer is grounded in its context, whether it's useful, whether it's matching the brand voice of the company, those are the cases where you need AI judges or LLM judges. And you need to be designing good quality rubrics for those. So, the observability layer pretty much gives you the feedback loop. You will log what happened, evaluate where the system is failing, and use those signals to improve the architecture. [music] Now, let's go to layer four, which is production and ops. How do you take everything that we design and scale it to production? Now, again, go back to the assumptions that we started with and ask, what has to

**[14:18](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=858s)**

change when the system starts serving real users reliably? And I want you to think about five dimensions here: scale, latency, reliability, [music] freshness, and security. Now, let's see how all of these five matter for our use case. First, scale. You want to keep the agent service stateless, store conversation state in memory externally, and put multiple replicas behind a load balancer, given that we need to be answering thousands of customers at once. Now, that gives us horizontal scaling as traffic grows. [music] Second

**[14:48](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=888s)**

is latency, right? Remember that in the observability part, we already log how long retrieval takes, [music] model calls, tools, etc. take. So, now you should be able to use those traces to find the bottleneck. And for production, you should be able to set an overall latency budget and break it across these components, [music] and then optimize the slowest stage first. For instance, if order lookup takes 3 to [music] 5 seconds, that's where we start. We can also add timeouts, so one slow dependency doesn't really end up holding the whole request. Now, the third thing

**[15:19](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=919s)**

you want to think about in production is reliability. Think about retries, fallback mechanisms, queues, and graceful degradation. If a particular tool temporarily fails, retry it within a limit. If it keeps failing, it has to escalate. Now, these are the kind of policies that you need to set up so that your system works reliably in production. [music] And say the traffic spikes, then you need to queue requests instead of letting the whole system fall over. And the fourth thing that matters in production is freshness. For instance, in our use case, it's possible that help center [music] articles change. So, you need to design a cadence

**[15:49](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=949s)**

on when in gestion should happen, or the ingestion pipeline should be designed in a way that it detects updates, re-parses content, regenerates embeddings where needed, and updates the index. Otherwise, it's possible that the agent is retrieving information correctly, but just the document it's retrieving it's outdated. And finally, something you need to think about in production is security and auditability. At production scale, we want authentication and authorization around every request, encryption for data in transit and at rest, and secrets to be kept outside the

**[16:19](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=979s)**

prompts and code. And an audit trail for sensitive actions, which we already designed in the observability section. And for our support system, we should be able to answer questions like which customer triggered the request, what information did the agent access, what tools did it call, and who approved a risky action, because all of this is super important in order to audit a production application that runs at scale. The final layer is about how you would optimize the architecture when you see special circumstances, when you see outliers, etc. So, I also built this last layer specifically for interviews,

**[16:50](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1010s)**

because that's where interviews are really looking for signal as to how well you can understand different design decisions and make architecture tradeoffs. Now, that layer is what separates your great candidates from good candidates. So, let's talk about layer five, optimization. Now, like I said, optimization is pretty much how you take a base system that's already working and improve it based on the patterns and problems that you're actually seeing. Now, the four most common dimensions of optimization in production systems are latency, cost, quality, and specific failure patterns, then identifying the bottleneck. Now,

**[17:21](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1041s)**

let's say latency is a problem. If the retrieval is too slow, maybe we need a faster index, fewer retrieved candidates, or a lighter re-ranker. If the model call is taking up most of your time, then the optimization here could just be moving to a smaller model while keeping the same evaluation bar. If tools are slow, we can remove unnecessary tools or run independent tool calls in parallel, and these kind of things. Usually in interviews you're given scenarios where some of these problems are mentioned, and you need to come up with the right optimization. Now, let's go to the next optimization

**[17:52](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1072s)**

dimension, which is cost optimization, which is your system is working well, but you need to think about ways to reduce the cost. Now, here you can use concepts like model routing, which is routing easier requests to smaller models and reserving the larger models for more complicated requests. You can also think about designing your application to use benefits of prompt caching, where stable parts of the prompt, like say system instructions, tool definitions, et cetera, can be reused, and you're charged much lesser. Or you can implement semantic caching, which means that you get repetitive

**[18:23](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1103s)**

questions or questions that are already being asked, you can cache those answers and not even call the LLM again, and pretty much serve the previous result to the customer. And then finally, let's say you want to do quality optimizations. For instance, if your evals show that the system is not good enough, you need to be figuring out where the quality loss is happening. Maybe the model isn't strong enough for one class of requests, or maybe the tool flow itself has to change, right? Moving to a stronger model can be one of the options, but that's only if the model is your bottleneck. Finally, the other dimension of optimization is around

**[18:53](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1133s)**

specific failure patterns. Let's say the support system expands into billing, returns, account security, technical troubleshooting, and all of this, right? Now, one agent might have too much context and too many tools, in which case having specialist agents behind an orchestrator might actually earn its complexity. And this is pretty much what interviewers are looking for, not someone who says at the first shot that they'll use a multi-agent system. Every complexity has to be earned, and you need to explain why you made those decisions. So, someone who really

**[19:23](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1163s)**

understands the base system well enough will understand how to justify that complexity. So, that's pretty much it for this video. I don't really want you to memorize this customer support architecture or what exactly I went over for a trade. You want to take the questions from each of the layer that we discussed and work on a different use case and practice this entire thing so that you get better at system design over time. Like I mentioned in the beginning, I've used the same framework for nine other commonly asked AI system design interview questions. Walk through

**[19:54](https://www.youtube.com/watch?v=Mv7eiQFQQ1E&t=1194s)**

the same framework and see how you're landing. The solution is also there in the book. It's also on my GitHub repository, and I'm definitely sure you'll be ready to tackle any AI system design interview question. The book I have is far more comprehensive than this and far more technical because there's only so much I can cover in a small video. But hopefully, this was helpful, and I will see you in the next one. If you haven't already, please do subscribe. It really helps support my work. All the very best.
