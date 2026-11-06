# Wekaro content engine: plan

Written 2026-11-03 for Sir. Plan only: nothing gets built until you say go.

## 1. The engine in one picture

Three things feed in, one process turns them into posts, and you approve everything before it goes out.

| Feeds in | What it is | How often |
|---|---|---|
| **AI news** | Official announcements and trusted reporting from the week | Weekly |
| **Your drafts** | Your rough articles and notes (prompt engineering first) | Whenever you send them |
| **Inspiration** | What competitors post on Instagram and X, and what does well | Weekly |

Every piece goes through the same four steps: **collect, draft, you approve, publish**. One approved piece is written once and then cut into several outputs: a page or news item on the site, plus the social posts that point to it. That way social always sends people to something on Wekaro.

## 2. Where the AI news comes from

Rule: every story links to a **primary source** (the company's own post, the paper, the release notes). Press is used to find stories and add context, never as the only source. No rumours, no leaks, no "sources say".

**Primary sources (checked every week)**
- Lab blogs and changelogs: Anthropic (news + API release notes), OpenAI, Google DeepMind / Gemini, Meta AI, Mistral, xAI, DeepSeek, Qwen.
- Developer tools: GitHub release notes for Claude Code, Cursor, LangChain, Hugging Face libraries; the Hugging Face blog and its daily trending papers.
- Research: arXiv papers that are trending on Hugging Face, university newsrooms.

**Context and discovery (to find what matters, then go to the primary source)**
- Press: Reuters technology, The Verge AI, TechCrunch AI, MIT Technology Review.
- Africa and Nigeria angle: TechCabal, Techpoint Africa, Rest of World. This is a gap most AI accounts ignore and fits our audience.
- Newsletters and writers: Import AI, The Batch (DeepLearning.AI), Simon Willison's blog, Latent Space.

**How it runs (semi-automatic, as decided on 17 October)**
1. Every Thursday a scheduled Claude run searches the sources above and shortlists about ten stories.
2. It picks five and drafts each in the shape the site already uses: headline, what happened (with source and date), and "What it means for you" in one line.
3. You get the five on a review card: approve, cut or edit each.
4. Approved stories go to the site's news section and become the Friday "This week in AI" carousel and news cards.

**One build change this needs later:** today the news lives inside the site's code (`hub-content.ts`), so each week's update needs a code change and redeploy. Moving news into the database lets approved stories go live without that. Small job, worth doing once the weekly rhythm is proven.

## 3. Where course and article content comes from

In order of importance:
1. **Your own drafts and expertise.** The most valuable source, because it's original and nobody else has it. Prompt engineering first.
2. **Curated free material put in order.** How the Architect course works today: we pick the best free resources and add structure, quizzes and progress. We always say it's curated.
3. **Explainers built from official docs.** Claude researches a topic from the vendor's own documentation and writes a plain-English lesson, checked against the source.
4. **Reader questions.** Questions from newsletter subscribers and comments become articles and practice questions.

## 4. Intake: turning your rough drafts into a course or article

You send the draft any way that's easy: paste it in the thread, upload a file or photo, share a Google Doc, or send a voice-note transcript. Messy is fine.

1. **Triage.** I read it and say what it should become: a single article, a short series, or a course module. Rough guide: one idea is an article (1,000 to 1,800 words); a skill you build up step by step is a course (lessons, each with a short quiz).
2. **Outline for your approval.** Title, who it's for, the sections, and what I'd add or cut. You approve or redirect before I write anything long.
3. **Check it.** Every factual claim is checked against a current source. Every example prompt is re-run against current models so it still works, and anything out of date gets flagged to you, not quietly changed.
4. **Rewrite in Wekaro voice.** Your ideas and examples stay; I fix structure, clarity and length. Voice: a sharp friend who's done the course, short sentences, numbers first, no hype words, no exclamation marks.
5. **Add the extras.** A short quiz (three to five questions, answers shuffled), a "try this" exercise, and the "Independent. Not affiliated with Anthropic." line if Claude is named.
6. **Cut the social pack.** From the same piece: one "Week by week" carousel, two practice-question posts, one big-number post if there's a good number, and four or five X posts.
7. **Your final sign-off**, then it's published. Your name goes on it as the author.

## 5. Competitor and inspiration process

**What we take:** topics, formats, hooks and timing. What people are asking about, which post shapes get saved and shared, and gaps nobody is covering well.

**What we never take:** their words, images, slide designs, or a post rebuilt slide by slide.

**How it works**
1. **Watch list.** You name the accounts you follow (Instagram and X). I add a few, and we keep 10 to 15 total, with a mix of big global accounts and accounts the same size as us.
2. **Weekly swipe file.** Instagram and X block the reading tools I have here, so you drop links or screenshots of posts that caught your eye into the thread during the week. I can read screenshots. For each one I note the topic, format, hook, and why it worked (saves, comments, shares where visible).
3. **Monthly pattern note.** What topics and formats are winning, and what's missing that Wekaro can own.
4. **Rewrite rule.** A post inspired by others is built from at least two or three sources plus our own angle, with facts re-checked at the primary source. If one creator's specific idea, data or framework is the heart of our post, we credit them ("via @handle").

**Guardrails (copyright and platform rules)**
- Ideas, facts and formats aren't protected; the wording, images and design are. So we take the first and never the second.
- No screenshots of other people's posts inside ours. No reposting their video or images.
- Short quotes only, always credited.
- No automated scraping of Instagram or X: it's against both platforms' terms, and an account like ours can be banned for it. Manual collection only.
- If a post would make someone say "that's just their post with our colours", we don't publish it.

## 6. How the engine decides what we post

The seven series from the brand identity boards are the menu. Each week mixes them so the profile grid has rhythm and every post points to something on the site.

**Starting weekly plan (Instagram)**

| Day | Series | Fed by |
|---|---|---|
| Mon | Week by week (lesson carousel) | Your drafts, courses |
| Tue | Practice question (2 slides) | Course quizzes |
| Wed | Big number or Free this week | Courses, the free Playbook |
| Thu | Practice question or reader question | Quizzes, comments |
| Fri | This week in AI (5-story carousel) | News run |
| Sat | New on the shelf or From the founder | Launches, your notes |
| Sun | Rest, or a story repost | |

X gets one to three short posts a day cut from the same material: a news line, a prompt tip, a practice question.

**How each topic is picked.** A topic gets in if it scores on most of these: it's timely or people are asking about it (from the swipe file and comments); it links to something on Wekaro; it's something we can say with authority; and it isn't something we posted in the last month.

**Feedback loop.** Every Monday I review last week's numbers (saves, shares, profile visits, link clicks, sign-ups) and adjust the plan. Saves and shares matter more than likes.

## 7. Where the work lives

To start, keep it light: drafts and plans in the project folder, approvals on review cards in this thread. Once the rhythm works, a simple content calendar (a Notion database, or a tracker page) shows each piece, its status and its publish day. You already have Notion connected, so that's the easy option when we get there.

## 8. Proposed first step

**A one-week pilot, using your first article.** You send one or two rough prompt-engineering articles and the competitor accounts you follow. I run one article through the full intake (outline, your approval, polished article, social pack) and draft that week's five news stories for approval at the same time. You see the whole engine working once, end to end, before we automate any of it.

What it needs from you:
1. Your rough prompt-engineering articles (any format).
2. The competitor accounts you follow on Instagram and X.
3. Your go-ahead on this plan, or changes.
