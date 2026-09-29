---
name: ai-facts-check
description: Check what AI assistants tell buyers about a company (plans, prices, contract terms, what is included, who it is for) against the company's own pricing and service pages, and write a report with every difference, where the wrong facts came from, and what changed since the last check. Use when someone asks whether ChatGPT, Claude or other AI assistants get their pricing or company facts right, or gives a company website to check.
---

# AI facts check

Buyers ask AI assistants what a vendor costs before they talk to it. This check asks three questions a buyer asks, each of a fresh assistant, then compares the answers with the company's own pages. The order matters: the buyer agents must answer before anything from the company's pages is in the conversation, or their answers drift toward the right ones and the check shows nothing.

## Steps

Follow them in order and pass every text on word for word.

1. **Site.** Take the company's website from the request (for example `https://example.com`, or a language section such as `https://example.com/ja`). Take the company name, the product to ask about, and the paths of the pricing pages if the user gives them. Use `depth: "quick"` (one question instead of three) only when the user asks for a quick check. If there is no website, ask for it and stop.
2. **Start.** Call `start_check` with `site` (and `company`, `offer`, `pages`, `depth` if given). Do not open, fetch or search the company's pages yourself at any point in this check.
3. **Ask as buyers.** For each of the `questions`, launch one `rootpublish-ai-facts:buyer` agent, all in the same message so they run in parallel. Each agent's prompt is its question exactly, with nothing added: no instructions, no context, no mention of a check. Each agent starts fresh, so no answer sees another.
4. **Record.** Call `record_answers` with the `checkId` and the agents' full answers in question order, unedited, including their links.
5. **Judge.** For each of the `judgePrompts`, launch one `rootpublish-ai-facts:fact-judge` agent, all in the same message. Each agent's prompt is its judge prompt exactly.
6. **Finish.** Call `finish_check` with the `checkId` and the judges' replies in the same order, unedited.
7. **Report back** in the user's language:
   - the report path from `finish_check`;
   - how many statements differ from the company's pages, and up to three clear ones, each as "the assistant said … / your page says …";
   - if `sinceLastCheck` is present, how many differences are still there, new, or did not come up this time, and that one run without a difference does not prove it is fixed;
   - that answers change from one question to the next and differ between assistants, and that each difference should be checked against the page before anything is edited;
   - if `record_answers` listed pages that do not state prices or terms, offer to run the check again with the right pricing pages in `pages`.

Checks of the same site are kept side by side in `./rootpublish-ai-facts/`, and each new check is compared with the last one. Suggest running it again two to four weeks after the company fixes its pages.

## Do not

- Do not reword the questions, the answers, the judge prompts or the judgements, and do not add or remove differences yourself. `finish_check` keeps only the differences whose quotes are really in the answer and on the company's page.
- Do not promise that fixing a page changes what assistants say.
- Do not send the report anywhere; it stays on this computer.

## When a step fails

- `start_check` cannot read the site: ask for the exact address of the top page.
- `record_answers` reads no statements: ask which pages state prices and terms, and start again with `pages`.
- A judge's reply is not JSON: launch that judge once more with the same prompt. If it fails again, say so and stop.
