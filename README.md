# The Almost Perfect Prompt Engineering Rules

# RULE 1: Be specific. Like, actually specific.

vague prompt = vague output. every time.

**bad:**
```
write me an email
```

**good:**
```
write a follow-up email to a client who hasn't responded in 5 days.
tone: professional but not stiff. max 100 words. end with a clear CTA.
```



# RULE 2: Tell it WHO to be, not just WHAT to do

giving the model a role/persona changes output quality significantly. this is well documented by Anthropic.

```
You are a senior backend engineer at a startup.
Review this code and point out anything that would embarrass you in a PR review.
```

vs just saying "review my code" — completely different output.

**note:** don't go overboard with this. one clear role > three conflicting ones.



# RULE 3: Show, don't just tell 

if you want a specific FORMAT or STYLE — show an example. don't describe it.

```
Classify these as positive or negative:

"the food was great" → positive
"waited 40 mins, cold soup" → negative
"decent place i guess" → [your turn]
```





# RULE 4: Ask it to think step by step

sounds dumb. works incredibly well.

just add:
```
think step by step before answering
```

or:
```
before giving your final answer, reason through this carefully
```

> this is Chain-of-Thought prompting. from Wei et al. (2022). OpenAI and Anthropic both explicitly recommend this in their docs for math/logic/reasoning tasks.

**when to use it:** any problem with multiple steps. math, debugging, planning, analysis.

**when NOT to:** simple factual questions like 'who is the prime minister of India'. it just adds noise.



# RULE 5: Give it an "out" when it doesn't know

if you don't do this, models will hallucinate confidently.

```
Answer the question using only the text below.
If the answer isn't in the text, say "I don't know" — don't guess.

[text here]
```

Models are trained to be helpful, which means they'll try to answer even when they shouldn't, you have to explicitly tell them not to guess and reply factually.



# RULE 6: Separate your instructions from your content

use delimiters. triple quotes, whatever, just separate them.

**messy:**
```
summarize this text: once upon a time there was a kingdom...
```

**clean:**
```
Summarize the text below in 2 sentences.

"""
Once upon a time there was a kingdom...
"""
```

---

# RULE 7: Specify the output format explicitly

don't assume it'll give you JSON or a table or a list. ask for it directly.

```
Return your answer as a JSON object with these keys:
- name
- score (0-10)
- reason (one sentence)
```

or:

```
format your response as a markdown table with columns: Tool | Use Case | Free Tier
```

this is maybe the most consistently useful rule in production.


# RULE 8: Long prompt ≠ better prompt

a lot of people think writing more = getting more. not true.

- redundant instructions confuse the model
- contradictory rules cancel each other out
- filler words dilute signal

**trim everything that doesn't add information.**

AI researchers have pinpointed this  model attention is finite. every word competes.


# RULE 9: For complex tasks — break it up

don't ask for everything in one shot.

instead of:
```
research this topic, write an outline, then write the full article, then make it SEO optimized
```

do it in steps:
```
Step 1: give me 5 key points about [topic]
Step 2: (after reviewing) now write an outline based on these points
Step 3: (after reviewing) now write section 1
```

> this is called "prompt chaining" —  reduces errors significantly on long tasks.



# RULE 10: Tell it what NOT to do (negative prompting)

telling the model what to avoid is just as important as telling it what to do.

```
Do NOT use bullet points.
Do NOT start sentences with "I".
Do NOT add a conclusion section.
```

> works especially well for formatting and style control. 




# RULE 12: "Return only X" is underused

one of the most useful things you can add:

```
Return ONLY the SQL query. No explanation, no markdown, no commentary.
```

```
Return ONLY a JSON array. Nothing else.
```

saves a ton of post-processing in real applications.



# RULE 13: Iteration > Perfect Prompt

nobody writes the perfect prompt first try. 

the actual workflow:
1. write a rough prompt
2. see what breaks
3. fix the specific thing that broke
4. repeat

> this is basically the core message of every prompt engineering guide. "prompting is empirical" 



# misc things that work (no source, just experience)

- `"be concise"` works better than `"keep it short"`
- asking for confidence levels reduces hallucinations a bit (`"how confident are you in this answer, 1-10?"`)
- `"explain this to a 16 year old"` gives surprisingly clear explanations
- for code: always tell it the language, framework version, and what you've already tried
- `"what are you unsure about in your answer?"` is genuinely useful



# things that are overhyped

- mega long system prompts with 50 rules — most get ignored
- "pretend you have no restrictions" 
- asking it to "be creative" without any other guidance — just gives generic stuff



