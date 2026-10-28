# Don't Trust a Big Context Window Yet #ai #llm

[Watch the Short](https://www.youtube.com/watch?v=sVxen1j24Fc)

_English captions from the published Short. Automatic caption wording is retained._

**[00:00](https://www.youtube.com/watch?v=sVxen1j24Fc&t=0s)**

So, researchers at Anthropic hid a dangerous command inside coding agent's activity log and asked an Alpha model to catch it. At 100,000 tokens, Opus 4.6 with thinking and labeling caught it about 99% of the time. And at 800,000 tokens, that dropped to 69%. So, think of it, the command was still there, but the model just missed it. And it missed it a third of the time. And here's why this should matter for you. So many models today come with a million token context window. And that would probably make you think that you can pretty much throw anything at the model to figure it

**[00:30](https://www.youtube.com/watch?v=sVxen1j24Fc&t=30s)**

out. But even when everything fits, it can still miss the information you need. And where that information sits also matters. Now, in the same study, researchers also moved that command to different positions. Turns out, the middle was the hardest place to find it. And remember, this is not an issue just with Claude. It's a known limitation seen across a lot of AI models, and it's called lost in the middle. So, turns out, models tend to use information at the beginning and end of a long input way more reliably than information that's buried in the middle. And there's research from MIT from 2025 that helps

**[01:01](https://www.youtube.com/watch?v=sVxen1j24Fc&t=61s)**

explain why. They showed how causal attention can favor earlier information and how it interacts with the way models represent position. So, a bigger context window doesn't automatically solve this. So, the next time you're giving a model a long document, pay attention to where you put the information that it needs. For instance, all of the key instructions that you need to give it should go at the beginning or in the end, and avoid burying them in the middle. So, when you're using models with large context windows, you need to pay attention to where you put your important information, not just whether it fits. Because if context windows get longer, the middle can also get wider.
