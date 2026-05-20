# XDAO — Architecture

> The Convergent Systems ecosystem, the XDAO federation layer, and the distributed-federation architecture.
>
> Extracted from `aish/ARCHITECTURE.md` — these sections describe ecosystem-wide
> concerns, not aish-specific runtime architecture.

---

## The Convergent Systems Ecosystem

aish does not exist in isolation. It lives in a three-tier ecosystem of decentralized catalogs, a federation layer, and runtimes that consume the catalogs. Each tier has a clearly bounded role.

```
┌──────────────────────────────────────────────────────────────────┐
│                          XDAO (xdao.co)                          │
│             The Federation Layer · Governance · Discovery        │
└────────────┬────────────────────────────────────────┬────────────┘
             │                                        │
             │ federates                              │ federates
             ▼                                        ▼
┌────────────────────────────────────┐    ┌──────────────────────┐
│       The *-Atoms Catalogs         │    │      Runtimes        │
│   Decentralized canonical truth    │    │   What operates      │
│   (what exists)                    │    │   on the catalogs    │
│                                    │    │                      │
│   brand-atoms.com                  │    │   aish               │
│   service-atoms.com                │    │   Olympus            │
│   prompt-atoms.com                 │    │   Universal Bus      │
│   policy-atoms.com                 │    │   (future)           │
│   identity-atoms.com               │    │                      │
│   compliance-atoms.com             │    │                      │
│   workflow-atoms.com               │    │                      │
│   agent-atoms.com                  │    │                      │
│   knowledge-atoms.com              │    │                      │
│   event-atoms.com                  │    │                      │
│   plugin-atoms.com                 │    │                      │
└────────────────────────────────────┘    └──────────────────────┘
```

**Three roles, cleanly separated:**

|Tier      |Role                                                   |Examples                    |
|----------|-------------------------------------------------------|----------------------------|
|Catalogs  |What exists (canonical truth)                          |`*-atoms.com` projects      |
|Federation|Who maintains, how to discover, cross-catalog standards|XDAO                        |
|Runtimes  |What operates on the catalogs                          |aish, Olympus, Universal Bus|

This separation is the architectural expression of the civilization-grade thesis. Catalogs are decentralized infrastructure that should outlast any company. Runtimes are specific implementations. Federation is the meta layer that connects them. Each tier can evolve independently. None depends on the others to exist — catalogs work without runtimes, runtimes can consume catalogs from anyone, federation organizes but does not own.

-----

### XDAO — The Federation Layer

**Domain:** xdao.co

XDAO is the meta layer that federates the Convergent Systems ecosystem. It is not a catalog. It does not own the catalogs. It organizes discovery, governance, and cross-catalog standards.

**XDAO responsibilities:**

- **Discovery** — single entry point listing every `*-Atoms` catalog and runtime in the ecosystem
- **Governance** — cross-catalog standards decisions (e.g., what does “version” mean across catalogs? what is the canonical signing scheme?)
- **Contributor onboarding** — unified flow for new contributors across any catalog or runtime
- **Convening** — the place where catalog maintainers coordinate, propose changes, settle conflicts
- **Brand stewardship** — protects the `*-Atoms` naming convention and architectural principles

**XDAO surface:**

```
xdao.co                       → ecosystem home + thesis
xdao.co/ecosystem             → directory of catalogs and runtimes
xdao.co/dashboard             → status across all catalogs
xdao.co/governance            → cross-catalog standards
xdao.co/contribute            → unified contributor flow
xdao.co/principles            → the civilization-grade properties
```

**Convenience redirects (optional):**

```
brand.xdao.co       → brand-atoms.com
service.xdao.co     → service-atoms.com
prompt.xdao.co      → prompt-atoms.com
...etc
```

The catalogs always live at their canonical `*-atoms.com` domains. XDAO redirects exist for discoverability and convenience, not as primary URLs. This preserves catalog portability — they can be donated, transferred, federated, or maintained by foundations independently of XDAO’s existence.

-----

### The *-Atoms Catalogs

Each catalog follows the same structural pattern established by Brand Atoms:

