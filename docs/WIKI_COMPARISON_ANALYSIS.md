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
- Install via **Settings → Community Nodes → Install** using package name `n8n-nodes-directus`
- Also documents npm install: `cd ~/.n8n/nodes && npm install n8n-nodes-directus`
- No mention of the node palette search method
- No mention of verification badge

### Gap Analysis:
| Issue | Severity | Detail |
|-------|----------|--------|
| **Contradictory install method** | HIGH | Our README recommends the exact method the wiki warns against (Settings → Community Nodes). The wiki says this can cause "Package is not vetted" errors. |
| **Missing palette search method** | HIGH | The wiki's recommended installation path isn't documented in our README at all. |
| **No verification badge mention** | MEDIUM | The wiki emphasizes looking for a verification badge; our docs don't reference it. |
| **Missing env var note** | LOW | `N8N_COMMUNITY_PACKAGES_ENABLED=true` for self-hosted users not mentioned. |

### Recommendation:
Update README.md installation section to match the wiki's recommended method (node palette search) as the primary approach. Keep npm install as an alternative for advanced users. Remove or demote the Settings → Community Nodes method.

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
| Issue | Severity | Detail |
|-------|----------|--------|
| **Wiki omits OAuth2** | INFO | The wiki only covers static tokens. Our docs cover OAuth2 which is additional value, not a conflict. |
| **Wiki omits Email/Password** | INFO | The wiki doesn't mention email/password auth. This is fine — it's a simpler guide. |
| **Token save step** | LOW | The wiki emphasizes clicking the checkmark (✓) to save the token — a common gotcha. Our docs don't mention this UX detail. |
| **Admin vs Custom role guidance** | LOW | Wiki's framing (Admin for getting started, Custom for production) is clearer than ours. |

### Recommendation:
Add the "click checkmark to save" note to our credential docs. Adopt the wiki's Admin/Custom role framing.

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
| Issue | Severity | Detail |
|-------|----------|--------|
| **Missing wiki troubleshooting items** | HIGH | The three most common installation issues from the wiki are completely absent from our TROUBLESHOOTING.md. These are likely the first errors users encounter. |
| **Docker guidance missing** | MEDIUM | Volume persistence issue not documented in our troubleshooting guide. |
| **Our docs have more depth** | INFO | Our troubleshooting covers operational issues the wiki doesn't address (good — complementary). |

### Recommendation:
Add the wiki's three troubleshooting scenarios to the top of our TROUBLESHOOTING.md since they're the first problems users encounter (installation/loading issues).

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
- Package name: `n8n-nodes-directus` (no `@directus/` scope)
- Repository: `arladmin/n8n-nodes-directus` on GitHub

### Gap Analysis:
| Issue | Severity | Detail |
|-------|----------|--------|
| **Scoped vs unscoped package name** | HIGH | The wiki references `@directus/n8n-nodes-directus` while our package is `n8n-nodes-directus`. This is either a different package entirely, or a namespace discrepancy that needs resolution. |

### Recommendation:
Investigate whether `@directus/n8n-nodes-directus` is a separate official Directus package, or if the wiki is referencing this project under a different scope. This affects whether our project is actually the "verified" node the wiki describes.

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

### Critical Issues (Action Required)

1. **Installation method contradiction** — Our docs recommend the method the wiki explicitly warns against. Users following our README may hit the "not vetted" error.

2. **Missing Trigger Node** — The wiki describes a "Directus Trigger Node" as a separate component. Our package only exports one node. This creates user confusion.

3. **Package name mismatch** — Wiki references `@directus/n8n-nodes-directus` vs our `n8n-nodes-directus`. Need to clarify if these are the same or different packages.

4. **Missing installation troubleshooting** — The three most common issues (stale files, not vetted, Docker persistence) aren't in our troubleshooting docs.

### Moderate Issues (Should Fix)

5. **No node palette search documentation** — The wiki's primary install method isn't in our docs.
6. **Token save UX note missing** — The "click checkmark to save" gotcha isn't documented.
7. **Docker volume guidance missing** — No Docker-specific setup advice.

### Informational (Nice to Have)

8. **Our docs are far more comprehensive** — The wiki is a quick-start; our docs cover 19 resources, AI agents, analytics, etc.
9. **No cross-referencing** — Neither our docs nor the wiki link to each other.
10. **OAuth2 not in wiki** — Our OAuth2 support is additional value not covered by the wiki.

---

## Recommended Action Plan

### Priority 1: Fix Installation Docs
- Update README to use node palette search as primary install method
- Add verification badge mention
- Add `N8N_COMMUNITY_PACKAGES_ENABLED=true` note for self-hosted

### Priority 2: Add Wiki Troubleshooting Items
- Add "package could not be loaded" fix (clear `~/.n8n/nodes`)
- Add "not vetted" error fix (use palette search, not Settings)
- Add Docker volume persistence guidance

### Priority 3: Resolve Identity Questions
- Investigate `@directus/n8n-nodes-directus` vs `n8n-nodes-directus`
- Determine if a separate Trigger Node should be built
- Coordinate with Directus team on wiki accuracy

### Priority 4: Enhance Documentation
- Add "click checkmark" token save note
- Add link to official Directus wiki
- Consider proposing wiki updates to better reflect full feature set
