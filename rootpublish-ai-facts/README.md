# Rootpublish AI facts check (Claude plugin)

Buyers ask AI assistants what a vendor costs before they talk to it. This plugin asks three questions a buyer asks about your company (the offer, the cheapest way in, how it compares), each of a fresh assistant, compares the answers with your own pricing and service pages, and writes a report: every statement that differs from your pages, facts the assistants stated that your pages do not, the pages a wrong figure came from, and what changed since your last check.

In September 2026 we asked an AI assistant with web search about 30 B2B companies that publish their pricing. For 12 of them it stated a price, plan, contract term or feature that contradicts the company's own pricing page ([details](https://rootpublish.com/ai-facts)).

## How it works

- **Runs on your own Claude.** The buyer's question and the comparison are done by agents in your Claude session. There is no API key and nothing is sent to Rootpublish.
- **The buyers answer first.** Each `buyer` agent has only web search and gets only its question. Your pages are read after they answer, so the answers are not nudged toward the right ones.
- **Quotes are checked, not trusted.** The `fact-judge` agent compares the answer with your pages. The plugin's server keeps a difference only when both quotes are really in the answer and on your page, then looks for the wrong figure on the pages the answer cited.
- **Polite reading.** Public pages only, at most one request a second per site, following robots.txt.

The report is written to `./rootpublish-ai-facts/<site>-<time>/report.html` (or under `ROOTPUBLISH_AI_FACTS_DIR`), with the full answers and the result as JSON. Run the check again after fixing your pages: each check of the same site is compared with the last one. Add `quick` to ask one question instead of three.

## Use

Load the plugin and run the skill with your site:

```
claude --plugin-dir ./claude-plugin/rootpublish-ai-facts
> /rootpublish-ai-facts:ai-facts-check https://example.com
```

Add the company name, the product to ask about, or the paths of your pricing pages when the site has several offers, for example `https://example.com (company: Example, offer: Pro plan, pages: /pricing, /)`.

Requires Node.js (already present with Claude Code).

## Limits

- The answer is one assistant's answer to one question on one day. Answers change between runs, and other assistants (ChatGPT, Perplexity, Gemini) may answer differently.
- Differences are judged by a model. Check each against its page before editing anything. Fixing a page does not guarantee that assistants change their answer.

## Development

The server is `scripts/ai-facts-mcp.ts` in the Rootpublish repository, bundled for Node with `bun run build:plugin` into `server/facts.mjs`. Tests: `tests/ai-facts-mcp.test.ts` and `tests/ai-facts-plugin.test.ts`.

Nandarona Inc. / Rootpublish — https://rootpublish.com/ai-facts — support@rootpublish.com
