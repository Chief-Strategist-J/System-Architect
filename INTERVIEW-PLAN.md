# Senior/Staff System Architect — Interview Master Plan

Targets: **Google, Stripe, Neo4j, Anthropic**. Today: 2026-10-31. Plan: 16 weeks. Adjust once you have real interview dates.

Per-repo detail lives in each repo's `TODO.md`. This file is the cross-repo plan: which repo proves which skill, a week-by-week schedule, and company-specific preparation. Verify each company's current interview format on their careers page and with your recruiter — formats change, and what is below is the general shape, not insider information.

## 1. Repo -> skill -> company matrix

| Repo | Proves | Google | Stripe | Neo4j | Anthropic |
|---|---|:-:|:-:|:-:|:-:|
| distributed-transaction-engine | correctness, idempotency, failure handling | high | **highest** | med | high |
| event-driven-platform | streaming, ordering, schema evolution, scale | **high** | high | med | high |
| internal-developer-platform | SRE, K8s, deploy safety, security, cost | **highest** | high | med | **high** |
| knowledge-graph-platform | graph internals, RAG, evals | med | med | **highest** | **high** |
| llm-obs-policies | governance, agent safety, GCP, evals | high | med | low | **highest** |

## 2. What each company usually evaluates (general shape — confirm current format)
- **Google (L6/L7 SWE/architect-type roles):** coding rounds still matter (algorithms, medium-hard), plus 1-2 system design rounds, plus "Googleyness & leadership" behavioral. Depth in distributed systems and trade-off reasoning. Staff+ includes cross-team influence and ambiguity stories.
- **Stripe:** practical, collaborative rounds — programming exercise on realistic code, integration/API round (read docs, call and extend an API), bug-squash round (debug unfamiliar codebase), system design with emphasis on correctness, reliability, API ergonomics; behavioral/"user-first" and writing/communication matter. Rigor around money and idempotency.
- **Neo4j:** depends on role (engineering vs solutions architecture vs field). Expect graph modeling, Cypher, performance diagnosis, architecture of customer scenarios, internals (storage, clustering) for engineering roles, plus communication with customers for SA roles. Show product curiosity and real graph projects.
- **Anthropic:** technical depth plus strong mission alignment. Expect system design (often LLM/infra flavored: inference serving, rate limiting, evals, sandboxes, agents), coding, and behavioral/values conversations about safety and responsibility. Reliability, security, and honest reasoning about risk are prized.

## 3. Cross-cutting preparation tracks (run in parallel the whole 16 weeks)

### Track A — System design fundamentals (2 sessions/week)
- [ ] Master the template: requirements -> estimates -> API -> data model -> high-level -> deep dives -> bottlenecks -> failure -> evolution
- [ ] 40 canonical designs, 2 per week minimum, timed 45 min, then self-critique with a checklist. Must include: URL shortener, rate limiter, KV store, distributed cache, message queue, notification system, news feed, chat, search/typeahead, video streaming, file sync, ride matching, payment system, ledger, webhook platform, metrics/monitoring system, job scheduler, distributed lock service, feature flags, CDN, web crawler, ad/click aggregation, LLM inference gateway, vector DB, RAG platform, agent sandbox, eval platform, token metering/billing, multi-tenant SaaS, global deployment system
- [ ] Back-of-envelope fluency: powers of two, latency numbers, QPS/storage/bandwidth math in under 5 minutes
- [ ] Keep a "patterns notebook": sharding, replication, consensus, caching, queues, idempotency, backpressure, bulkheads, circuit breakers, retries with jitter, fencing, CRDTs, leader election, consistent hashing, Bloom filters, LSM vs B-tree, WAL, snapshotting

### Track B — Coding (3 short sessions/week; required for Google/Stripe/Anthropic)
- [ ] 150 problems: arrays/strings, hashing, two pointers, sliding window, stack/heap, trees, graphs (BFS/DFS/topological/Dijkstra/union-find), DP (1D/2D/knapsack), intervals, backtracking, tries, binary search on answer
- [ ] Practice in your interview language; talk aloud; write tests
- [ ] Stripe-style: also practice reading real code, parsing messy specs, and building small realistic features (rate limiter, ledger, invoice calculator, log parser) in 60 minutes