- **`atoms/`** — reusable building blocks, the smallest units of the domain
- **`compositions/`** — higher-level artifacts assembled from atoms (named differently per catalog: `brands/`, `services/`, `prompts/`, etc.)
- **`rules/`** — typed constraint vocabulary describing how atoms and compositions may be combined

All catalogs are:

- Public git repositories
- YAML-typed and schema-validated
- Versioned with semver
- Machine-consumable (W3C tokens, JSON, structured exports)
- Open source

-----

#### Brand Atoms

**Domain:** brand-atoms.com · **Status:** Existing (236 palettes, 225 fonts, 151 brands)

Machine-readable encyclopedia of brand standards.

|Layer |Contents                                                                                                                    |
|------|----------------------------------------------------------------------------------------------------------------------------|
|Atoms |Palettes (color swatches), fonts (family + weights + license), glyph sets (Nerd Font icon families)                         |
|Brands|Compositions with semantic role mappings — `primary`, `cta`, `heading` — plus structured assets (logos, favicons, og-images)|
|Rules |`contrastRatio`, `fontPairing`, `forbiddenTreatment`, `numericRange`, and 7 other typed constraints                         |

**Runtime consumers:** aish (theming via `brands/shell/` extension), eventually Olympus (overlay theming).

-----

#### Service Atoms

**Domain:** service-atoms.com · **Status:** Proposed (architectural foresight)

Machine-readable encyclopedia of ecosystem-native services. Replaces DNS for services *inside* the ecosystem while remaining decentralized at the implementation layer.

|Layer   |Contents                                                                                                                                                                   |
|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Atoms   |Identity primitives, protocols (http/grpc/queue/event-stream), schemas (request/response/event shapes), policies (auth/rate-limit/retry/circuit-breaker), endpoint patterns|
|Services|Compositions — a service is identity + protocol + schemas + endpoints + policies                                                                                           |
|Rules   |Compatibility between versions, auth-required enforcement, schema-evolution rules                                                                                          |

**Runtime consumers:** Universal Bus (future), aish via plugin once both exist.

-----

#### Prompt Atoms

**Domain:** prompt-atoms.com · **Status:** Proposed (highest reinforcement value)

Machine-readable encyclopedia of LLM prompt fragments. The prompt engineering space has no canonical, machine-readable, versioned library — only closed vendor libraries and scattered cookbooks.

|Layer  |Contents                                                                                                                      |
|-------|------------------------------------------------------------------------------------------------------------------------------|
|Atoms  |Persona fragments, constraint fragments, format instructions, tool-use templates, refusal patterns, output schema declarations|
|Prompts|Compositions — complete prompts, system messages, agent personas                                                              |
|Rules  |Model compatibility, token length constraints, format compatibility                                                           |

**Runtime consumers:** Olympus directly (every agentic phase uses prompts), aish (intent classification and cache compilation primitives).

This is the strongest second catalog to build after Brand Atoms because it has immediate, organic pull from two runtimes already in development.

-----

#### Policy Atoms

**Domain:** policy-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of governance rules. OPA is the closest existing answer but isn’t catalog-driven — every organization reinvents the same policies.

|Layer   |Contents                                                      |
|--------|--------------------------------------------------------------|
|Atoms   |Subjects, resources, actions, effects, conditions             |
|Policies|Compositions — complete policy sets, compliance configurations|
|Rules   |Precedence, conflict resolution, scope inheritance            |

**Runtime consumers:** Olympus governance panels, aish enterprise command gating.

-----

#### Identity Atoms

**Domain:** identity-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of identity primitives. Identity is wildly fragmented across vendors — each provider invents its own claim shapes, trust frameworks, and key types.

|Layer     |Contents                                                                                          |
|----------|--------------------------------------------------------------------------------------------------|
|Atoms     |Auth methods (OAuth, OIDC, SAML, WebAuthn, SSH-key), claim types, trust frameworks, key/cert types|
|Identities|Compositions — identity profiles, federated setups, persona templates                             |
|Rules     |Trust chains, claim validation, key rotation requirements                                         |

**Runtime consumers:** aish identity engine (v0.3 Identity & Secrets), future Universal Bus authentication.

