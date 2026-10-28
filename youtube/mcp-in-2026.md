# How MCP Works in 2026 Explained For Beginners

**Published YouTube description:** [Read the full description](descriptions/mcp-in-2026.txt)

**Watch:** [How MCP Works in 2026 Explained For Beginners](https://www.youtube.com/watch?v=ro3CiyWcCT0)
**Published:** October 15, 2026

## In this video

- **What MCP is and why it exists**: a common way for AI applications to discover and use external tools and data
- **How it works under the hood**: hosts, clients, servers, and an email request followed from the model to the underlying API and back
- **Local and remote servers**: where the integration runs and how the client connects to it
- **APIs, MCP, skills and plugins**: what each one does and how they fit together
- **The four changes in 2026**: stateless requests, long-running Tasks, stronger authorization, and capabilities for production use

## Visual cheat sheet

![MCP in 2026: foundations, architecture and the four changes](images/mcp-in-2026.png)

[Open the full-resolution graphic](images/mcp-in-2026.png) · [Read the companion cheat sheet](mcp-cheatsheet.md)

## Resources

- [MCP documentation](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture), for the host, client and server architecture
- [July 2026 specification changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog), for the protocol changes covered in the video
- [Multi Round-Trip Requests](https://modelcontextprotocol.io/specification/2026-07-28/basic/patterns/mrtr), for requests that need more input before they can finish
- [MCP Tasks](https://modelcontextprotocol.io/extensions/tasks/overview), for long-running operations and durable task handles
- [Authorization specification](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization), for identity, credentials and access
- [MCP Apps](https://blog.modelcontextprotocol.io/posts/2026-01-26-mcp-apps/), for interactive interfaces inside supporting hosts
- [Enterprise-Managed Authorization](https://blog.modelcontextprotocol.io/posts/enterprise-managed-auth/), for access through a company's identity provider
- [Agent Skills](https://agentskills.io/home), for reusable instructions, resources and scripts

## Sources and clarifications

- **Launch:** [Anthropic's MCP announcement](https://www.anthropic.com/news/model-context-protocol), November 25, 2024.
- **Usage growth:** [Agentic AI Foundation, September 1, 2026](https://aaif.io/blog/mcp-usage-surged-as-the-protocol-went-stateless). ChatGPT users' MCP tool calls grew 98 times from January to August 2026. Railway reported daily MCP user growth of more than 500% from July to August. These are separate measures, not a single growth rate for the whole ecosystem.
- **2026 protocol changes:** the [2026-07-28 changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog) documents the stateless design, MRTR, Tasks moving into an extension, authorization changes, caching, request headers and deprecation policy. Earlier clients and servers may still use earlier protocol revisions.
- **Tasks:** the [Tasks extension](https://modelcontextprotocol.io/extensions/tasks/overview) uses `tasks/get` for status and results and `tasks/update` for additional input. Client and server must support it. Cancellation is cooperative, not a guarantee that work stops immediately.
- **Security:** [NSA, Model Context Protocol: Security Design Considerations for AI-Driven Automation](https://media.defense.gov/2026/Jun/02/2003943289/-1/-1/0/CSI_MCP_SECURITY.PDF), May 20, 2026. Authorization improvements do not eliminate prompt injection, tool poisoning or inappropriate tool use.
- **Examples:** the Gmail integration and event-reminder workflow illustrate the architecture. MCP servers can be built by providers, third parties or your own team. Skills and plugins can work with or without MCP.

## Transcript

_English captions from the published video. Automatic caption wording is retained; timestamps link to the video._

**[00:00](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=0s)**

MCP is blowing up yet again in 2026. MCP calls from ChatGPT users have apparently multiplied 98 times [music] just from January to August this year. And companies are reporting more than 500% growth in usage of their MCP servers just in the past few months. And I think a big reason for this interest is all of the recent updates. And right now, MCP looks very different even just compared to the past few months. If you're a beginner confused by all of these updates happening to MCP and

**[00:31](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=31s)**

wondering where should you actually start, then this is the only video you need. We'll go all the way from the foundations to the current state of MCP in 2026. Here's everything we're going to cover in today's video. You can take a screenshot or find the HD version in my GitHub repository as well. Now, let's first understand what MCP is and why it exists, right? Because that sets a foundation for everything else we'll discuss. So, MCP or Model Context Protocol is pretty much an open standard that gives AI applications a consistent way to connect with external tools and

**[01:01](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=61s)**

data. It's open source in nature, but it was first introduced by Anthropic [snorts] in late 2024. And for a good part, for a few months, it wasn't really that popular. Now, what started the momentum was the agentic AI wave in early 2025. And remember, the whole promise of agents is that you're no longer just chatting with a model or having a conversation, you actually wanted to go and do some work for you. And also remember, a lot of everyone's actual work lives in external applications like [music] emails, calendars CRMs databases GitHub

**[01:31](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=91s)**

whatever, right? We mostly use a lot of our tools in our everyday work, and agents had to communicate with these tools or applications in some format. So, as agents started becoming more popular, MCP also started getting a lot more attention, and it accelerated [music] even further in mid-2025 when major companies like OpenAI and Google started supporting it as well. Now, when I start talking about this, it raises a pretty obvious question, right? App integrations and APIs have been there for years. Why do we need to introduce this whole MCP layer? All your favorite

**[02:02](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=122s)**

tools, you know, your Gmail, Stripe, GitHub, whatever, they have APIs already. So, what is this new standard adding? Now, remember that APIs were primarily designed for developers writing software. So, let's say I'm an engineer who's building an application that sends automatic emails to all my subscribers whenever I have events coming up. And I'm using the Gmail API to send those emails. Now, I can go and read Gmail's API documentation, understand the [music] operations it exposes, understand what information each request needs, and write deterministic software that creates the

**[02:33](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=153s)**

right request. Now, think of the post-AI world, right? Let's say I want my agent to do that work instead, [music] which is I communicate with it in natural language and it sends out emails to everybody who's going to be attending my events. Now, the agent has to replace some of the work that I was doing previously, right? [music] It needs to have context of what the Gmail API can do, what kind of operations are available to it, and what kind of arguments to be put into a request so that [music] it can send out request to that API. So, in short, the API already gives us the ability to connect to

**[03:03](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=183s)**

Gmail, but there's still a lot of context that the model needs before it can just go use that API reliably. [music] So, when developers started building agents, they started building that middle layer themselves. You'd expose a set of tools to the model and essentially just say that these are exactly all the actions you can take with the Gmail API. Send email is probably one of them. Read email is probably the other. And these are the exact arguments you need in order to create that request. For instance, for the send email operation, you probably need the sender's email, the receiver's

**[03:34](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=214s)**

name, and the receiver's email, and pretty much informing it about the request structure, [music] right? So, the agent has context that it needs. It can look at the user's request, identify [music] the right tool for the job, construct a structured request with the right arguments. And the software deterministic code underneath can translate that into an actual Gmail API call. [music] and the response from Gmail comes back through the same layer to that model. Now, one problem that came up when developers were building this way is that there wasn't a standard way to build this middle layer, right? Now, remember that different AI

**[04:05](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=245s)**

applications and frameworks could describe tools differently, discover them differently, and invoke them differently, and the return results are also different. So, everybody was essentially building their own version of that bridge between the model and the API. [music] And that's the problem MCP is trying to solve. It standardizes this layer so that AI applications have a [music] common way to discover external capabilities, understand how to use them, and then receive the results. Now, there's also another side to this coin, which I want you to keep in mind, which is the overhead of security and

**[04:36](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=276s)**

authorization. Now, the underlying APIs might already support things like OAuth, API keys, and other authentication mechanisms, but the AI integration still needs to connect those credentials and permissions into the agent [music] in order to have a safe experience. And MCP has increasingly standardized parts of that connection as well. Every integration doesn't have to go and reinvent the whole authorization flow independently. Now that you know why we need MCP, let's go a little deeper and understand how MCP actually works under the hood. MCP uses what we call a

**[05:06](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=306s)**

client-server architecture. Now, there are three terms you want to know and understand. The first is the host. This is the larger AI application that users actually interact with. Think of something like your Claude code, Codex, whatever cursor or harnesses like or even just your ChatGPT app. And remember that the host is an application which does much more than just the MCP, like managing model, managing the context, permissions, and all of that. And MCP is a small part of that experience. Now, inside that host, there is an MCP

**[05:36](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=336s)**

client, which is essentially the part of that larger application that handles all of these MCP connections. Now, what it does is it knows to communicate with external applications like your Gmail, Stripe, GitHub, everything that we are talking about. It also understands what kind of operations they expose, sends requests, and also receives responses, right? So, it's pretty much this entity that's sitting within the host that handles all of the communication. And the third part is pretty much what the client communicates with, which is the MCP server. Now, the MCP server is what

**[06:06](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=366s)**

exposes the capabilities of an external application using MCP conventions and standards. And it also handles communication with the client. Now, if all of this seems like too many words at once, let's take an example [music] to understand, right? The same example that we are using. This whole idea of sending an email to all of your event registrations. Now, you pretty much open Codex and say, "Send an email to everyone who registered for tomorrow's event." Now, Codex is the host, and inside [music] Codex, its MCP client is connected to the Gmail MCP server. And Gmail's MCP server exposes what tools

**[06:36](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=396s)**

are available, [music] what each one does, and what arguments they require. Codex can then make that information available to the AI model inside it, and the model looks at your request, [music] figures out which are the tools it needs to handle that request, and generates the required request, right? That has to go to the Gmail MCP server. And all of this communication is handled by the MCP client. Now, the Gmail MCP server takes the standardized MCP request and translates [music] it into whatever Gmail's actual API expects. Now, the API pretty much performs the action, sends

**[07:06](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=426s)**

the response back, and that result comes back through the MCP server and the client to Codex. So, two hops, basically, which is server to client, and client back to host, which displays it to the user. [music] And the whole point here is that MCP as a protocol gives us a standardized way to do all of this without having to build custom [music] components. Now, I want you to remember one more thing here because we'll bring this back when we talk about the changes in 2026 to MCP. The original MCP life cycle had four ideas: initialization, discovery,

**[07:36](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=456s)**

request and response, and session. Let's talk about [music] each one of them. Now, first is initialization. Now, the MCP client and server would essentially introduce themselves, agree on which version of MCP they were speaking with, and exchange information about what each site supported. And then there would be a discovery phase, which is the client would ask the server what it actually exposed. So in our Gmail example, the server might return tools for searching, for reading, etc. Along with what each one does and what arguments it expects.

**[08:06](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=486s)**

And only after this handshake is when the actual request and response [music] stops happening. So the AI model decides which capability it needs, the client sends that structured request over to the MCP server, and the MCP server completes that work and hands it back as a structured result. And finally, remember that the MCP server could create a session during the initialization and give the client a particular session [music] ID. And you can think of the session ID as the server's way of knowing where a request is coming from and which ongoing interaction it belongs to. >> [music] >> So when the client is having follow-up

**[08:37](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=517s)**

conversations, it would include that session ID so that the server could associate them with the same [music] session. So pretty much the MCP life cycle would be initialization, discovery, the request and response, and also having a session ID for follow-up requests. Now MCP is also usually confused with a few other terms in AI and let's clear that out before we move on. So let's learn about MCP versus APIs versus skills versus plugins today, right? In fact, you can think of all of these terms as layers in the same system and you can almost visualize an API

**[09:07](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=547s)**

sitting at the bottom, MCP on top of it, and skill about that, and a plugin as a package that can host different skills [music] and MCP servers. Let's start with APIs, right? Basically, if an AI system needs to interact with an external application, [music] the bottom layer is still the API. MCP doesn't replace that. It sits on top of it and gives the AI system a standardized way to discover and use those application capabilities. Now the MCP server can translate between the standardized MCP request [music] on one side and whatever

**[09:37](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=577s)**

the application's API expects on the other. And a layer above that is where the skills ecosystem sits. To put it very plain, a skill contains reusable knowledge about how a particular task should be done. For instance, [music] it can contain instructions, procedures, examples, scripts, or any other context that helps an agent follow a repeatable workflow. I want to take you back to the email example, right? Because it just follows a nice continuous [music] thread. Let's just say you're sending the event reminder email, you could build an event reminder skill that teaches the agent [music] how your

**[10:07](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=607s)**

company writes emails. Reminder emails pretty much. Maybe every reminder needs the event link, the name, the location, what attendees should prepare, and a particular brand voice essentially. So, that skill teaches your agent how the task should be done. And when the agent actually needs [music] to send the email, it can use the sending capability exposed through the Gmail MCP server. [music] And underneath that MCP server is Gmail's API. So, it's possible that the skill is using the MCP server, which [music] is calling the API. And finally,

**[10:37](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=637s)**

there's this idea of plugins, which is mostly something you would have heard of if you work with the OpenAI ecosystem, because tools like Codex and [music] ChatGPT work, etc., have the idea of plugins. And a plugin isn't really a competing standard, it's just a packaging [music] unit. So, when OpenAI shipped their plugin marketplace back in April 2026, they had about 90-plus plugins. [music] And all of these were essentially bundles of skills that could potentially also use MCP servers. And they were available as a single installable package. So, if you install the Gmail plugin in Codex, [music] it's

**[11:07](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=667s)**

essentially the Gmail MCP server plus the workflow knowledge around it, wrapped up so that you can install one thing instead of wiring a bunch of different things. And OpenAI also has plugins for data science [music] use cases, for product management use cases, product design use cases, and all of that. And they're pretty much bundles that contain [music] both skills and MCP servers wrapped up. So, the simple mental model I want you to think of is API is how the software interacts with the external application. [music] MCP is the standardized connection layer that lets the AI system discover and use

**[11:38](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=698s)**

those capabilities. The skill is the workflow layer that teaches the agent how a particular job should be done. And finally, if you've heard of the word plugin and you're confused in the context of AI, a plugin is a bundle that ships you multiple skills and workflows, which might or might not use MCP servers. All right, now let's go to the most interesting part of this video, which is what has changed in 2026 in the whole MCP space. Now, the first major change that happened in MCP is that it became stateless. Now, what does that

**[12:08](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=728s)**

actually mean? Remember the four pieces that we just learned as part of the MCP life cycle, initialization, discovery, request and response, and all of the session ID thing? So, the client and server would initialize their connection, and then the server could create a session, and future requests carried that session ID so that the server knew that they belonged to the same ongoing interaction. That's a TLDR, right? But what started happening is when they were popular MCP servers, let's for instance say Gmail MCP server, right? It's probably getting hundreds of thousands of requests. [music] There's not going to be one server

**[12:39](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=759s)**

handling every single request. Companies will essentially have multiple server copies and instances handling that load. So, say the first request from the user reaches server one, for example. That server knows the state associated with that session. And state here is what happened so far and what's been done. And the next request from the same user happens to reach server seven, and server seven does not have the state this old server was maintaining. Now, there are basically two ways around this. One is you keep routing that client back to [music] the same server,

**[13:09](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=789s)**

or you make sure that all server instances have the same shared session state. And both of these kind of add infrastructure complexity because the protocol itself expects a server to remember this ongoing interaction. So, one of the [music] biggest changes that happened in July 2026 was removing that entire requirement. Now, that whole initialization pretty much [music] went away and the concept of session ID also went away. Every MCP request now is self-contained and it's contained at the protocol level. So, instead of introducing itself and expecting that the server will remember that

**[13:39](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=819s)**

information later, the client includes that information every [music] request. And this small change actually makes MCP much more easier to scale. But, it does introduce a problem, which is kind of interesting and also is solved in the latest spec, which is what if the server starts processing your request and realizes that it probably needs something from the client before it can finish. Say, in our email example, if you ask it to email everyone registered for an event, but before sending out hundreds of emails, the server wants you to confirm that action. And if we no longer have the ongoing session ID, how

**[14:11](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=851s)**

does it stop and ask for any input? Now, that's where this whole idea of multi-round trip requests was introduced to solve this issue. It's also called MRTP. And the whole thing here isn't that this is not a callback where the server somehow reaches back out later. The first request that was sent is actually finished [music] and it closes. What the server actually sends back is a special kind of response. So, instead of a normal result, it comes back flagged as input required and that flag is a signal to the client that the work isn't

**[14:41](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=881s)**

finished [music] yet and some other input is needed from the user. Along with that flag, the server also sends back a small piece of request state, which is essentially its own note to itself as to what had happened and what it's looking for. So, that even if the server changes, it goes from say server one to server 10 or 15 or whatever it is, the new server has all of the context. So, MCP pretty much moved away from keeping an ongoing protocol session and towards independent requests while still giving us a way to handle interactions that need maybe more than one round. Now, the second major change

**[15:11](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=911s)**

in MCP is this idea of MCP tasks and this one pretty much comes from how agents themselves have evolved over the past year. Remember when MCP was first designed, a lot of agent interactions look much closer to normal tool calls. They would be very short-lived. For instance, search something, read something, or just send something, [music] and the answer comes back pretty quickly. But now, in 2026, the scope of agents have become much larger. They take on longer pieces of work. For instance, they can analyze large code bases, process thousands [music] of

**[15:41](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=941s)**

customer conversations, run probably a deep research workflow, or essentially anything that can take sometimes minutes or even hours. And you really don't want these MCP requests be sitting open the entire time waiting for the work to finish, which is why the idea of an MCP task comes in, which is the server can decide if the incoming request should be treated as a task, [music] and instead of returning the final result immediately, it pretty much hands back a task handle, [music] and the client can use that handle to keep checking at

**[16:11](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=971s)**

regular intervals. Now, the third major change is that security and authorization in the protocol became much more stronger recently. Now, as MCP's adoption grew, people started connecting agents to their email, to their code, CRMs, databases, and a lot of other system containing sensitive or private information. So, their identity, credentials, and permissions started mattering a lot more than before. Now, the latest July 2026 spec tightened the authorization model. For instance, clients now have a much stronger way for

**[16:41](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=1001s)**

verifying which authorization server actually issued a credential, >> [music] >> and credentials are also tied to the issuer that created them. And MCP also moved away from having clients dynamically register themselves with every authorization server, and towards [music] client metadata documents, which gave authorization systems a much more predictable way to identify clients. And MCP had this notion of enterprise-managed authorization, but it became much more stable in 2026. [music] And the whole idea of enterprise-managed authorization is that if a company has,

**[17:12](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=1032s)**

say, thousands of employees, the company can define a set of approved MCP servers [music] and centrally monitor this instead of every user having to approve a bunch of servers. So, all of the vetting can be done at a company level. [music] And remember that these authorizations are pretty cool. They're much better than the older versions, but [music] they don't magically solve all of your security problems, but they do strengthen the protocol much more and make it harder for bad actors to come in. Now, the last and fourth shift is that MCP became much more production [music] ready in the past few months and this is a bunch of different updates all

**[17:43](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=1063s)**

together, not one major update, but let's talk about three popular ones. One is this idea of extensions. [music] So, MCP now has a formal extension system. So, instead of putting every new capability directly into the core protocol, developers can put optional capabilities separately and have a modular way of building this. [music] And everything that we went over so far, MCP tasks, MCP apps, and enterprise manager authorization, they all live as extensions. And every server and client doesn't have to support every possible feature, which brings me to the second

**[18:13](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=1093s)**

update, which is MCP apps. So, MCP can now support richer [music] interfaces instead of always returning just text or data. For instance, a server can hand back an interactive [music] interface that a supporting host can render. So, an analytics MCP server, for instance, can return [music] a dashboard or a workflow can return a form or a CRM tool, for example, could return an interactive customer view. So, it's not just text or data anymore. And the third more operational update is around caching and easier request handling. Tool lists and other list responses can

**[18:43](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=1123s)**

now include caching information so that [music] clients don't have to keep fetching the same catalog again and again. All right, so that's about all of the updates. Now, if you kind of step back and look at it from a 10,000 ft view, now the pattern across all of these four changes is the fact [music] that the mindset with which MCP updates are being done is completely different in 2026. In 2025, it was just a standard way in order to connect to external tools and applications. It was pretty early and the whole idea was to just provide people the functionality and in

**[19:13](https://www.youtube.com/watch?v=ro3CiyWcCT0&t=1153s)**

2026 the focus is pretty much shifted into what happens when all of these connections start happening and they start happening at scale and in production style applications where agents are doing longer pieces of work. They're working with much more sensitive data and companies actually depend on this working reliably well so that they can run their operations. Hopefully that answers all of your questions on MCP and you now can understand better how it works. I will see you in the next video with more such deep dives and do subscribe if this kind of content is interesting to you. All the very best.
