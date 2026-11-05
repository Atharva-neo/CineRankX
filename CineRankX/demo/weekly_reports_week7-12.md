# CineRankX: Weekly Reports, Weeks 7 to 12

Format follows your Week 1/Week 2 sheets (Team / Outcome / KPI signal / Risk / Mentor action, then the text boxes).
**Project:** CineRankX | **Mentor:** Prof. Sahil Nadaf | **Week ending:** fill in each week's date.

Assumptions (change if your weeks 3 to 6 differ): Team A = data and IR engine, Team B = ML strategy selector, Team C = ranking engine and UI.
Weeks 3 to 6 are assumed to have covered the TF-IDF matrix, strategy classifier, and ranking engine. Numbers below come from the project's real logs and reports.

---

## Week 7: Evaluation suite and KPI definition

| Team | Outcome this week | Common KPI signal | Risk / dependency | Mentor action / owner |
|---|---|---|---|---|
| A | Search metrics built: Precision@10, Recall@10, MRR, NDCG, latency | 20 test queries run | Relevance labels are proxy-based | Approved, continue |
| B | Classifier metrics: accuracy, precision, recall, F1, ROC-AUC, confusion matrix | SMOTE tested, shipped off | Rare content-based class | Approved |
| C | Recommendation metrics: P/R/F1@K, MAP, coverage, diversity, novelty, cold-start | 23 KPIs in eval_config.yaml | Needs final ranker output | Approved, review report |

**Cross-team KPI synthesis:** All three workstreams now report against one shared KPI list (23 metrics, exact names from eval_config.yaml), so results are comparable. The common pattern is that every metric is computed from real data and nothing is hand-entered.

**Challenges / Risks:**
- Amber: MovieLens 20M only has users with 20+ ratings, so no natural cold-start users exist. Fixed by simulating a cold-start cohort (real users cut to 5 ratings).
- Green: SMOTE for class imbalance was investigated and kept disabled because it did not improve results.
- Green: popularity now ranked by observed rating counts instead of TMDB popularity.

**Help needed:** None this week.

**Integration & Collaboration:** Evaluation module reads the IR engine, classifier and ranker through their existing interfaces. 71 tests passing.

**Next week commitments:**
| Owner | Deliverable | Due |
|---|---|---|
| Team A/B/C | Final KPI report script and README update | End of week 7 |
| Team C | Web UI to view recommendations | End of week 8 |

**Mentor response:** KPI list is complete and well defined. Start the user interface next and keep the numbers honest.

---

## Week 8: Recommendation results dashboard

| Team | Outcome this week | Common KPI signal | Risk / dependency | Mentor action / owner |
|---|---|---|---|---|
| A | Catalog and index loaded by the app | Search index loads | None | Approved |
| B | Strategy prediction wired into the app | Strategy shown per user | Model must be saved | Approved |
| C | Streamlit dashboard: run engine, upload ranked CSV, score breakdown, CSV export | Output KPIs on each list | Mislabelling list summaries as accuracy | Approved, merged PR |

**Cross-team KPI synthesis:** The app shows list-level numbers (average score, relevance, novelty) and states they are not held-out precision or recall. This avoids reporting misleading accuracy.

**Challenges / Risks:**
- Amber: ranked CSV from users could be malformed. Added validation (required columns, finite numbers, unique ranks and movie IDs).
- Green: merge of the teammate's feature branch (recommendation-results-ui) into main.

**Help needed:** None this week.

**Integration & Collaboration:** Pull request from teammate merged into main; UI reuses the ranker and classifier directly, no duplicated logic. Unit tests added for results validation.

**Next week commitments:**
| Owner | Deliverable | Due |
|---|---|---|
| Team C | UI redesign to the supplied reference design | End of week 9 |

**Mentor response:** Working end-to-end demo achieved. Improve usability and presentation.

---

## Week 9: UI redesign and usability

| Team | Outcome this week | Common KPI signal | Risk / dependency | Mentor action / owner |
|---|---|---|---|---|
| A | Overview page: catalog, funnel, objective weights | Real counts shown | Needs processed data | Approved |
| B | Engines page with live engine status and measured metrics | Metrics from KPI report | Report must exist | Approved |
| C | Runner redesign: user ID check, six weight sliders with normalization and reset, ranked cards with reasons, CSV export | Run takes about 0.2 to 0.5 s | Invalid input handling | Approved |

**Cross-team KPI synthesis:** All screens now read real data (catalog, ratings, KPI report); no mock values. Page links are shareable (view in the URL).

**Challenges / Risks:**
- Amber: Streamlit styling limits. Solved with a theme file and custom CSS.
- Green: stale module cache after code edits. Fixed by restarting the server.
- Green: icon font broke after a CSS change; corrected.

**Help needed:** None this week.

**Integration & Collaboration:** Followed the supplied UI reference (design tokens, Feather icons only). Generate button disabled until the user ID and weights are valid.

