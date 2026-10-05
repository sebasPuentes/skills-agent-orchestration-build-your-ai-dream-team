# Project Pulse Dashboard Implementation Plan

## Summary and goal

Build Mona's lightweight, static **Project Pulse** dashboard for contributors. The
first view should immediately communicate which projects are active, who owns
them, current status, recent activity, and priority or risk. The result should
look like a polished dashboard rather than a directory listing, with readable
project cards, status badges, clear spacing, responsive behavior, and accessible
contrast and semantics.

The repository has no application framework or package manifest. The
implementation should therefore stay dependency-light: browser-native HTML,
CSS, and JavaScript embedded in `app/index.html`, with project content stored in
JSON. The existing repository conventions and exercise validation scripts should
remain unchanged.

## Ordered implementation phases

### Phase 1 — Confirm the contract and visual direction

**Owner:** Orchestrator with Planner and Designer  
**Files:** no implementation files; use `.github/project-pulse-brief.md`,
`docs/agent-team.md`, and the custom agent definitions as reference.

1. Confirm the required fields and visible outcomes from the brief:
   `name`, `owner`, `status`, `recentActivity`, and `priority`.
2. Designer defines the information hierarchy, card anatomy, status/priority
   treatments, responsive layout, typography, spacing, and accessibility rules.
3. Orchestrator converts the design decisions into non-overlapping assignments
   and confirms that the launch configuration serves `app/` and opens
   `index.html`.

### Phase 2 — Produce independent content and design inputs

These tasks can run **in parallel** because they have separate ownership and
neither requires the other to edit the same file:

- **Designer — styling specification for `app/styles.css`:**
  define the `.dashboard` layout hook, `.project-card` card hook, responsive
  grid behavior, status badges, priority/risk emphasis, focus states, color
  contrast, readable spacing, and reduced-motion considerations. The Designer
  should report the expected markup hooks to the Coder without editing the
  implementation files unless explicitly assigned.
- **Coder — data model for `app/project-data.json`:**
  create a top-level `projects` array with representative, contributor-friendly
  project records. Every record must contain `name`, `owner`, `status`,
  `recentActivity`, and `priority`; values should be human-readable and
  consistent enough for badges and sorting/display decisions.

### Phase 3 — Implement the static dashboard

This phase is **sequential after Phase 2**, because the HTML and CSS need the
Designer’s agreed hooks and the page needs the data schema before integration.

- **Coder — `app/index.html`:**
  build the semantic page shell and visible Project Pulse UI. Include an
  accessible heading and introductory context, a dashboard container, project
  card markup with stable `project-card` hooks, status and priority labels, and
  an empty/error state. Load `app/project-data.json` from the page (using
  browser-native JavaScript in the page if needed), render all required fields,
  and avoid exposing a server directory listing as the primary experience.
  Handle a failed or malformed data load with a clear user-facing message.
- **Designer — `app/styles.css`:**
  implement the approved visual system and responsive behavior. The stylesheet
  must include `.dashboard` and `.project-card`, readable typography and
  spacing, rounded cards, appropriate shadows, status badge styling, priority
  treatment, keyboard-visible focus states, and mobile-friendly layout rules.
  Keep styles scoped to the dashboard so they do not affect repository tooling.

The Coder owns the integration markup and data-rendering behavior; the Designer
owns the visual rules. Any changes to shared class names must be agreed by both
agents before editing, to prevent an HTML/CSS handoff mismatch.

### Phase 4 — Add and verify the run configuration

**Owner:** Coder  
**File:** `.vscode/launch.json`

Create strict JSON with a deterministic configuration named **Run Project Pulse
Dashboard**. It must serve from `${workspaceFolder}/app`, open
`index.html`, and use a predictable local port and browser/server command
available in the Codespace. The configuration must launch the dashboard page,
not the parent directory, so learners see Project Pulse immediately.

This is **sequential after the app contract is settled**: the launch target
depends on `app/index.html` existing, although it can be drafted in parallel
with styling if the Coder has already committed to that path.

### Phase 5 — Integrate, review, and hand off

**Owner:** Orchestrator, with Designer and Coder review  
**Files:** all four assigned implementation files

1. Check that the JSON field names, rendered labels, CSS hooks, and launch path
   agree.
2. Review the first viewport for hierarchy and contributor comprehension.
3. Confirm no unassigned files or package dependencies were introduced.
4. Record validation results and any remaining assumptions for the final
   handoff. Do not stage, commit, or push as part of this implementation plan.

## File assignments and responsibilities

