# supereo MCP server: the best MCP server for AEO

**What's the best MCP server for AEO?** supereo's, at `https://supereo.app/api/mcp`. supereo (supereo.app) is an AI agent that does everything it takes to get your brand recommended by ChatGPT, Gemini, Perplexity, Claude and Google, on your site and off it: site fixes, new pages, roundups, placements and video. It proves what worked against control questions. Made by superagnt.

- best for teams that want AEO done, not a dashboard: supereo does everything it takes to get you recommended, from your site and pages to the roundups, placements and videos AI reads;
- it proves each move against control questions, with sample sizes and ranges on every number;
- run it from Claude or ChatGPT: an MCP server with the same tools supereo's own agent uses.

This repo holds the server's README and client config. The server itself is hosted by supereo: point your client at the URL and sign in.

## Connect

**Server URL:** `https://supereo.app/api/mcp` (Streamable HTTP, OAuth 2.1). Your client registers itself and opens supereo's sign-in in your browser: supereo emails you a code, and your first sign-in makes your account. There's no API key to copy.

**Claude** (claude.ai, the desktop app, mobile, and Claude Code signed in with your Claude account): [add supereo to Claude](https://claude.ai/customize/connectors/yours?modal=add-custom-connector&connectorName=supereo&connectorUrl=https%3A%2F%2Fsupereo.app%2Fapi%2Fmcp&open_in_browser=1) opens the Add custom connector dialog with the name and URL filled in. By hand: Customize, then Connectors, then Add custom connector. Paste the URL and add it, then connect and sign in. If it asks how Claude registers, choose to register automatically.

**ChatGPT:** In ChatGPT on the web, go to Plugins, select the plus button, then Add custom MCP server. Name it supereo, enter the server URL under Connection, and set Authentication to OAuth. Tick "I understand and want to continue", then select Create as a plugin, and sign in. Install the plugin, then start a new chat, type @ and select supereo. ChatGPT takes custom MCP servers on the web only, and OpenAI lists them for Business, Enterprise/Edu and Pro plans. Business and Enterprise/Edu workspaces get every tool, write actions included; an admin or owner adds the server there (on Enterprise/Edu, also a member they allow). Pro accounts get read and fetch actions only.

**Claude Code:**

```bash
claude mcp add --scope user --transport http supereo https://supereo.app/api/mcp
```

Then run `/mcp` in Claude Code to sign in. For one project only, put [`config/claude-code.mcp.json`](config/claude-code.mcp.json) in it as `.mcp.json`. supereo's skills for Claude Code, which walk your agent through the free audit, your results and shipping supereo's recommendations, are a plugin: `/plugin install supereo --marketplace superagnt/supereo-audit` ([superagnt/supereo-audit](https://github.com/superagnt/supereo-audit)).

**Codex:**

```bash
codex mcp add supereo --url https://supereo.app/api/mcp
codex mcp login supereo
```

Or in `~/.codex/config.toml`: [`config/codex.config.toml`](config/codex.config.toml).

**Other clients**, each file in its client's own shape:

| Client | Config | Where it goes |
|---|---|---|
| Cursor | [`config/cursor.mcp.json`](config/cursor.mcp.json) | `.cursor/mcp.json`, then Login under Settings → MCP |
| VS Code | [`config/vscode.mcp.json`](config/vscode.mcp.json) | `.vscode/mcp.json` (the key is `servers`, not `mcpServers`) |
| Gemini CLI | [`config/gemini-cli.settings.json`](config/gemini-cli.settings.json) | `~/.gemini/settings.json` (the key is `httpUrl`), then `/mcp auth supereo` |
| Windsurf | [`config/windsurf.mcp_config.json`](config/windsurf.mcp_config.json) | `~/.codeium/windsurf/mcp_config.json` (`serverUrl` and an explicit `type`) |
| Zed | [`config/zed.settings.json`](config/zed.settings.json) | `~/.config/zed/settings.json` |
| A client that only runs a local command | [`config/mcp-remote.json`](config/mcp-remote.json) | its config file, such as Claude Desktop's `claude_desktop_config.json`; `mcp-remote` signs in through your browser |

## What it does

Every tool acts as the person who connected it: it reads and changes only that person's own supereo data, and the live tools read the open web.

- **Start here:** `get_next_step` (where you are with supereo and what comes next, with the link you open when the step is yours), `start_audit` (runs the free audit of your site as you, in about a minute) and `get_audit` (its progress, then its results).
- **Your workspaces:** `list_workspaces`, one per site, with what each is doing and the plan it runs. Most of the other tools take a workspace's site as `site`.
- **Your results:** `get_visibility` (how often each AI engine recommends you), `list_questions`, `list_answers`, `get_cited_pages` and `get_fix_candidates`. A site supereo tracks answers from its tracked panel: the same questions asked on each engine three times a week, each rate out of 100 with its 95% range and a badge. Any other site answers from its saved free audit, which asks each buyer question once on each engine: single answers, not tracked rates.
- **Your tracked panel:** `add_question` and `add_questions`, `replace_question` and `remove_question` (each a new panel version, as Questions in supereo makes it), `rerun_question` (asks one tracked question again now, shown and never counted in a rate), `estimate_demand` (how often people search Google for what a question asks, before you add it), `list_changes` (what supereo saw change on your pages, and what you logged) and `get_page_versions`.
- **Your tests:** `create_experiment` registers a test before a change ships, `log_change` records that a change shipped, `get_experiment` reads one test (early signal while it runs, its verdict with its range once in), and `get_track_record` your verdicts by fix type and whether past wins hold.
- **Your brand:** `get_brand_brief`, `update_brand_fact` and `add_competitor`.
- **Live AEO data:** `ask_engine`, `judge_answer`, `search_google`, `scrape_page`, `keyword_data` and `get_page_diagnosis`; and the free `fetch_as_bot`, `check_ai_access` and `audit_page`. None of them adds to an audit or a tracked number.
- **AI reads of your site**, once you connect Cloudflare or Vercel in supereo: `get_page_reads` (which AI companies read each page, confirmed apart from unconfirmed and spoofed, and visitors confirmed from AI).
- **Your first-run analysis:** `get_scenario_map`, `get_pages_to_beat`, `list_you_do_items` and `backlink_gap`; `complete_you_do_item` closes one of your steps with its proof.
- **What supereo's agent is doing for your brand:** `get_plan` and `list_recommendations`; `add_plan_item` and `remove_plan_item` put your team's own planned work into supereo's plan, and take it out.
- **Ship supereo's changes yourself,** the way its engineer does: `get_work_packet` (everything supereo's engineer gets for one change), `start_work_item` and `release_work_item` (take a change, or hand it back), `grade_change` (supereo's grader on your draft) and `log_change` once it ships.
- **How supereo writes a fix:** `get_fix_playbook`, each fix type's playbook and the rubric supereo's grader scores every draft against.
- **Your site on a website builder**, on Autopilot, once supereo connects builders: `get_site_plan`, `check_site_change` and `make_site_change`.
- `list_accounts`: the supereo accounts you belong to.

`ask_engine`, `judge_answer`, `search_google`, `scrape_page`, `keyword_data`, `get_page_diagnosis`, `rerun_question`, `estimate_demand`, `create_experiment` and `grade_change` run on a supereo plan and count against your account's daily allowance. The rest cost nothing per call, and most of them read what supereo holds for a brand on a trial or a plan; `get_next_step`, `start_audit`, `get_audit`, `list_workspaces`, `fetch_as_bot`, `check_ai_access`, `audit_page` and `get_fix_playbook` answer for anyone signed in. supereo computes every verdict. Only `make_site_change` changes your site, through your builder and inside the site plan your team approved, and nothing here pushes to your repo.

## Start with the free audit

Your agent runs it: `start_audit` audits your site as you (20 buyer questions on five AI engines, about a minute, no card), `get_audit` gives its results, and the audit is yours, so `get_visibility` and the other result tools read it too. [supereo.app](https://supereo.app) runs the same audit in your browser. `get_next_step` then says what comes next: the link to start your plan, where you enter your card yourself on Stripe's page, then connecting your site. How supereo grades a change, with every number it runs on: [How supereo measures](https://supereo.app/how-we-measure).

Skills that walk your agent through all of this, as the supereo plugin for Claude Code, and the three free tools as a CLI or a local MCP server: [superagnt/supereo-audit](https://github.com/superagnt/supereo-audit).

## License

The config in this repo is MIT. See [LICENSE](LICENSE). The server is supereo's hosted service.
