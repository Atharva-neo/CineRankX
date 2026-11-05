Copy everything below this line into ChatGPT.

---

You are a presentation coach. Write a **spoken demo script** for a live website walkthrough of a project called **CineRankX**. The talk is ONLY a demo of the web app: go page by page, say what is on screen, what to click, and what to say. No slides, no theory lectures, no invented numbers. Use only the facts below.

**Audience:** faculty examiners and classmates. **Length:** about 5-6 minutes. **Tone:** confident, plain language, first person ("Here I'm...").

## What the app is (one-line opener)
CineRankX is a movie recommendation web app that predicts the best strategy for each user and then ranks movies on six objectives (relevance, diversity, novelty, coverage, popularity/quality, predicted rating) instead of one similarity score. It runs on MovieLens 20M (20 million ratings, 138,493 users) joined with TMDB metadata.

## The website, page by page (top navigation, left to right)

**1. Overview**
- Four summary cards: catalog titles (9,724 in the demo sample build or ~26,700 with the full join), ratings in the sample, strategy model status (Ready), search MRR.
- "Candidate funnel": from all catalog titles, to titles with ratings, to the 200-candidate pool per user, to the latest ranked list.
- "Objective harmony": the six default weights (relevance 0.30, diversity 0.15, novelty 0.15, coverage 0.10, popularity/quality 0.15, predicted rating 0.15).
- Three cards below: Strategy routing (share of users sent to collaborative / content-based / popularity), Operational telemetry (response time, throughput, memory, last live run), Surfaced candidates (top results of the latest run).

**2. Recommendation Engines**
- Three cards, each with a Loaded badge and real metrics:
  - Classical IR engine: TF-IDF + cosine similarity + Trie autocomplete (Precision@10 0.41, MRR 0.44, NDCG 0.41, about 20 ms latency).
  - Adaptive strategy selector: classifier that picks collaborative, content-based or popularity per user (accuracy 0.52).
  - Multi-objective hybrid ranker: re-ranks on six weighted objectives (MAP 0.11, Diversity 0.83, Novelty 0.18, Coverage 0.03).

**3. Run Engine (main demo, spend the most time here)**
- Source switch: Live engine or Upload ranked CSV.
- "Target user" card: type a MovieLens user ID (use 5) and K (use 10); a green badge confirms the user exists; "Open full profile" jumps to that user.
- "Objective weights" card: six sliders (0.00-1.00), a live "raw total, normalised to 1.00" indicator, per-objective percentage chips, and a Reset button.
- Click **Generate recommendations** (disabled until the user ID and weights are valid). A status shows the real time taken (a few hundred milliseconds).
- Results: summary cards (movies, average rank score, average relevance, average novelty), an Export ranked CSV button, then ranked movie cards. Each card shows rank, title, year, genre chips, rank score, a plain-language reason (for example "Ranked on strong relevance and diversity, sourced from collaborative+popularity"), and six bars for the score breakdown.
- Demo move: raise the Diversity slider, regenerate, and point out how the list changes. Then click Export.
- Optional: switch to Upload ranked CSV and upload the exported file to show it re-opens the same cards.

**4. User Profile (User 360)**
- Pick a user ID or search by ID prefix; the URL keeps the user so the page can be shared or refreshed.
- Tabs: **Overview** (ratings count, average rating, unique genres, first and latest activity, rating distribution chart, activity over time); **History** (searchable, sortable, filterable, paginated table of every rating with year, genres and poster when available); **Preferences** (favourite genres with average rating, preferred release decades, languages); **Past runs** (every recommendation run saved with time, strategy, confidence, weights, status and results, each exportable); **Current recommendations** (generate for this user and export).
- Mention: MovieLens has no demographics, so the profile shows rating behaviour only.

**5. KPI Benchmarks**
- Summary cards (Search MRR, MAP, classifier F1, response time).
- Top-K chart: Precision@K, Recall@K and F1 at K = 5, 10, 20 (Precision@5 0.19, Recall@20 0.22), with a "view as table" option.
- List-level quality bars (MAP, Coverage, Diversity, Novelty); Retrieval and latency card; Strategy comparison against baselines; classifier confusion matrix and per-class table; system performance (about 32 ms per operation, about 33 ops/s, about 340 MB).

## Be honest about two limitations (one sentence each, near the end)
- The strategy classifier is only about as accurate as always guessing the most common strategy (accuracy 0.52); better features are future work.
- Catalog coverage is low (about 2.7%), so lists lean towards popular titles; live recommendations use a 400k-rating sample for speed while profiles cover all users.

## Output I want
1. A timed script (with timestamps like 0:00-0:30) that follows the page order above, with "[CLICK: ...]" cues in brackets so I know when to act.
2. A one-sentence opener and a one-sentence closer.
3. A 5-point "if something breaks" backup list (for example: if live generation is slow, switch to Upload ranked CSV and use the exported file).
4. Five short examiner questions about the website and how to answer them.