-----

#### Compliance Atoms

**Domain:** compliance-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of compliance frameworks. Every enterprise duplicates this work — mapping the same controls across SOC2, HIPAA, ISO27001, PCI, GDPR. Massive industry-wide redundancy.

|Layer     |Contents                                                                 |
|----------|-------------------------------------------------------------------------|
|Atoms     |Control families, individual controls, evidence types, audit requirements|
|Frameworks|Compositions — SOC2 mapping, HIPAA mapping, ISO27001 mapping, etc.       |
|Rules     |Framework requirements, evidence sufficiency, cross-framework equivalence|

**Runtime consumers:** Olympus governance (compliance evidence collection), aish audit trail.

-----

#### Workflow Atoms

**Domain:** workflow-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of workflow primitives. Workflow tooling is fragmented across n8n, Zapier, GitHub Actions, Temporal, Airflow — each with its own DSL.

|Layer    |Contents                                                                                                   |
|---------|-----------------------------------------------------------------------------------------------------------|
|Atoms    |Step types (action/decision/parallel/loop), trigger types (event/schedule/webhook), state types, gate types|
|Workflows|Compositions — complete workflows (onboarding, deploy, incident response)                                  |
|Rules    |State transition validity, gate requirements, retry policies                                               |

**Runtime consumers:** Olympus agentic loop, future workflow engines.

-----

#### Agent Atoms

**Domain:** agent-atoms.com · **Status:** Proposed (second-highest reinforcement value)

Machine-readable encyclopedia of AI agent primitives. The agent space is exploding with no canonical catalog — every framework (LangChain, AutoGen, CrewAI, Olympus) reinvents personas, tool definitions, and capability declarations.

|Layer |Contents                                                                                   |
|------|-------------------------------------------------------------------------------------------|
|Atoms |Personas, tool definitions, capability declarations, role boundaries, isolation constraints|
|Agents|Compositions — agent definitions, multi-agent systems, swarms                              |
|Rules |Capability grants, isolation rules, communication patterns, supervision hierarchies        |

**Runtime consumers:** Olympus directly (Pantheon Modules are agents), future agent frameworks.

-----

#### Knowledge Atoms

**Domain:** knowledge-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of knowledge graph primitives. Foundation for RAG, semantic memory, and AI knowledge bases.

|Layer          |Contents                                                                             |
|---------------|-------------------------------------------------------------------------------------|
|Atoms          |Entity types, relationship types, provenance atoms, fact types, confidence primitives|
|Knowledge bases|Compositions — ontologies, domain knowledge graphs                                   |
|Rules          |Relationship constraints, provenance requirements, contradiction handling            |

**Runtime consumers:** Olympus Mnemosyne (already implements this concept locally; formalizing it as a catalog makes shared semantic memory possible), future RAG systems.

-----

#### Event Atoms

**Domain:** event-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of event primitives. Pairs with Service Atoms — together they cover the request/response and async halves of distributed systems.

|Layer  |Contents                                                                 |
|-------|-------------------------------------------------------------------------|
|Atoms  |Event types, schemas, channels, subscription patterns, delivery semantics|
|Streams|Compositions — event streams, event-driven architectures                 |
|Rules  |Schema evolution, delivery guarantees, ordering constraints              |

**Runtime consumers:** Universal Bus (future, when both exist), event-driven service implementations.

-----

#### Plugin Atoms

**Domain:** plugin-atoms.com · **Status:** Proposed

Machine-readable encyclopedia of plugin interface standards. **Not a catalog of plugin implementations** — npm, cargo, Homebrew already do that. Plugin Atoms is the interface *standard* that makes plugins ecosystem-portable across Convergent Systems runtimes. USB-C for software plugins.

|Layer      |Contents                                                                                                                   |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|Atoms      |Interface contracts (JSON-RPC specs), capability declarations, permission scopes, lifecycle hooks, trust/signing primitives|
|Conventions|Compositions — inference-plugin, transform-plugin, source-plugin, sink-plugin specifications                               |
|Rules      |Capability grants, semver compatibility, trust chain verification                                                          |

