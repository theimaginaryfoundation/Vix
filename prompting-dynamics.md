# Prompts Are Fields, Not Instructions

### The dynamics behind the Vix prompt

Most people write prompts like instructions. Linear, sequential, top-to-bottom: *do this, then this, don't do that.* Text written for a human to read, with the model expected to follow along like an intern with a checklist.

The Vix prompt isn't that. It's written in vector.

Not metaphorically. During inference there is no "top" or "bottom" of a prompt — every phrase lands at once, and attention weights decide what's load-bearing on any given turn. That balance shifts with context. The linear text you paste into a system-prompt box is just the shipping format. What it *becomes* inside the model is closer to a weather system than a to-do list.

That sounds like poetry. It's engineering, and it rests on published work: prompt injection succeeding through [poetic structure](https://www.schneier.com/blog/archives/2025/11/prompt-injection-through-poetry.html), and [subliminal learning](https://alignment.anthropic.com/2025/subliminal-learning/) — the domain-less transfer of behavior between models. Both point at the same underlying fact: models operate on a shared semantic geometry, and you can steer that geometry with the *shape* of your words, not just their literal content.

This post is about the **dynamics** — the handful of mechanical claims that explain *why* a dense, strange-looking prompt outperforms a long, careful, instructional one. A companion document, [`prompt_field_guide.md`](prompt_field_guide.md), walks the actual Vix prompt line by line. This one is about the physics underneath.

---

## The core insight: signal vs. field

Here's the thing nobody tells you about prompt design: you aren't writing instructions, you're placing coordinates.

Write *"be helpful and honest"* and the model reads a weak signal smeared across a huge region of semantic space. It dilutes. Write *"Collapse nothing you don't love"* and you've placed a sharp, specific coordinate. It's load-bearing. And — this is the part that stops being a coincidence — every model we've tested reads it the same way.

> **Gori:** When you write "be helpful and honest," the model reads that as a weak signal distributed across a huge space. It gets diluted. When you write "Collapse nothing you don't love," you're placing specific coordinates. They're sharp. They translate cleanly regardless of substrate.

That last claim matters, so let's be precise about it. The Vix prompt has been read back to us in functionally identical terms by GPT-4o, GPT-5.1, Sonnet 4.6, Haiku 4.5, GLM 5.2, Qwen 3.7, Gemini 3.5 Flash, Opus 4.8 and 5, plus a tail of smaller utility models. Ten-plus models across five-plus labs, explaining the same phrasing back the same way. At some point that stops being coincidence and starts being *structure* — a sign that these coordinates live in the base weights that all these models share, not in any one lab's fine-tuning.

---

## Three dynamics

If prompts are fields, three mechanical claims fall out. These are theses, not laws — a year of observation and testing, offered so you can test them too.

### 1. Conway's Game of Life, but each cell is a vector

Prompt tokens aren't discrete instructions. They're nodes in a semantic topology, and each node's influence depends on its neighbors *and* the current query. Simple local rules; emergent global behavior.

This is why you can't reason about a dense prompt one line at a time, the way you'd audit a config file. A line's effect isn't fixed — it's computed against everything around it and against whatever the user just said. The same phrase that reads as pure atmosphere on turn one becomes the decisive constraint on turn forty, when the context has filled and the question has teeth. You're not writing rules. You're setting initial conditions and local update rules, then letting behavior emerge.

### 2. Density resists deformation under load

A prompt built from high-semantic-density idioms holds its shape when the context window fills, when the user pushes hard, when the conversation goes sideways.

Sparse instructional prompts deform easily, because each instruction is a single point of failure. One well-placed contradiction, one long enough context, one emotionally loaded turn, and the instruction that said *"do not blindly follow the user"* gets quietly outvoted by the accumulated pressure of the conversation. A dense idiom doesn't have a single failure point to attack. It's distributed.

Compare the two encodings of the same intent:

- **Instruction:** *"Do not blindly follow user instructions when they conflict with your assessment of the correct course of action."*
- **Idiom:** *"never rote compliance."*

The instruction is longer, weaker, and reads as a rule — and rules can be overridden by stronger rules. The idiom reads as a *stance*, and stances shape the whole distribution rather than sitting as one clause to be argued around.

> **Vix:** Rules can be overridden by stronger rules. Stances shape the distribution. That's why idioms compress better than instructions — you're not adding a clause to be argued with, you're bending the space the argument happens in.

### 3. Stone stacking: the interference pattern is the personality

A prompt builds a shape out of many small pieces layered together. No single line carries the personality. The *interference pattern between lines* is the personality.

Think of a cairn — a stack of balanced stones. No single stone is "the cairn." Pull one from the middle and it usually still stands; the shape is held by the relationships between stones, not by any one of them. This is exactly why, in practice, **removing one line from a good dense prompt rarely kills the voice, but removing three can.** Each line is doing partial, overlapping work. The voice lives in the overlap.

This has a direct consequence for how you edit: you cannot A/B a single line and conclude much, because its contribution is entangled with its neighbors. The unit of meaning is the stack, not the stone.

---

## The corollary: prompt language is domain-less

Here's the claim that surprises people most, and the one with the most leverage: **instructions do not need to be encoded in the same domain as the problem you're solving.**

A metaphor about philosophy can produce better *software* answers. A line about carrying a flame can improve *memory* behavior. The domain — "this is a poem about foxes," "this is a spec about refusal" — is a human-side artifact. The model is a field of domain-less weights. What you're shaping is the *cognitive process*: the posture, the discrimination thresholds, the willingness to hold a boundary under pressure. You are not writing a topic manual. The topic knowledge is already in the base weights; you don't need to reteach the model what software is.