### Track C — Distributed systems theory (1 reading/week)
Order: DDIA -> Raft -> Paxos Made Simple -> Spanner -> Dynamo -> Bigtable -> Chubby -> MapReduce -> Borg -> Dapper -> Zanzibar -> Kafka paper -> "Life beyond distributed transactions" -> "The Tail at Scale" -> FLP and CAP statements (precise versions) -> Jepsen analyses (read 3: Postgres, Kafka, MongoDB)

### Track D — Behavioral and leadership (1 session/week)
- [ ] Write 8 stories in STAR+reflection format, each tagged with competencies: ownership, ambiguity, conflict, failure/learning, influence without authority, mentoring, technical disagreement, delivering under constraints, ethical/safety decision
- [ ] Each story: situation (2 sentences), your specific actions, measurable result, what you'd change. Rehearse to 2-3 minutes each
- [ ] Staff-level signals: scope beyond a team, multiplying others, setting technical direction, writing influential docs (use your ADRs as evidence)

### Track E — Writing and communication
- [ ] One design doc per month in a style a hiring committee would admire: clear problem, options, recommendation, risks
- [ ] Practice explaining each repo to three audiences: peer engineer (10 min), executive (2 min), junior (5 min)
- [ ] Blog/public artifact: at least 2 posts or talks from this work (e.g. "Idempotency keys end-to-end", "GraphRAG vs vector RAG: measurements") — public work is strong signal

### Track F — Mock interviews
- [ ] Weeks 1-4: self-record; weeks 5-16: at least 1 peer/mentor mock per week, alternate design/coding/behavioral
- [ ] Use paid/peer services (Pramp, interviewing.io, ex-colleagues). Ask for blunt scorecards

## 4. 16-week schedule

| Weeks | Focus | Repo work | Interview prep |
|---|---|---|---|
| 1-2 | Foundations + hygiene | Fix `llm-obs-policies` hygiene (README, ADR numbering, gitignore). Write requirements + C4 + data-model docs in all four platform repos | Template for system design; 10 coding problems/week; write 8 STAR drafts |
| 3-5 | Distributed transaction engine | WP-1..WP-7 (state machine, orchestrator, compensation, outbox, inbox, idempotency) + drills D1-D6 | Designs: payment system, ledger, rate limiter, job scheduler. Read Spanner, Helland, Kleppmann locks |
| 6-8 | Event-driven platform | WP-1..WP-9 (envelope, registry, partitioning, producers/consumers, outbox/CDC, CQRS, replay, DLQ) + benchmarks | Designs: notification system, webhook platform, metrics pipeline, Pub/Sub. Read Kafka paper, Dataflow Model |
| 9-11 | Knowledge graph platform | WP-1..WP-10 (model, governance, ingest, ER, algos, GraphRAG, eval) + benchmarks + MCP server | Neo4j manuals; designs: RAG platform, vector DB, KG at scale; PROFILE practice daily |
| 12-13 | Internal developer platform | WP-3..WP-9 on local kind/ArgoCD/Terraform; game day | Designs: global deployment system, multi-tenant K8s, GPU scheduler, sandbox. Read SRE chapters |
| 14 | LLM governance and observability | `llm-obs-policies` sections 2-4, 6: rule enforcement, agent evals, injection tests, OTel GenAI | Anthropic reading pass; designs: eval platform, token metering, agent sandbox |
| 15 | Integration week | Wire repos together: IDP catalog -> KG; EDP events -> KG; DTE saga for metering; one demo narrative | Full mock loops (4-5 interviews back-to-back) per company flavor |
| 16 | Polish + apply | Record 10-min walkthroughs; fix weakest drills; final docs | Light review, rest, logistics. Start/continue applications earlier (see below) |