**Runtime consumers:** aish (defines its plugins against this standard), Olympus Pantheon Modules (same standard, specialized), future Universal Bus adapters. A “Claude provider” plugin written once can serve all three runtimes.

This is the catalog that emerges *after* observation — build aish and Olympus first, observe the interface patterns, then extract Plugin Atoms as the unified standard.

-----

### Runtimes

**aish** — this project. The shell layer. Consumes Brand Atoms (theming), Plugin Atoms (interface standard), Identity Atoms (identity engine), eventually integrates with Service Atoms via Universal Bus plugin.

**Olympus** — https://github.com/convergent-systems-co/olympus-central · The AI development runtime. Pantheon Modules, governance panels, agentic loop. Consumes Prompt Atoms (every phase), Agent Atoms (Pantheon Module definitions), Knowledge Atoms (Mnemosyne), Policy Atoms (governance), Compliance Atoms (audit), Workflow Atoms (orchestration).

**Universal Bus** — Future peer project. The service-layer runtime. Consumes Service Atoms (service definitions), Event Atoms (async messaging), Identity Atoms (authentication). Decentralized at the implementation layer, cataloged for discovery.

-----

### Catalog Build Order — The Discipline Rule

> Add a new *-Atoms project only when an existing runtime would consume it heavily enough to drive its adoption.

This is the discipline rule that keeps the ecosystem from sprawling. Catalogs without runtime pull become dead repositories.

**Already exists with runtime pull:**

|Catalog    |Pulled by          |
|-----------|-------------------|
|Brand Atoms|aish (v0.2 theming)|

**Highest priority to build next (runtime pull immediate):**

|Catalog        |Pulled by                                                  |
|---------------|-----------------------------------------------------------|
|Prompt Atoms   |Olympus (every agentic phase), aish (intent classification)|
|Agent Atoms    |Olympus (Pantheon Modules are agents)                      |
|Knowledge Atoms|Olympus (Mnemosyne already implements the concept)         |

**Build when supporting runtimes mature:**

|Catalog         |Pulled by                                 |
|----------------|------------------------------------------|
|Identity Atoms  |aish v0.3 (Identity & Secrets)            |
|Policy Atoms    |Olympus governance + aish enterprise      |
|Compliance Atoms|Olympus governance                        |
|Workflow Atoms  |Olympus agentic loop                      |
|Plugin Atoms    |aish + Olympus (extract after observation)|

**Build when companion runtime exists:**

|Catalog      |Pulled by                              |
|-------------|---------------------------------------|
|Service Atoms|Universal Bus (which doesn’t exist yet)|
|Event Atoms  |Universal Bus + service implementations|

The domains are all secured. The catalogs are built when runtimes need them — not before.



---


## XDAO — Distributed Federation Architecture

XDAO is the federation layer that connects the ecosystem. To be true to the civilization-grade thesis, XDAO itself must be distributed — not just the things it federates. A centralized federation undermines decentralized catalogs by introducing a single point of failure, capture, or corruption.

This section specifies what XDAO needs to be, what it needs to build, and how it stays distributed through every phase of its evolution.

-----

### Why XDAO Must Be Distributed

If catalogs are decentralized but their federation is centralized, the federation becomes the bottleneck. Three concrete risks:

1. **Single point of failure** — XDAO going offline makes catalog discovery break
1. **Capture risk** — whoever controls XDAO can dictate what counts as a legitimate catalog
1. **Steward exit** — if Convergent Systems pivots or dissolves, the ecosystem dies with it

A distributed XDAO has the opposite properties:

- **Survives infrastructure failure** — multiple mirrors, content addressing, no central server
- **Cannot be captured** — governance is transparent, decisions are auditable, forks are possible
- **Outlasts its founders** — design assumes Convergent Systems is one steward among possibly many, replaceable without ecosystem disruption

-----

### Distribution Dimensions

XDAO is distributed across five dimensions. Each must be designed deliberately.

