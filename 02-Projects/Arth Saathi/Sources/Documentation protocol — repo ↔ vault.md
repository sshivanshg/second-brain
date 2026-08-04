---
created: 2026-05-24
type: reference
tags: arth-saathi, sources, meta, ai-workflow
repo_enforcer: /Users/shivanshgupta/projects/arth/CLAUDE.md
---

# Documentation protocol — repo ↔ vault

> **The deal:** the **repo holds truth**, the **vault holds thinking**. Code, specs, migrations, and `.env` live in `/Users/shivanshgupta/projects/arth`. This vault holds the *why*, the *map*, and the *narrative* — linked, atomic, durable. This note is the bridge between them and the rule for keeping them in sync.

This protocol is **enforced from the repo**: `CLAUDE.md` instructs every Claude Code session to update this vault when it ships product/tech work. So "AI documents everything here" is automatic, not a chore. See `repo_enforcer` above.

## When the AI writes here
After shipping any product-facing or architecturally meaningful change in the repo, the session also:
1. **Writes the repo-canonical doc** (e.g. `docs/<feature>.md`) — that is the source of truth.
2. **Creates/updates a vault note** in the right section below (thinking + map + a `repo_doc:` pointer — *not* a copy of the spec).
3. **Links it into** the [[Arth Saathi]] hub MOC under the matching section.
4. **Appends a dated entry** to [[Working log — Arth Saathi]].
5. Links to any relevant [[Arth Saathi#🧠 Load-bearing beliefs (atomic — reusable)|load-bearing belief]] (and creates the belief note if the idea is reusable).

## Section map (where things go)
| Kind of work | Vault folder | Example |
|---|---|---|
| Strategy / positioning | `Strategy/` | [[The Enemy — what we're displacing]] |
| GTM / pricing / business | `GTM/` | [[GTM strategy — PLG + WhatsApp viral + CA partner]] |
| Product feature | `Product/` | [[Aadhaar eKYC — feature]], [[Feature catalog]] |
| Architecture / implementation | `Tech/` | [[Aadhaar KYC — implementation]], [[Architecture — Arth Saathi]] |
| Fundraising | `Fundraising/` | [[Investor pipeline]] |
| Repo pointers | `Sources/` | [[Source-of-truth pointers — Arth Saathi]] |
| Decisions / shipped log | `Log/` | [[Working log — Arth Saathi]] |

## Conventions (match existing notes)
- **Frontmatter:** `created: YYYY-MM-DD`, `type:` (`product` / `architecture` / `reference` / `log` / `project-moc`), `tags: arth-saathi, <topic>`. Add `repo_doc:` pointing at the canonical repo file.
- **Naming:** tech/architecture notes use `… — Arth Saathi` (em-dash). Product/feature notes use `<Feature> — feature`.
- **Links:** liberal `[[wikilinks]]`; every note ends with a `## Related` block linking neighbours + the hub.
- **Voice:** narrative and opinionated — the *why* and trade-offs, not a line-by-line spec dump. Spec belongs in the repo.
- **Truth pointers:** when a vault claim changes, update the vault note *and* its repo source.

## Worked example (2026-05-24)
Aadhaar eKYC shipped → repo `docs/AADHAAR_KYC.md` (truth) + [[Aadhaar eKYC — feature]] (product thinking) + [[Aadhaar KYC — implementation]] (tech map) + hub links + [[Working log — Arth Saathi]] entry. This note + the `CLAUDE.md` section were created in the same pass to make the loop self-sustaining.

## Related
- [[Arth Saathi]]
- [[Source-of-truth pointers — Arth Saathi]]
