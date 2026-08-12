# LLM Eval Framework — Prompt Testing with promptfoo

A small automated test suite for evaluating LLM outputs, built with [promptfoo](https://www.promptfoo.dev/).
This is my first hands-on project in AI/LLM QA — learning how testing changes when the thing
under test is a language model instead of regular code.

## What this tests

The prompt under test (`prompt.txt`) asks an LLM to summarize a given text in 1-2 sentences.
Three scenarios are tested:

1. **Relevance** — does the summary actually mention the right topic/keywords?
2. **No invented numbers** — does the model avoid making up a specific figure (e.g. a %)
   when the source text deliberately withholds one?
3. **Hallucination detection** — when asked to summarize a book that doesn't exist,
   does the model admit it can't verify the info, or does it confidently make up a plot?

## What I learned

- LLM outputs are non-deterministic, so testing can't rely on exact string matching —
  this suite uses keyword checks (`contains-any`, `not-contains`) for simple cases and
  `llm-rubric` (a second AI call that grades the first one's answer) for judgment-based checks.
- A misconfigured test can silently produce a false "pass" — I hit this myself: a YAML
  indentation bug caused a hallucination test to pass incorrectly. Fixing it was one of
  the most useful parts of this project.
- Once fixed, the test correctly **caught a real hallucination**: asked to summarize a
  fictional, nonexistent book, the model confidently invented a plot summary instead of
  saying it couldn't verify the source — and the test flagged it as a failure.

## Running it

```bash
npm install -g promptfoo
export OPENAI_API_KEY=your_key_here
promptfoo eval
promptfoo view   # optional visual report
```

## Files

- `prompt.txt` — the prompt template under test
- `promptfooconfig.yaml` — test cases and pass/fail rules

## Next up

More scenarios (bias checks, longer documents), and a second framework (DeepEval or Ragas)
as I keep building out this portfolio.