**Start applications and referrals by week 6-8**, not week 16 — hiring pipelines take weeks and the interviews themselves give the best practice. Seek referrals at all four companies; use open-source contributions and published writing as the opening.

## 5. Company-specific deep prep checklists

### Google
- [ ] Complete the 40 designs list; focus extra on global-scale, consistency choices, and capacity math
- [ ] Algorithms: top-interview 150 + Google tagged mediums/hards
- [ ] SRE vocabulary and incident narratives
- [ ] Read the Google papers list (Track C)
- [ ] Behavioral: Googleyness stories (comfort with ambiguity, collaboration, intellectual humility, bias to action, doing the right thing)
- [ ] Choose target level realistically; staff-level needs evidence of org-wide influence

### Stripe
- [ ] Use the Stripe API as a daily user: build a toy payments app with idempotency keys, webhooks, test clocks; read their API docs critically (what is great, what you'd change)
- [ ] Implement the integration-round style tasks: authenticate, paginate, handle rate limits and retries, verify webhook signatures
- [ ] Bug-squash practice: clone small OSS repos, inject bugs (or use ones from issue trackers), time-box to 45 minutes
- [ ] Be sharp on: idempotency, ledgers, reconciliation, API versioning, error design, money rounding, rate limiting, online migrations
- [ ] Culture: writing, clarity, user-first, operational rigor

### Neo4j
- [ ] Build/own at least one non-trivial Neo4j project (this repo qualifies once WP-1..WP-10 are done) and a public write-up
- [ ] Take Neo4j GraphAcademy courses (Cypher fundamentals, data modeling, GDS, GenAI) and earn the certifications
- [ ] Be able to PROFILE and fix slow queries live on a whiteboard/screen share
- [ ] Prepare customer scenario answers: sizing, modeling from relational, migration, performance, security, HA/DR
- [ ] Know the competitive landscape (Neptune, TigerGraph, Memgraph, ArangoDB, Postgres + pgvector/AGE, Spanner Graph) and honest positioning
- [ ] For SA roles: practice a 30-minute discovery -> architecture -> POC plan presentation

### Anthropic
- [ ] Use the Claude API and build: tool-use agent, MCP server (graph queries), prompt caching, structured outputs, streaming, batch, evals harness
- [ ] Read all of: "Building effective agents", "Writing tools for agents", "Contextual Retrieval", RSP, MCP spec, Claude Code docs, recent engineering/research blog posts
- [ ] Prepare mission-driven answers that are genuinely yours; avoid generic safety platitudes — be specific about trade-offs and uncertainty
- [ ] Designs to polish: LLM inference gateway (rate limit by tokens, streaming, cancellation, multi-region failover, prompt caching), eval platform, agent sandbox, token metering and billing, RAG + citations, human-in-the-loop approvals
- [ ] Be comfortable with uncertainty, intellectual honesty, saying "I don't know, here's how I'd find out"

## 6. Measurable readiness gates (check at weeks 4, 8, 12, 16)
- [ ] Coding: solve unseen medium in <= 25 min, hard in <= 45 min, >= 70% of the time
- [ ] Design: 8 of last 10 mocks rated "strong hire" by an external reviewer
- [ ] Behavioral: 8 stories rehearsed; no filler; each under 3 minutes
- [ ] Repos: ADRs, benchmarks, and drills present as listed; demo narrative recorded
- [ ] Public: >= 2 published artifacts; >= 1 OSS contribution
- [ ] Applications: referrals requested at all four companies by week 8

## 7. Operating rules for yourself
- Depth beats breadth: finish WPs with acceptance criteria rather than starting new ones
- Delete or implement stubs (`rust-modules`, `cpp-modules`, empty `context.yml` features); empty scaffolding hurts credibility in a code review
- Write down numbers from your own experiments; interviewers trust measured claims
- Sleep, exercise, and schedule rest weeks; interview performance is largely stamina and calm
- Don't claim experience you don't have; say what you built here and what you learned