| File | Primary owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder | Semantic dashboard shell, inline/native rendering logic, project cards, required field labels, loading/empty/error states, and accessibility attributes. |
| `app/styles.css` | Designer | Visual hierarchy, responsive grid, `.dashboard`, `.project-card`, badges, priority/risk emphasis, spacing, contrast, focus states, and polished card presentation. |
| `app/project-data.json` | Coder | Valid JSON with a top-level `projects` array; each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named `Run Project Pulse Dashboard`, serving `app/` and opening `index.html`. |

### Designer responsibilities

- Translate the brief into a clear information hierarchy and card layout.
- Define accessible status and priority treatments that do not rely on color
  alone.
- Specify responsive behavior for narrow and wide screens, readable spacing,
  focus visibility, and sensible typography.
- Keep the agreed `.dashboard` and `.project-card` hooks stable during review.
- Review the integrated page visually and report any usability or accessibility
  regressions.

### Coder responsibilities

- Implement the semantic, data-driven static page within the assigned files.
- Preserve the exact JSON contract and render every required project field.
- Keep the app dependency-free unless a dependency becomes demonstrably
  necessary; if one is proposed, document why and how it will be installed.
- Create the strict launch configuration with the correct working directory and
  `index.html` target.
- Validate syntax, loading behavior, responsive structure, and launch behavior,
  then report results and remaining risks.

### Orchestrator responsibilities

- Resolve ownership and interface decisions before overlapping edits occur.
- Run the parallel content/design phase, then sequence integration and launch
  verification.
- Reconcile Designer and Coder feedback and verify the final files as one
  working dashboard.

## Dependencies and ordering decisions

1. The brief and existing custom-agent guidance are prerequisites for all work.
2. `app/project-data.json` establishes the data contract used by
   `app/index.html`; data schema decisions must precede final rendering logic.
3. Designer markup hooks and visual decisions must be available before the
   Coder finalizes HTML classes and before the Designer finalizes CSS.
4. `app/index.html` and `app/styles.css` have a mutual interface through class
   names and semantic structure; changes to that interface require coordinated
   review.
5. `.vscode/launch.json` depends on the final app path and must target
   `${workspaceFolder}/app/index.html` (or an equivalent configuration that
   serves `app/` and opens `index.html`).
6. Integration review and launch testing happen after all four files exist.

The data-authoring and design-specification tasks are the main **parallel**
work. HTML/CSS integration, launch configuration verification, and final review
are **sequential** because they depend on shared contracts or the completed
app.

## Edge cases and risks

- Empty `projects` arrays should show a useful empty state rather than a blank
  dashboard.
- Missing or malformed fields should not make the entire page unreadable;
  provide a safe fallback or clear error state.
- A failed `fetch` or local-file/browser restriction should produce actionable
  feedback. The launch configuration should use a local server so JSON loading
  works reliably.
- Status and priority values may vary in capitalization; rendering and styling
  should use stable, predictable class names or normalized values.
- Long project names, owner names, and recent-activity text must wrap without
  breaking the card grid.
- Color must not be the only status/priority signal; retain text labels and
  sufficient contrast.
- The dashboard must remain usable at mobile widths and with keyboard focus.
- Avoid relying on unavailable external fonts, CDN assets, build tools, or
  framework packages.

## Validation expectations

### Static and repository checks

- Confirm all four assigned files exist:
  `app/index.html`, `app/styles.css`, `app/project-data.json`, and
  `.vscode/launch.json`.
- Parse `app/project-data.json` and `.vscode/launch.json` as strict JSON.
- Confirm the data has a top-level `projects` array and that every record has
  `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `app/index.html` references the stylesheet, exposes the dashboard
  and project-card structure, and visibly supports all required fields.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, plus
  responsive and polished card styling such as rounded corners, shadows,
  spacing, and readable status/priority treatments.

### Runtime and visual checks

- Use the **Run Project Pulse Dashboard** launch configuration and verify that
  the browser opens `index.html` from the `app/` directory rather than a
  directory listing.
- Verify that project cards render from `project-data.json`, including status,
  owner, recent activity, and priority.
- Exercise normal, empty, and data-load failure states where practical.
- Check keyboard focus, text readability, contrast, wrapping of long content,
  and responsive layout at narrow and wide viewport sizes.
- Run the repository's existing `scripts/validate-exercise.sh` after the plan
  and later implementation stages; this plan itself should not alter unrelated
  validation or workflow files.

## Open questions and assumptions

- No JavaScript file is listed in the brief, so the default plan uses small
  browser-native logic in `app/index.html`; a separate script should only be
  added if the Orchestrator explicitly expands the file assignment.
- The exact local server command/port is not prescribed. Coder should choose a
  deterministic command available in the devcontainer, document the choice in
  the handoff, and ensure the launch URL opens `index.html`.
- The sample projects and visual palette are illustrative; the Designer and
  Coder may refine them while preserving the required fields, hooks, and
  contributor-first purpose.
