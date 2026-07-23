**English** · [Français](README.fr.md)

> [!NOTE]
> **Reserved · future home of Carrière** — developed in the canonical base repository [`libre-ai/libre-ai`](https://github.com/libre-ai/libre-ai) ([multi-repo topology, ADR-0008](https://github.com/libre-ai/libre-ai/blob/main/docs/adr/0008-multi-repo-target-topology-and-brand.md)).
> This repository will reopen as the real product repository when the owner activates it, consuming the base as a versioned dependency. **The specification is currently being shaped** — no product code yet.

# Carrière

**A sovereign job-search assistant for French executives.** Match your profile, skills, and job expectations against real market data — job taxonomy, employer landscapes, salary benchmarks, hiring signals — and get actionable, transparent insights. Never a black box. Never your data traded.

The canonical problem it addresses: _"Where are the jobs I'm qualified for, who's actually hiring, and what are they offering?"_ for French **cadres** (professionals, executives) — a market that sees ~300k openings annually, where the hidden job market dominates but remains unmeasured, and where data fragmentation (France Travail, employer sites, professional networks) makes deliberate search hard.

## Why it's different

- **Sovereign by design.** No cloud vendor lock-in, no algorithmic opaqueness. You own your search history and job alerts; the data flows through infrastructure you can audit or self-host.
- **Built on authoritative sources, not scraping.** Jobs sourced through official APIs ([France Travail Offres d'emploi](https://www.francetravail.io/), ROME 4.0 taxonomy); no reverse-engineering, no TOS violations.
- **Real market data.** Salary bands, hiring frequency by employer and sector, job title mappings, skills-to-roles matching — sourced from official labour statistics and publication-governed APIs.
- **Explainable recommendations.** When a job matches, you see _why_ — which of your stated skills aligned, which experience expectations it meets or misses, which salary ranges are typical for that role. No ranking black box.
- **Built for French professional culture.** ROME codes, cadre-specific job categories, gross salary norms, contract types (CDI/CDD/stage/contrat pro), and location-to-mobility understanding baked in — not retrofitted.

## Status — at the idea stage

Carrière is being shaped as a specification and early feature outline; no product code exists yet. The problem space is well-understood:

- France Travail provides an official jobs API (OAuth2, 24-hour re-query limits, structured data); licence is specific and requires attribution.
- ROME 4.0 is the canonical taxonomy for matching jobs to skills (6, 32, 80, or 492-level granularity available).
- Apec (Association pour l'emploi des cadres, the employer body for French executives) does not expose a public API; cadre-specific job data is currently import-manual or through partner integrations only.
- Salary transparency is limited: EU Directive 2023/970 (pay transparency) is not yet transposed into French law (Council of State decision, June 2026; likely enforcement date 2028). Today, salary ranges are absent from most official postings and must be inferred.
- The hidden job market (informal networks, direct recruiting) is known to dominate but is not quantifiable — claims of "70% of jobs" are unverified and should be rejected.
- Salary transparency is uneven: most postings omit a range, so market pay signals must be reconstructed from sourced references rather than read off the ad.

**Next steps** — the owner will define:

- Core feature set (profile matching, job alerts, salary negotiation guides, employer research tools, interview prep).
- Integration roadmap (France Travail, ROME, partner data sources).
- User experience (mobile-first? desktop? both?).
- Privacy guarantees and data retention policies.

Benchmark target: **to be set by the owner.** Carrière is at the idea stage; its best-in-class parity reference has not been chosen yet.

## Architecture — assembled from shared foundations

Carrière is a product assembled from independently versioned bricks defined in the base repository. Each brick is usable and testable on its own; the product composes them via published packages (the multi-repo target of [ADR-0008](https://github.com/libre-ai/libre-ai/blob/main/docs/adr/0008-multi-repo-target-topology-and-brand.md)).

| Brick                                       | Role                                                      | Status                             |
| ------------------------------------------- | --------------------------------------------------------- | ---------------------------------- |
| **Web platform** (`@libre-ai/web-platform`) | SSR foundation, server-side data fetching                 | Published from base                |
| **Design system tokens**                    | Colour, typography, spacing, responsive scales            | Published from base                |
| **Auth / identity**                         | OAuth2 session handling, France Travail integration       | To be designed, consumed from base |
| **Data contracts**                          | Job posting schema, ROME taxonomy, salary data structures | To be defined in base `contracts/` |

The product host (this repo, at activation) will wire these bricks into a cohesive user experience.

## Where the work happens

When activated, all active development will be in this repository:

- `apps/carriere` — the product host (server-rendered pages, job search interface, alerts and saved searches, employer profiling).
- `src/` — application code, components, integrations with France Travail API.
- `tests/` — end-to-end and unit test suites.

Specification, data contracts, and shared bricks are authored in the base repository, under:

- `docs/apps/carriere.md` — the full product brief (to be written; it will be linked here once it exists).
- `crates/` — Rust backends for job matching, salary modeling, or employment-market analytics (if applicable).
- `packages/` — TypeScript packages for France Travail API integration, ROME taxonomy handling, etc.
- `contracts/` — locked schemas for job postings, user profiles, and saved searches.

To follow progress or contribute, open issues and pull requests in [`libre-ai/libre-ai`](https://github.com/libre-ai/libre-ai). This repository is reserved and will activate upon owner decision.

## License

EUPL-1.2.