**Next week commitments:**
| Owner | Deliverable | Due |
|---|---|---|
| Team A | Load full MovieLens 20M + TMDB and rebuild all models | End of week 10 |

**Mentor response:** Interface is much improved. Now validate on the full dataset.

---

## Week 10: Full dataset, rebuild and local database

| Team | Outcome this week | Common KPI signal | Risk / dependency | Mentor action / owner |
|---|---|---|---|---|
| A | Full join of MovieLens 20M + TMDB 2023: 26,743 movies matched (98%) | Join rate about 98% | Large files | Approved |
| B | Classifier retrained on full data | Accuracy 0.52 | Close to majority baseline | Review features |
| C | SQLite store: 20,000,263 ratings, 138,493 users, indexed | Queries fast by user | Disk size about 1.4 GB | Approved |

**Cross-team KPI synthesis:** Full-data KPI report (300 users, 400k ratings): Precision@10 (search) 0.41, MRR 0.44, MAP 0.11, Diversity 0.83. The common weakness is the strategy classifier, which is about as accurate as always guessing the most common strategy.

**Challenges / Risks:**
- Amber: classifier accuracy 0.52 vs 0.52 baseline; the content-based class is rare. Plan: better profile features and more evaluation users.
- Green: online download was too slow, so the provided archive files were used.
- Green: 20M-row file loaded in chunks to avoid memory problems.

**Help needed:** None this week.

**Integration & Collaboration:** Ingestion pipeline unchanged except extra TMDB fields (poster, language, release date) kept for the UI. All 230 tests pass after rebuild.

**Next week commitments:**
| Owner | Deliverable | Due |
|---|---|---|
| Team C | User profile (User 360) page | End of week 11 |

**Mentor response:** Good scale-up. Be honest about the classifier result in the report.

---

## Week 11: User 360 profile and recommendation history

| Team | Outcome this week | Common KPI signal | Risk / dependency | Mentor action / owner |
|---|---|---|---|---|
| A | User history: search, genre filter, rating range, sort, pagination done in the database | 6 new tests pass | None | Approved |
| B | Preferences: favorite genres, eras, languages, rating distribution | Real data only | No demographics in MovieLens | Approved |
| C | Past runs saved (time, strategy, weights, status, results) and current recommendations per user | Runs logged to database | No login system in project | Approved |

**Cross-team KPI synthesis:** Every user page is built from stored ratings; unavailable fields (age, location) are clearly shown as unavailable instead of invented.

**Challenges / Risks:**
- Amber: live recommendations only cover users in the 400k-rating sample; profiles cover all 138,493 users. Stated clearly in the UI.
- Green: project has no authentication layer, so none was added (single local operator tool).

**Help needed:** None this week.

**Integration & Collaboration:** Runner now logs each run to the same database; profile page reuses the runner and results display. 230 tests passing.

**Next week commitments:**
| Owner | Deliverable | Due |
|---|---|---|
| All | Final testing, commit, deployment, demo script | End of week 12 |

**Mentor response:** Strong feature. Prepare the final demo and report.

---

## Week 12: Final testing, release and demo preparation

| Team | Outcome this week | Common KPI signal | Risk / dependency | Mentor action / owner |
|---|---|---|---|---|
| A | End-to-end test of all pages and deep links | All 5 pages load, no errors | None | Approved |
| B | Final KPI report saved as JSON and shown in the KPI page | 23 KPIs displayed | Report must be regenerated after data changes | Approved |
| C | Code committed; demo script and sample ranked CSV prepared | 230 tests pass | Cloud deployment blocked | Help needed |

**Cross-team KPI synthesis:** The system runs end to end on the full dataset (20M ratings): search 0.41 Precision@10, recommendation MAP 0.11, diversity 0.83, about 32 ms per operation. The shared limitation is low catalog coverage (about 2.7%) and the weak strategy classifier; both listed as future work.

**Challenges / Risks:**
- Red: public cloud deployment not possible yet. We do not have admin access to the GitHub repository, and the 1.4 GB data is not in git. Plan: fork the repo and bundle a small sample dataset, or demo locally.
- Amber: classifier accuracy near baseline (0.52); will be reported honestly.

**Help needed (escalations / HOD):** Repository admin or fork access for cloud deployment, or approval to demo locally.

**Integration & Collaboration:** Work committed to main (Redesign UI and User 360 profile). Demo flow rehearsed: Overview, Engines, Run Engine, User Profile, KPI Benchmarks.

**Next week commitments:**
| Owner | Deliverable | Due |
|---|---|---|
| All | Final report, presentation, viva preparation | End of week 13 |
| Team C | Deployment (fork + sample data) if approved | End of week 13 |

**Mentor response:** Project is complete and demo-ready. Finalize report and presentation; local demo acceptable if deployment is not possible.
