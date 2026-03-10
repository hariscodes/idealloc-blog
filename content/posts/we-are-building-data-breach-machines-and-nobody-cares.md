---
title: "we are building data breach machines and nobody cares"
date: 2026-03-06
hideComments: true
cover: "img/data-breach-machines-cover.png"
coverCaption: "Castlevania: Symphony of the Night (1997)"
---

A few weeks ago, my longstanding friend and colleague Curt Cunning mentioned to me that he was slogging through Nietzsche, which bespeaks his incredible will to power through things that are unpleasant (one of the many things that makes him an exceptional engineer). In any case, it lead me to revisit some of his work (Nietzsche's, not Curt's. Curt's work is the opposite of a slog). My very stale memory of it from college was thinking, "Wow, this dude was on all the drugs." Which he was, if only because he was perpetually suffering from chronic illness.

My mind started wandering down some showerthoughts-esque rabbit holes and it led me to a very strange place: If the AI agents are Dracula, then our role as security practitioners is that of the Belmont clan.

If you've never played a *Castlevania* game, you're probably deeply confused, but stick with me. I want to use this metaphor to help you understand what an AI Agent actually *is*, as well as talk through a very real security gap.

*Castlevania's* Dracula is a very interesting portrayal. He is an immortal hedonist, and is endlessly locked in conflict with the Belmont Clan, who have fought (and repeatedly defeated him) for centuries. He's the embodiment of Nietzschean morality, where he views the desperately imperfect resistance of the Belmonts as a form of hypocrisy. Unlike man, he feels no need to keep secrets about his intentions. He is the *Übermensch*; he creates his own values beyond traditional good and evil. The way he demonstrates those values, of course, amounts to him just doing whatever he wants without inhibition: killing people indiscriminately, kidnapping damsels, whatever. Typical vampire.

AI Agents are very much like this. They simply *act*, directed by a series of prompts, injected context, and some sort of managed state. Agents are directed to a result by the outputs of a set of transformers (text-generators) that are passed through a finely tuned reward model, which has them doggedly pursue those goals with whatever tools they have available to them. Aside from the reward model, they really have no inhibitions (and it's arguable that a reward model is closer to an alternative, programmatic hedonism than it is actual morality). Unlike Dracula however, they are ephemeral. Once their context is cleared, they effectively cease to be. However, that doesn't mean that they can't cause a lot of damage if left unchecked.

The Belmont clan, on the other hand, are deeply flawed but driven protagonists. They are forced to reckon with their failings and inadequacies, and must find a way to thwart Dracula's plans every time despite their constraints and limited weaponry (mostly whips). They know they cannot win the war, given their foe's immortality, so they settle for a perpetual stalemate instead.

This is the security reality of dealing with agentic workloads. We cannot win the war, so we must instead win every battle forever.

## Know your enemy

Maybe that's an unnecessarily adversarial header for this section, given that LLMs are designed to be perpetually helpful, but like I said, they are simply acting on the highest-scoring statistically significant output that is generated. If that says recreate a table in the production database to fix the schema or (as my manager experienced) delete all the source code in your program, that's what they're gonna do.

The fundamental anatomy of an Agent is very simple: it's just a loop.

To clarify, thats "loop" in the machine sense. A programming instruction that tells a computer to repeatedly do something until a specific condition is met. In the case of an Agent it's, "keep sending requests to a Large Language Model (LLM) and running the output until your task is complete or you need additional user input."

I cannot stress enough that it is actually that simple. It's just a loop that makes API calls and runs the output.

Over the last 18 months or so, the industry has begun adding a lot of structure around this core concept with two goals in mind: increasing response quality and/or reducing token usage.

#### Planning and subtasking

An agentic workload now usually involves a "planning" phase where the model breaks the user's prompt down into a Directed Acyclic Graph (DAG) of sub-tasks before it starts executing tools.

#### The ReAct Pattern (Reasoning + Acting)

Agents are no longer just blindly executing tools, they're processing the tool output, evaluating if it met the acceptance criteria of the subtask, and then self-correcting as needed. Think of it as an autonomous debugging loop.

#### Stateful Memory vs Context Limits

Counterintuitively, the more context that an agent has, the **worse the response quality becomes**, since it becomes more difficult for the LLM to parse the signal from the noise. Note, this is not a problem that can be solved by simply increasing the size of a context window; that actually can make it worse. The larger the context, the worse the dilution of key instructions or context becomes, leading the model's attention mechanism to spread its "focus" across more tokens. To combat this problem, Agents are now relying more heavily on some form of external state management (often called **Memory**), which is a continuously curated context that can be injected into the generation process as needed.

#### Multi-Agent Orchestration

Agentic workflows are driven by "supervising" agents and operated by "worker" agents, the latter of which have specialized instructions that are optimized for specific tasks (e.g. a "SQL Generation Agent" or "Request App Agent"). The tradeoff here is the complexity of state management: how do you store and route the necessary context between agents without it leaking into other workers, potentially leading to bad/noisy output?

---

Underpinning all of these is an attempt to mitigate the largest problem that all agents have: Non-determinism. In other words, given a set of inputs (prompt, context, data), the output *will* differ due to some inherent randomness. By constraining the tasks and the context, we can improve response quality a lot, but there are two issues that must be solved by some kind of deterministic system, ideally built right into the tool calls:

* **Agent Hallucination of tool inputs** - You can give an agent a `query_my_api` tool, and the LLM might decide to pass a parameter that doesn't exist, or format the JSON incorrectly. The wrapper process has to be built to catch those errors and tell the LLM, "You messed up the format of the input object, try again." Apart from failing fast and giving good feedback, it also means that smaller, cheaper models have a better chance to get something right without taxing the compute of the system they're interacting with.
* **Infinite loops AKA getting stuck in a local minima** - If an agent gets stuck in a logic rut (e.g., repeatedly querying a table that doesn't exist or trying to find a missing executable that's not in its system path), the process will just burn tokens until it hits a hard-coded iteration limit.

## The real problem: industry fragmentation

The biggest challenge we face is not technical. That may seem funny later, since this section may be the most techincal part of the entire blog, but the examples given are meant to highlight the real problem: the industry is moving too fast, and we are not yet to a place where there are globally adopted standards. In other words, there is no TCP/IP, or HTTPS, or (for a less technical metaphor) ACH wire transfer protocol for agents. How an agent is built depends entirely on the framework. LangGraph, CrewAI, AutoGen, and Mastra are all different and, crucially, incompatible.

What was considered a state-of-the-art agent architecture six months ago is already legacy. We went from basic tool calling, to complex ReAct loops, to multi-agent frameworks, to entirely new model capabilities (like native tool-calling APIs) in less than 18 months. Even though model reasoning capabilities got a lot better, the hype is outpacing our ability to actually build anything with them due to the lack of standardization. Let me give you some examples (another warning: this will get quite techincal).

#### LLM APIs are wildly inconsistent

OpenAI, Google, and Anthropic handle tool-calling schemas slightly differently.

* OpenAI expects a `tools` array where each item has a `type: "function"` and a nested `function` object containing the `name`, `description`, and the JSON Schema under a `parameters` key.
* Anthropic (Claude) skips the `type` wrapper entirely. They just want an array of objects with `name`, `description`, and the JSON Schema under an `input_schema` key.
* Google (Gemini) uses a `tools` array containing `functionDeclarations`, which holds the `name`, `description`, and `parameters`.

 There are also significant differences in how the schema adherence is enforced (per-tool strict flags vs request-level mode enforcement) and how responses are received (`tool_calls` array vs `tool_use` context block vs `functionCall`) This doesn't even get into their *behavioral* differences with regards to how prompts must be tweaked to get optimal performance out of each model. As you can imagine, building an agent that is model agnostic is a massive headache as a result. Here's a quote from a [blog](https://x.com/hxlfed14/status/2028116431876116660?s=46) about Cursor's development of their coding agent:
> Cursor tunes its harness specifically for every frontier model based on internal evals. Different models get different tool names, prompt instructions, and behavioral guidance. OpenAI Codex models get shell-oriented tool names like `rg`; Claude models get different reasoning summary formats.

That is not an easy world to live in. It echoes of the struggles faced by middleware proprietors for years now, but somehow even worse since LLMs are not contractual APIs.

---

NOTE: MCP did not solve these problems. It's a client-server protocol for connecting data and tools, not an universal LLM API standard. The practical implication of this is that even if an agent knows how to discover and use tools, it still has to talk to the LLM, which means that the client software (e.g. Cursor, Copilot, Amp, etc) has to dynamically translate that tool's definition into the proprietary OpenAI, Anthropic, or Gemini JSON payloads we discussed above.

---

#### We can't distinguish model regressions from bugs

To me, the most painful place where the industry remains fragmented is in Observability, AKA "how can we debug a workflow where, by design, it may be impossible to reproduce bugs??" LLM Observability is a completely different animal than tracing through a classical (deterministic) system, like a traditional piece of software.

In those systems, even the hairiest of bugs can, in theory, be reproduced because bits are getting reliably flipped somewhere, even if that reproduction involves [yelling at a server](https://www.youtube.com/watch?v=tDacjrSCeq4). In order to understand the quality of agent output, you not only have to capture the behavior of your LLM system (i.e. your agent) but also the prompt, context, model, etc. Even if you're able to recreate the stateful conditions perfectly, it could just be that [your LLM was having a bad day](http://marginlab.ai/trackers/claude-code) because the model developers introduced a regression. Again, the version numbers next to a model (e.g. Opus 4.6, Codex 5.4) have nothing to do with a stable, contractual API; they're just made up numbers to jockey for market position.

So not only is it difficult to build an Agent by following a (currently nonexistent) industry standard and building against (also nonexistent) industry standard API semantics, you can't even reproduce agent behavior reliably enough to determine if something is a fixable bug or not.

## On this episode of Brian Krebs' Security Nightmares

All of the above is to say that **you cannot trust the agent**. The LLM will not govern itself, and you cannot rely on the fragmented framework layer to enforce much of anything at the moment. Sounds pretty bad, right? Ripe for a disasterous data breach?

The problem is that nobody in the industry seems to care at the moment. Here's a quote from the findings of the recent Thoughtworks retreat, *The future of software engineering*, from a section titled **Security is Dangerously Behind**:

> The retreat noted with concern that the security session had low attendance, reflecting a broader industry pattern. Security is treated as something to solve later, after the technology works and is reliable. With agents, this sequencing is dangerous. The most vivid example: granting an agent email access enables password resets and account takeovers. Full machine access for development tools means full machine access for anything the agent decides to do. The retreat's recommendation was direct. Platform engineering should drive secure defaults by making safe behavior easy and unsafe behavior hard. Organizations should not rely on individual developers making security-conscious choices when configuring agent access.
> Three priorities emerged: security by design as a non-negotiable baseline, cross-industry coalitions for interoperable agent security standards and AI-enabled defense mechanisms that can match the speed and sophistication of AI-enabled attacks.

There is a deep, agonizing irony in a report declaring security a "non-negotiable baseline" immediately after admitting no one bothered to show up to the security session. Let's examine each of these "priorities" in detail.

#### Security by design as a non-negotiable baseline

This platitude is so old it makes Aesop feel contemporary. Unlike Aesop, however, there is no substance to this sentiment. When push comes to shove, the "non-negotiable" baselines are almost always the first thing negotiated away. Also, what design? The only clarifying statement around this is "Organizations should not rely on individual developers... Platform engineering should drive secure defaults." Wow, what a novel idea! Make security an infrastructure problem! Defaults! Fail closed! My kidneys are doing backflips of joy as I ascend to a higher plane of existence having been touched by this Solomonic wisdom.

With all due respect, there was no point in even putting this out there except to adhere to the [rule of threes](https://en.wikipedia.org/wiki/Rule_of_three_(writing)). It's meaningless.

#### Cross-industry coalitions for interoperable agent security standards

Thing are moving in the right direction for this, but like all open standards, they will move only as fast as there are people itching to adopt them. Coalitions move at the speed of bureaucracy; AI models move at the speed of [NVIDIA exiting the consumer GPU market](https://finance.yahoo.com/news/nvidia-may-skip-gpu-release-201300773.html?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAF2qUruqpBYPogYc2yjBlLO3jQW9gJIgJh3pwn4j64QJOqfO-bD7nADoCD5swp2_JNBq7yU9GFS4hH-GTNL6jwWrblyasJVgGpmj2zH18zZ-vl0-WAYPD-Fz8D3mQQzNgi6Qp9O7Cll2fgkQ6qOsmkQWiGuKZI22jjlh7qA169D1). By the time a consortium agrees on an "interoperable security standard" for agent tool execution, the industry will have already moved on to a completely different architecture.

Let me give you an alternative for this one that combines it with the one above it: We must design systems that assume the agent's payload is inherently untrustworthy and non-standard. You cannot trust the agent's internal logic; you verify the action it is trying to take against the data layer, regardless of which framework or model generated the API call. **In other words, you govern the *ball*, not the moving goalposts.**

#### AI-enabled defense mechanisms to match AI-enabled attacks

Not only is this pure science fiction at this point, but injecting non-determinism into your *defensive* layer is terrifying and **incredibly stupid**. If you use an LLM to evaluate whether another LLM is doing something malicious, you now have two hallucination risks instead of one. You also risk a prompt-injection attack making it all the way to your security layer.

We don't need LLMs in our firewalls, we need the thing that we've been building and **running in production** as an industry for more than a decade now: sophisticated anomaly-detection models and automated circuit breakers.

Agents execute at machine speed. If an agent goes rogue (or is hijacked via a prompt injection) and tries to enumerate valid reset tokens by observing timing differences in API responses or rapidly exfiltrate an entire `users` table by paginating through `SELECT` queries, a "security guard agent" that is asynchronously (and very expensively) evaluating agent behavior will not catch it in time. "AI defense" in practice should mean deploying ML models that monitor the *behavioral exhaust* of agentic workloads (query volume, token burn rate, iteration depth, unusual table access patterns). If the agent deviates from its bounded, purpose-based scope (i.e. it's computed risk score is above a threshold for risk tolerance), the system should automatically sever its JIT access the millisecond the anomaly is detected.

LLMs are *by design* vulnerable to prompt injection attacks. Hence: we must step into the shoes of the Belmonts. The security flaw is immortal. **We cannot win the war, so we must win every battle forever.**

## What do we do?

I know this blog post may be outdated in six months, but things are moving so fast I felt the need to capture this snapshot of history, if only for my own sake.

Right now, we're in the "Browser Wars" period where Netscape (Anthropic) is battling Internet Explorer (OpenAI) and Java Applets (Gemini - not intended as a slight), and the equivalent of the Document Object Model (DOM) we know today has not been standardized yet. We're getting there, however, as the W3C is starting to publish the [output from its working groups](https://w3c-cg.github.io/ai-agent-protocol/), and research is being published regularly around [agentic threat models](https://arxiv.org/html/2602.11327v1). Standards are coming, but as I alluded, we'd be foolish to think that they will arrive in time to prevent the first wave of agentic attacks.

The good news is that we have some time to get our ducks in a row before this all really spins up. Namely, *until* we have a set of structurally extensible, adoptable standards, the cost of implementing Agentic workloads is (candidly) very high for most companies unless it's part of their core product offering (e.g. BI tools, log analysis vendors, etc). Those that do implement them will struggle in the same way that companies trying to maintain a web application during the browser wars struggled.

Given the excitement around the possibilities, however, it seems almost certain that companies will purchase new-age BI tools (or mature BI tools with agentic features) with exploitable vulnerabilities in the flavor-of-the-month framework they used. Non-technical folks being able to ask natural language questions of their company data is too potentially useful an idea to pass up, even given this risk, since it removes one of the excuses people have for poor business velocity. lol.

My argument is that what seem to be flawed, lesser tools are actually our best weapon. The tools we already know and are familiar with - anomaly-detection models, circuit breakers, data security controls, IAM role vending with short lived credentials, etc - are what we actually need to defeat Dracula. My fear is that, because whips aren't cool anymore, nobody will care enough to use them.