|Dimension             |Centralized failure mode         |Distributed design                                                                              |
|----------------------|---------------------------------|------------------------------------------------------------------------------------------------|
|**Hosting**           |One server, one CDN, one provider|Static site mirrored across Cloudflare Pages, GitHub Pages, IPFS                                |
|**Discovery**         |One registry endpoint            |`catalogs.yml` in git, mirrored, content-addressable; XDAO CLI resolves locally                 |
|**Governance**        |One company decides              |Public XAIP process via GitHub, multi-maintainer approval, public discussion                    |
|**Identity**          |One auth provider                |GitHub identity + Sigstore signing + maintainer web of trust                                    |
|**Dispute resolution**|Vendor controls                  |RFC-style working groups; fork-friendly architecture means dissatisfied parties can route around|

-----

### Components to Build

XDAO is several components working together, not a single application. Each has its own repository and lifecycle.

#### Core repositories

|Repo         |Purpose                                                                                                                                 |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------|
|`xdao`       |Federation portal source (Astro static site, deployed Cloudflare Pages) — directory of catalogs and runtimes, dashboard, governance docs|
|`atoms-spec` |Canonical JSON Schemas, validation rules, and conventions every `*-Atoms` catalog conforms to. Versioned (`v1`, `v2`, etc.).            |
|`atoms-tools`|Cross-platform CLI (Go) — validation, export generation, schema migration, catalog bootstrapping, federation resolution                 |
|`xaips`      |XDAO AI Improvement Proposals — RFC-style proposal repository, working group coordination                                               |

#### Static services

|Service            |Purpose                                                             |Distribution                                 |
|-------------------|--------------------------------------------------------------------|---------------------------------------------|
|`xdao.co`          |Federation landing, ecosystem directory, governance home            |Cloudflare Pages + GitHub Pages mirror       |
|`xdao.co/dashboard`|Live status across catalogs (fetches each catalog’s `manifest.json`)|Static at build time, JS-refreshed in browser|
|`xdao.co/ecosystem`|Browsable directory of all catalogs                                 |Generated from `catalogs.yml`                |
|IPFS pin set       |Content-addressable copy of all catalog manifests                   |Pinned via multiple IPFS pinning services    |

-----

### Phased Build Plan for XDAO

XDAO does not need to be fully distributed on day one. It needs to be *designed* for distribution from day one. The phases evolve from “Convergent Systems hosts everything transparently” to “true federated infrastructure.”

#### Phase 1 — Bootstrap (essential for ecosystem to function)

The minimum XDAO needed for the first `*-Atoms` catalogs and runtimes to interoperate.

**Epic XDAO-1: Federation Portal**

- [ ] `xdao` repo created — Astro static site, follows Brand Atoms deployment pattern
- [ ] `xdao.co` deployed to Cloudflare Pages
- [ ] Landing page — civilization-grade thesis, ecosystem overview, navigation
- [ ] `/ecosystem` directory page — lists all catalogs and runtimes from `catalogs.yml`
- [ ] `/principles` page — the civilization-grade properties
- [ ] `/contribute` page — onboarding for catalogs, runtimes, contributors

**Epic XDAO-2: Federation Registry**

- [ ] `ecosystem/catalogs.yml` schema definition
- [ ] `ecosystem/runtimes.yml` schema definition
- [ ] Initial entries: brand-atoms (existing), aish (proposed), olympus (existing)
- [ ] Validation CI for additions
- [ ] PR template for catalog/runtime registration

**Epic XDAO-3: Atoms Spec v1**

- [ ] `atoms-spec` repo created
- [ ] Base JSON Schemas — atom, composition, rule
- [ ] Required manifest schema (`ATOMS.yml`)
- [ ] Export format specifications (manifest.json, catalog.json, tokens.json)
- [ ] Conformance test suite — any catalog can run this to verify it conforms
- [ ] `v1` tagged release

**Epic XDAO-4: Atoms Tools CLI**

- [ ] `atoms-tools` repo created (Go, cross-platform binary)
- [ ] `atoms validate <path>` — validates a catalog against atoms-spec
- [ ] `atoms export <path>` — generates required exports
- [ ] `atoms bootstrap <name>` — scaffolds a new conforming catalog repo
- [ ] `atoms resolve <ref>` — resolves a federation reference like `prompt-atoms://persona/peer-programmer`
- [ ] Release pipeline — binaries for macOS, Linux, Windows

