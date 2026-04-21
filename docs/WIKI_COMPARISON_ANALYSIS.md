# Official Directus Wiki vs. Project Documentation: Comparison Analysis

## Overview

This analysis compares the [official Directus wiki page for n8n integration](https://docs.directus.io) against the documentation and actual implementation in the `n8n-nodes-directus` project repository.

---

## 1. Installation Instructions

### Official Wiki Says:
- Install via **node palette search** (click `+` button, search "Directus")
- Explicitly warns **against** using Settings → Community Nodes
- Mentions the node is "verified" with a verification badge/shield icon
- Self-hosted users may need `N8N_COMMUNITY_PACKAGES_ENABLED=true`

### Our README Says:
- ~~Install via **Settings → Community Nodes → Install** using package name `n8n-nodes-directus`~~ **FIXED** — now uses palette search as primary method
- Also documents npm install: `cd ~/.n8n/nodes && npm install n8n-nodes-directus`
- ~~No mention of the node palette search method~~ **FIXED**
- ~~No mention of verification badge~~ **FIXED**

### Gap Analysis:
| Issue | Severity | Status | Detail |
|-------|----------|--------|--------|
| **Contradictory install method** | HIGH | ✅ FIXED | README now uses node palette search as primary method. |
| **Missing palette search method** | HIGH | ✅ FIXED | Palette search is now the documented primary path. |
| **No verification badge mention** | MEDIUM | ✅ FIXED | Verification badge / shield icon mentioned in README. |
| **Missing env var note** | LOW | ✅ FIXED | `N8N_COMMUNITY_PACKAGES_ENABLED=true` documented for self-hosted. |

### Recommendation:
~~Update README.md installation section to match the wiki's recommended method (node palette search) as the primary approach. Keep npm install as an alternative for advanced users. Remove or demote the Settings → Community Nodes method.~~ **DONE**

---

## 2. Credential Configuration

### Official Wiki Says:
- Two fields: **Directus URL** and **Token**
- Token obtained from: Users → select user → Token section → Generate Token → click checkmark to save
- Recommends **Admin token** for getting started
- Recommends **Custom role** for production (principle of least privilege)
- No mention of OAuth2

### Our README Says:
- Documents both **Static Token** and **OAuth2** authentication
- More detailed permission guidance per operation type
- References credential creation via n8n UI

### Our Credential Files Support:
- Static Token (DirectusApi)
- Email & Password (DirectusApi)
- OAuth2 with 6 SSO providers (DirectusOAuth2Api)

### Gap Analysis:
| Issue | Severity | Status | Detail |
|-------|----------|--------|--------|
| **Wiki omits OAuth2** | INFO | N/A | The wiki only covers static tokens. Our docs cover OAuth2 which is additional value, not a conflict. |
| **Wiki omits Email/Password** | INFO | N/A | The wiki doesn't mention email/password auth. This is fine — it's a simpler guide. |
| **Token save step** | LOW | ✅ FIXED | README now emphasizes clicking the checkmark (✓) to save the token. |
| **Admin vs Custom role guidance** | LOW | ✅ FIXED | README now uses wiki's framing (Admin for getting started, Custom for production). |

### Recommendation:
~~Add the "click checkmark to save" note to our credential docs. Adopt the wiki's Admin/Custom role framing.~~ **DONE**

---

## 3. Available Nodes

### Official Wiki Says:
- **Directus Node**: "Perform CRUD operations on items, users, and files"
- **Directus Trigger Node**: "Automatically start workflows when events occur in Directus"

### Our Project Actually Provides:
- **Directus Node** with **19 resources** (Items, Users, Files, Activity, Collections, Fields, Relations, Permissions, Roles, Folders, Webhooks, Settings, Auth, Server, Assets, Extensions, Utils, Presets, Flows) and 100+ operations
- **No dedicated Trigger Node** — triggers are handled via the main node's Flow and Webhook resources

### Gap Analysis:
| Issue | Severity | Detail |
|-------|----------|--------|
| **Wiki mentions Trigger Node** | HIGH | The wiki references a "Directus Trigger Node" as a separate component. Our package does not export a separate trigger node. The `package.json` only lists one node: `Directus.node.js`. |
| **Wiki undersells capabilities** | INFO | The wiki describes basic "CRUD on items, users, and files" while we support 19 resources with advanced features (AI agent tools, flow automation, revision comparison, analytics, etc.). |

### Recommendation:
Clarify whether a separate Directus Trigger node should be built (to match wiki expectations) or whether the wiki needs updating. If users arrive expecting a Trigger node and can't find one, this creates confusion.

---

## 4. Troubleshooting

### Official Wiki Covers:
1. **"The specified package could not be loaded"** — stale files in community nodes directory; fix by clearing `~/.n8n/nodes` and reinstalling
2. **"Package is not vetted for installation"** — caused by installing via Settings → Community Nodes; fix by using palette search instead
3. **Docker volume persistence** — `~/.n8n/nodes` must be in a persistent volume

### Our TROUBLESHOOTING.md Covers:
1. Connection issues (ECONNREFUSED, timeouts)
2. Authentication errors (invalid token, OAuth2 failures)
3. Flow operation issues (404, timeouts, no data)
4. User management errors (role errors, duplicates, invitations)
5. Preset query problems
6. Activity log issues
7. AI agent tool problems
8. Performance issues

### Gap Analysis:
| Issue | Severity | Status | Detail |
|-------|----------|--------|--------|
| **Missing wiki troubleshooting items** | HIGH | ✅ FIXED | Installation Issues section added to top of TROUBLESHOOTING.md with all three wiki scenarios plus a fourth (community packages env var). |
| **Docker guidance missing** | MEDIUM | ✅ FIXED | Docker volume persistence documented in both README and TROUBLESHOOTING.md. |
| **Our docs have more depth** | INFO | N/A | Our troubleshooting covers operational issues the wiki doesn't address (good — complementary). |

### Recommendation:
~~Add the wiki's three troubleshooting scenarios to the top of our TROUBLESHOOTING.md since they're the first problems users encounter (installation/loading issues).~~ **DONE**

---

## 5. Documentation Links & Structure

### Official Wiki References:
- Working with Directus Actions → `/guides/integrations/n8n/directus-n8n-actions`
- Using Directus Triggers → `/guides/integrations/n8n/directus-n8n-triggers`
- Advanced Features → `/guides/integrations/n8n/directus-n8n-advanced`

### Our Documentation Structure:
- `README.md` — main overview and usage
- `MIGRATION.md` — v1.x to v2.0 migration
- `docs/API_REFERENCE.md` — operation reference
- `docs/ARCHITECTURE.md` — technical design
- `docs/TROUBLESHOOTING.md` — issue resolution
- `docs/CONTRIBUTING.md` — contributor guide
- `docs/agent-setup-guide.md` — AI agent setup
- `examples/` — workflows, prompts, conversations

### Gap Analysis:
| Issue | Severity | Detail |
|-------|----------|--------|
| **Wiki links to external guides** | INFO | The wiki references separate Directus-hosted guides for actions, triggers, and advanced features. We can't control that content. |
| **No cross-referencing** | LOW | Our docs don't reference the official wiki, and the wiki doesn't link to our repo docs. |

### Recommendation:
Add a link to the official Directus wiki in our README for users who want the official perspective.

---

## 6. Package Name & Identity

### Official Wiki Says:
- Refers to "Directus community node" generically
- Mentions `@directus/n8n-nodes-directus` as the package name (in the "not vetted" troubleshooting section)

### Our Package:
- Package name in `package.json`: `n8n-nodes-directus` (unscoped)
- Published to npm under the `@directus` scope as `@directus/n8n-nodes-directus`

### Gap Analysis:
| Issue | Severity | Status | Detail |
|-------|----------|--------|--------|
| **Scoped vs unscoped name** | N/A | Not an issue | The unscoped name in `package.json` is the local/dev name. The `@directus/` scope is applied at publish time. No discrepancy. |

---

## 7. Feature Coverage Comparison

### What the Wiki Implies Users Can Do:
- CRUD on items, users, files
- Trigger-based automation
- Webhook management
- "Advanced features" with filter syntax and complex queries

### What Our Node Actually Supports (beyond wiki scope):
- All of the above, plus:
- **19 resources** (vs. 3 mentioned in wiki)
- **Flow automation** (trigger, chain, loop, monitor)
- **AI agent integration** with OpenAI/Claude function calling
- **Activity log analytics** (aggregation, peak usage, error tracking)
- **Revision comparison** with diff generation
- **Preset queries** with reusable filters
- **Bulk operations** with error handling strategies
- **2FA management**
- **OAuth2 authentication** with 6 SSO providers
- **Server utilities** (hash, cache, random string)
- **Asset transformations** (resize, format conversion)

### Gap Analysis:
| Issue | Severity | Detail |
|-------|----------|--------|
| **Significantly undersold** | INFO | The wiki barely scratches the surface of what this node can do. If this is indeed the same package, the wiki should be updated to highlight differentiating features. |

---

## Summary of Findings

### ✅ Fixed

1. ~~**Installation method contradiction**~~ — README now uses palette search as primary install method.
2. ~~**Missing installation troubleshooting**~~ — TROUBLESHOOTING.md has new Installation Issues section with all wiki scenarios.
3. ~~**No node palette search documentation**~~ — Palette search is now the primary documented method.
4. ~~**Token save UX note missing**~~ — "Click checkmark to save" documented in README.
5. ~~**Docker volume guidance missing**~~ — Documented in both README and TROUBLESHOOTING.md.
6. ~~**Package name mismatch**~~ — Not a real issue; unscoped name in `package.json` is published under `@directus` scope.

### Open Issues

7. **Missing Trigger Node** — The wiki describes a "Directus Trigger Node" as a separate component. Our package only exports one node (`Directus.node.ts`). Triggers are handled via the main node's Flow and Webhook resources. **This is the only remaining gap that requires an implementation decision.**

### Informational (No Action Needed)

8. **Our docs are far more comprehensive** — The wiki is a quick-start; our docs cover 19 resources, AI agents, analytics, etc.
9. **No cross-referencing** — Neither our docs nor the wiki link to each other.
10. **OAuth2 not in wiki** — Our OAuth2 support is additional value not covered by the wiki.

---

## Remaining Action Plan

### Build a Directus Trigger Node (Decision Required)

The wiki describes a dedicated **Directus Trigger Node** that "automatically starts workflows when events occur in Directus." Our package currently handles this through the main Directus node's Flow/Webhook resources, but n8n users expect trigger nodes to appear as separate palette entries that can start a workflow.

**Options:**
1. **Build `DirectusTrigger.node.ts`** — A proper n8n trigger node that creates/manages Directus webhooks and listens for events. This matches what the wiki promises and how n8n's trigger architecture works.
2. **Document the current approach** — Explain in README that triggers are available via the main node's Webhook resource, and coordinate with Directus to update the wiki.

### Nice to Have
- Add a cross-reference to the official Directus wiki in our README
- Propose wiki updates to better represent the full feature set (19 resources, AI agent tools, etc.)
