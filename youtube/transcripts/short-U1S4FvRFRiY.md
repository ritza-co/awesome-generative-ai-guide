# The Context Window Problem Nobody Talks About #ai #contextwindow

[Watch the Short](https://www.youtube.com/watch?v=U1S4FvRFRiY)

_English captions from the published Short. Automatic caption wording is retained._

**[00:00](https://www.youtube.com/watch?v=U1S4FvRFRiY&t=0s)**

Have you noticed how the longer you chat with agents like Claude, the more they seem to forget things that you've already told them. Now, this can happen because of a feature and kind of a bug called context compaction. And how that is set up decides whether your instructions will survive. So all AI models, they can take a limited amount of information at once. That limit for every AI model is called a context window. Now, as your chat grows, your messages, your documents, your tool results start filling up that window. So eventually the application like clot core has to compact the conversation so that the agent can keep going. There are three common context compaction algorithms and each one handles the

**[00:30](https://www.youtube.com/watch?v=U1S4FvRFRiY&t=30s)**

older information in a different way. First, sliding windows. Here you keep the most recent messages and drop the older ones. It's simple and needs no extra model call. But unless you preserve important instructions separately, they can pretty much disappear just because you gave them earlier in your chat. Now, a 2026 study measured the consequence of this. With the rule in context, there were zero violations. But with sliding window context compaction, the agents violated it 38% of the time. Now, the second method is called observation masking. Now, observations refer to what comes back from tools, search results, file contents, test logs, and all of this

**[01:00](https://www.youtube.com/watch?v=U1S4FvRFRiY&t=60s)**

which are not part of the conversation. And in this algorithm, you replace the older tool outputs with placeholders and keep the surrounding conversation in actions. Now, this helps save space, but the agent needs a way to retrieve those details again if it turns out that it needs them. And the final and most commonly used compaction algorithm is summarization, which essentially is asking another model to summarize older history, keep recent messages intact, and update the summary as the conversation goes. This can help preserve decisions and progress where anything the summary leaves out can disappear from the agent's working context. Now remember the study that I was talking about in the same study

**[01:30](https://www.youtube.com/watch?v=U1S4FvRFRiY&t=90s)**

agent still violated the rule 26% of the times even after applying the summarization algorithm. So how do you actually keep those instructions from disappearing if you were building an application? Turns out that keeping the original instructions word for word outside the compaction brought violations back to zero in their benchmark and those set of pinned instructions took just about 47 tokens. So if you're building agents, understanding compaction and how to design it well is definitely something you need to master. You must decide what should stay as it is, what can be summarized, and what the agent can retrieve later. And also identify some

**[02:00](https://www.youtube.com/watch?v=U1S4FvRFRiY&t=120s)**

rules that can stay out of the compaction algorithm so that it can be read by the agent all the time. But you also need to be testing it to see if it still follows instructions. Does it still complete the task after compaction? Because the agent didn't really forget.