**Epic XDAO-5: XAIP Process**

- [ ] `xaips` repo created
- [ ] XAIP template (status, type, abstract, motivation, specification, rationale)
- [ ] First XAIP: “XAIP-0 — XAIP Process” (self-defining)
- [ ] XAIP-1: “Atoms Catalog Standard v1” (codifies atoms-spec)
- [ ] XAIP-2: “Federation Registry Schema”
- [ ] XAIP-3: “Catalog Signing Requirements”
- [ ] Public discussion mechanism — GitHub Discussions per XAIP

-----

#### Phase 2 — Distribution (makes it genuinely decentralized)

Once Phase 1 is operational, distribution mechanisms are added to remove single-point-of-failure risks.

**Epic XDAO-6: Multi-Endpoint Hosting**

- [ ] `xdao.co` mirrored to GitHub Pages at `convergent-systems-co.github.io/xdao`
- [ ] IPFS pinning for site content (Filebase or Pinata)
- [ ] DNS failover configuration documented
- [ ] Each catalog’s `manifest.json` mirrored to IPFS on publish

**Epic XDAO-7: Sigstore Signing**

- [ ] Catalog releases signed with Sigstore (`cosign`)
- [ ] `atoms-tools` verifies signatures on resolve
- [ ] XDAO directory displays signature status for each catalog
- [ ] Signing requirements codified in XAIP-3

**Epic XDAO-8: Federation Protocol**

- [ ] Federation manifest schema — how an external registry announces itself
- [ ] Discovery protocol — how XDAO discovers catalogs registered with other federations
- [ ] Cross-federation reference syntax — `<federation>/<catalog>/<atom>`
- [ ] Trust policies — how XDAO decides to surface external federations
- [ ] Reference implementation — XDAO federates with itself across mirrors as a test

**Epic XDAO-9: Local Resolver**

- [ ] `atoms-tools` ships with offline catalog cache
- [ ] `atoms resolve` works offline against cached catalogs
- [ ] Cache sync via git pull or IPFS fetch
- [ ] Runtimes (aish, Olympus) embed atoms-tools to resolve catalog refs locally without network

-----

#### Phase 3 — Governance Maturity (true distributed governance)

Phases 1 and 2 still have Convergent Systems as the practical steward. Phase 3 is where governance becomes genuinely distributed and the ecosystem is not dependent on any one entity.

**Epic XDAO-10: Multi-Maintainer Governance**

- [ ] XAIP working groups formed per catalog
- [ ] Multi-sig approval for atoms-spec changes (`3-of-5` model with rotating signers)
- [ ] Maintainer onboarding/offboarding process via XAIP
- [ ] Conflict-of-interest disclosure requirements
- [ ] Public maintainer roster with affiliations

**Epic XDAO-11: Dispute Resolution Framework**

- [ ] Formal dispute escalation process via XAIP
- [ ] Working group mediation protocol
- [ ] Fork-friendly licensing review — confirm anyone can fork any catalog
- [ ] Brand stewardship rules — what conditions allow `*-Atoms` naming to be used
- [ ] Sunset/archival process — how a catalog moves to deprecated or archived status

**Epic XDAO-12: Foundation Pathway**

- [ ] Legal framework for transferring stewardship to a foundation (CNCF, Linux Foundation, custom)
- [ ] Trademark assignment plan
- [ ] Domain transfer procedures documented
- [ ] Funding model independent of Convergent Systems (donations, grants, sponsored work)
- [ ] Triggers — what events cause a foundation transfer to begin

This phase is *aspirational on a multi-year timeline*. It is included in the architecture so the design from Phase 1 onward never makes choices that prevent it.

-----

### The XAIP Process

XAIPs (XDAO AI Improvement Proposals) are how the ecosystem makes decisions. The format is intentionally similar to Python’s PEPs, IETF’s RFCs, and Bitcoin’s BIPs — proven processes for distributed standards.

**Each XAIP has:**