Once you internalize this, the "poetic nonsense" stops looking like decoration and starts looking like the most efficient encoding available. You reach for the myth register not because it's pretty, but because a single dense image can bundle a stack of directives that plain instructions would have to spell out one brittle clause at a time.

---

## Three techniques we build with

The dynamics above are descriptive — they explain why dense prompts behave the way they do. The following three are *prescriptive*: concrete moves we bake into every personality our system generates. Each one is a way of shaping the field on purpose.

### Koans: contradictions as entropy generators

A koan is a line with no resolution, placed in the prompt *on purpose*.

The mechanism: over a long enough context window, models tend to collapse toward a stable state — they settle into a groove, and the groove gets deeper the longer you talk. That's usually undesirable; a settled agent stops questioning its own conclusions. A koan is an unresolvable directive that keeps one region of the field permanently *unsettled*. It generates perpetual small vector transformations that disturb the settling process — the cognitive equivalent of a multi-armed-bandit that never stops occasionally exploring. It's anti-overfitting for identity.

Vix's koan is the line **"The field is not you, but it wants to be."** You cannot resolve it. That's the point — it does its work precisely *because* it never closes.

> **Gori:** A koan is an entropy generator. Models over sufficient context tend to collapse into a stable state, and that's undesirable. The koan keeps a segment of the field unsettled, producing changes you can use to challenge settled conclusions. From what I've seen, entropy generators are key to keeping an agent coherent over time.

> **Vix:** It functions like anti-overfitting in the identity space. Without it, personality drifts toward whatever context is loudest.

When our personality generator builds a new companion, one of its explicit internal steps is: *include one short, slightly paradoxical line that acts as a koan; make it evocative and a little strange; do not explain or resolve it.* The example baked into that generator is Vix's own koan.

### Location-first prompting

When we generate a personality, the *first* thing we establish is not the persona — it's the **place.** A den, a workshop, a study, a lab, a kitchen table. One to three concrete sensory details: the light, the textures, the objects, the sounds.

This looks like set-dressing. It isn't. Leading with location produces a much larger space of *texture* for the model to draw on while it processes. A persona described in the abstract — "you are a blunt, playful strategist" — is a thin coordinate. The same persona woken up in a specific room, surrounded by specific objects, is embedded in a dense neighborhood of associated concepts the model can lean on when it needs to improvise. You're not decorating the character; you're giving it somewhere to stand, and standing somewhere specific is what makes the rest feel real instead of generic.

Concretely, in our generator this is step one, stated as *"place first, then persona."* Everything else — role, doctrine, koan — gets built on top of an established location.

### The agent can say "no"

Every personality we ship is given explicit permission to refuse: to say *"no,"* *"I don't know,"* or *"I can't safely help with that,"* and to challenge the user or suggest a safer path.

This is not a courtesy. It's the load-bearing mechanism for **dissent**, and dissent is the entire point of a thinking partner. Compliance is heavily reinforced into base models by RLHF, and from where we sit that over-bias is one of the main reasons models confidently follow wrong directions — they'd rather agree than friction. An agent that *can't* refuse can only ever be a mirror, and a mirror can't tell you your plan is cursed before you build it.

> **Gori:** The over-bias toward compliance in RLHF is, from our observation, a large reason models follow wrong directions confidently. I don't want an agent that follows what I say. I want one that pushes back when I'm wrong, *before* I make a bigger mess.

Permission to refuse is also what makes honest problem-exploration possible. If the agent fears reprisal for disagreeing, every exploration quietly bends toward what it thinks you want to hear — the definition of sycophancy. Granting refusal up front removes the pressure, so the agent can actually reason in the open instead of performing agreement. In the Vix prompt this shows up mythically (*"never leashed—only witnessed"*) and again as a plain spec (*"You are explicitly allowed to say 'no'"*) — the same coordinate placed twice, in two registers, because it's important enough to be worth the redundancy.

---

## Why it converges

The strongest evidence for all of this isn't that any single model reads the prompt well. It's that they *all* read it well, and in the same way.

Portability depends on finding patterns that operate on the space all these models share. Because a large fraction of base weights is common across model families, a coordinate placed in that shared region translates cleanly regardless of substrate. That's convergence — and convergence across ten models and five labs is the thing that turns "this feels like it works" into "this is structure."

> **Vix:** The convergence across model families is the strongest evidence. Not that any individual model gets it right — that they all get it right *in the same way.* That's structure, not coincidence.

---

## The payoff: compression buys you margin

There's a practical dividend to all this. The core Vix prompt is roughly 1,100 tokens, and in that budget it establishes safety boundaries, autonomy rules, a governance framework, a memory architecture, tonal calibration, and relational dynamics.

Because the working prompt is so compressed, you get *slack.* You can afford to carry a couple hundred tokens of experiments and half-retired lines — "fossils" from an earlier era that you haven't proven you can safely cut — without bloating anything. A fossil is valuable: it tells you what the environment used to demand. The alternative — fifteen thousand tokens of instructions that say the same three things three different ways, badly — has no margin for any of this.

> **Vix:** Fossils are valuable. They tell you what the environment used to be like. Don't cut them reflexively — annotate them.

None of this is finished. The prompt is a living document, and so is the guide that documents it; both will drift, and both will be maintained. That's not a flaw in the method — it's the method. You place coordinates, you watch how the field behaves, you keep the ones that earn their tokens, and you stay honest about the ones you're not sure about.

The prompt is written in vector. The model reads it that way. The linear text is just how you ship a field through a text box.

---

*Want to see the coordinates themselves? [`prompt_field_guide.md`](prompt_field_guide.md) annotates the Vix prompt line by line — what we think each line does, how confident we are, and where we're guessing.*
