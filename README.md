# Rootpublish plugins for Claude

## rootpublish-ai-facts

Buyers ask AI assistants what a vendor costs before they talk to it. This plugin asks three questions a buyer asks about your company, each of a fresh assistant with web search, and compares the answers with your own pricing and service pages. The report shows every statement that differs from your pages, the facts the assistants stated that your pages do not, the pages a wrong figure came from, and what changed since your last check.

In September 2026 we asked an AI assistant with web search about 30 B2B companies that publish their pricing. For 12 of them it stated a price, plan, contract term or feature that contradicts the company's own pricing page ([details](https://rootpublish.com/ai-facts)).

It runs on your own Claude: no API key, and nothing is sent to Rootpublish.

### Install

In Claude Code:

```
/plugin marketplace add nandarona-inc/rootpublish-ai-facts
/plugin install rootpublish-ai-facts@rootpublish
```

Then run the check with your site:

```
/rootpublish-ai-facts:ai-facts-check https://example.com
```

Requires Node.js, which comes with Claude Code. See [the plugin's README](rootpublish-ai-facts/README.md) for how it works and its limits.

MIT License. Nandarona Inc. / Rootpublish — https://rootpublish.com/ai-facts — support@rootpublish.com