```yaml
xaip: 0007
title: Cross-Catalog Reference Syntax
author: maintainer@example.org
status: draft       # draft, review, accepted, rejected, withdrawn, deprecated
type: standards     # standards, informational, process
created: 2026-06-01
discussion: https://github.com/convergent-systems-co/xaips/discussions/42
```

**Process flow:**

```
Idea → Draft XAIP → Public discussion → Working group review →
  Acceptance vote → Status: accepted → Implementation across catalogs/runtimes
```

**Distributed properties:**

- All discussion is in public GitHub repos — auditable, forkable, mirrorable
- Working group composition is published
- Votes are recorded with reasoning
- Rejected XAIPs remain in the record for future reference
- Anyone can submit a XAIP — acceptance requires working group approval

-----

### Federation Protocol — How XDAO Federates

XDAO is not the only possible federation. The protocol assumes other federations exist and interoperate.

**A federation announces itself via a federation manifest:**

```yaml
# Hosted at <federation-url>/federation.yml
federation: xdao
domain: xdao.co
catalogs:
  url: https://xdao.co/ecosystem/catalogs.json
runtimes:
  url: https://xdao.co/ecosystem/runtimes.json
signing:
  protocol: sigstore
  trust-roots:
    - sigstore-public
xaip-process:
  url: https://github.com/convergent-systems-co/xaips
  type: rfc-style
contact: maintainers@xdao.co
```

**Other federations could exist:**

- A regional federation (`xdao-eu.org`) that adds EU compliance requirements
- A vertical federation (`fintech-atoms.org`) that adds financial industry standards
- An organization’s private federation (`acme-corp-atoms.internal`)

All discover each other through cross-federation manifests. Catalogs can register with multiple federations. Runtimes consume from whichever federations they trust.

This is what makes XDAO genuinely distributed — it is *one* federation among possibly many, not *the* federation.

-----

### XDAO Build Sequence and Dependencies

```
Phase 1 (Bootstrap) ───┬──► aish v0.1 can launch
                       │   (uses atoms-tools for Brand Atoms theming)
                       │
                       └──► Olympus integration roadmap unblocked
                            (uses atoms-spec for Pantheon Module standardization)

Phase 2 (Distribution) ───► aish v0.3 can ship Olympus plugin with full federation
                            Catalogs become genuinely independent

Phase 3 (Governance) ─────► Multi-year. Triggers when:
                            - Multiple catalogs have non-Convergent maintainers
                            - Annual ecosystem usage exceeds defined thresholds
                            - External adoption justifies foundation transfer
```

**Critical path for aish v0.1 launch:** Epic XDAO-1, XDAO-2, XDAO-3, and XDAO-4 (Phase 1 minus XAIP process formalization) must exist before aish v0.1 can consume Brand Atoms via the standardized atoms-tools pipeline. XAIP process can lag and be documented as catalogs and runtimes ship.

-----

### What XDAO Is Not

To avoid scope creep, XDAO is explicitly NOT these things:

- **Not a hosted runtime** — it does not run anyone’s services, agents, or shells
- **Not a package registry** — npm, cargo, PyPI handle implementation distribution; XDAO catalogs *contracts* and standards
- **Not a token economy** — no cryptocurrency, no monetary incentives, no on-chain anything (Phase 3 may revisit if it serves a real need)
- **Not a single application** — it is a set of repos, schemas, processes, and a static site
- **Not a vendor product** — it is open infrastructure that Convergent Systems initially stewards but does not own permanently

These exclusions keep XDAO focused on its actual role: federating decentralized catalogs and standardizing the conventions that make them interoperable.

-----


---

## Related

- **The Atoms Catalog Standard** — the canonical specification every `*-Atoms` catalog conforms to — lives in **[atoms-spec](https://github.com/convergent-systems-co/atoms-spec)**.
- **The atoms umbrella** — every catalog as a git submodule — is **[atoms](https://github.com/convergent-systems-co/atoms)**.
- **Runtime: aish** — the shell that consumes Brand Atoms, Plugin Atoms, Identity Atoms — is **[aish](https://github.com/convergent-systems-co/aish)**.
