# Content engine: where we stopped

Last updated 2026-11-06. Read this first when picking the work up in a new workspace. The full plan is in [plan.md](./plan.md).

## Where things stand

- **Site:** the Wekaro rebrand and hub homepage are merged (PR #1) and live at https://ultimatestudy.xyz. Newsletter sign-ups save to the database.
- **Content engine:** planning stage only. The plan was written on 2026-11-03 and nothing has been built yet. Rule for this work: propose a step, wait for the owner's go-ahead, then build.

## What the plan covers (short version)

1. **AI news:** weekly run (Thursday). Primary sources only (lab blogs, release notes, papers); press and newsletters are just for finding stories. Claude drafts five stories in the site's existing shape (headline, what happened with source and date, "What it means for you"), the owner approves, and they feed the site news and the Friday "This week in AI" carousel.
2. **Course and article content:** the owner's own drafts first, then curated free material, explainers built from official docs, and reader questions.
3. **Intake for the owner's rough drafts:** triage (article, series or course), outline for approval, fact check and re-test every example prompt, rewrite in Wekaro voice, add a short quiz, cut a social pack, final sign-off, published under the owner's name.
4. **Competitor inspiration:** a 10 to 15 account watch list on Instagram and X. The owner drops links or screenshots in during the week (Claude's tools cannot read Instagram or X, and scraping breaks both platforms' terms). Take topics, formats and hooks; never wording, images or slide designs. Credit "via @handle" when one creator's idea is the core of a post.
5. **What we post:** a weekly plan built on the seven series from the brand identity boards, reviewed every Monday by saves, shares and link clicks.

## Next step (proposed, waiting on the owner)

A one-week pilot: take one of the owner's prompt-engineering articles through the full intake and draft that week's five news stories alongside it.

**Waiting on the owner for:**
- [ ] Go-ahead on the plan (or changes)
- [ ] Rough prompt-engineering articles (any format)
- [ ] The competitor accounts followed on Instagram and X

## Build work the plan will need later (not started)

- Move the weekly news out of `src/lib/data/hub-content.ts` and into the database, so approved stories go live without a code change and redeploy.
- Optionally a content calendar (Notion database or a tracker page) once the weekly rhythm is working.

## Other open items outside the content engine

- Set `NEXT_PUBLIC_APP_URL=https://ultimatestudy.xyz` in Vercel; the share image still points at the old `ultimate-study-nu.vercel.app` address.
- Domains: `wekaro.ai` and `getwekaro.com` are planned but not registered. Instagram handle and trademark search still to check.
- Paystack is not live yet. Before it goes live, the webhook must check the amount and currency paid (it doesn't today).

## Related files

- Brand brief and voice rules: kept in the project's shared files (`brand/brand-brief.md`), not in this repo. Voice in one line: a sharp friend who has done the course; short sentences, numbers first, no hype words, no exclamation marks.
- Social post series and caption rules: `brand/claude-design-identity-prompt.md` in the project's shared files, and the identity boards artifact.
