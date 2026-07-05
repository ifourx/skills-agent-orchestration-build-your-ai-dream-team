# Project Pulse Dashboard: Implementation Plan

This comprehensive implementation plan outlines the structural, design, and environment configurations required to build Mona's **Project Pulse dashboard**. The plan delegates clear, non-overlapping responsibilities to the **Designer** and **Coder** agents to construct a highly polished, responsive, and deterministic static web application.

---

## 1. Summary

The **Project Pulse dashboard** is a lightweight, static frontend application designed to give contributors an instant, clear overview of active team projects. The application consists of three main frontend files (`index.html`, `styles.css`, `project-data.json`) and a local workspace runner configuration (`.vscode/launch.json`). 

### Core Goals:
- **Visual Clarity**: Users can quickly identify active projects, owners, status, recent activity, and risk/priority levels.
- **Polished Presentation**: A high-end card-based layout featuring modern visual affordances (shadows, rounded corners, responsive spacing, and intuitive color coding).
- **Smooth Developer Experience**: Quick, deterministic workspace serving and browser launching with a single click in VS Code.

---

## 2. Agent Responsibilities

To ensure maximum focus and prevent merge conflicts, responsibilities are cleanly divided between the **Designer** and the **Coder**:

### 🎨 Designer Responsibilities
- **UI/UX Direction**: Establishes information architecture, visual hierarchy, readable line heights, and padding.
- **Visual Polish**: Designs beautiful card modules, rounded corners (`border-radius`), elevation shadows (`box-shadow`), clear status badges, and distinctive priority levels.
- **Responsive Layout**: Directs CSS Grid or Flexbox logic to adapt seamlessly across desktop, tablet, and mobile devices.
- **Deterministic CSS Hooks**: Manages and enforces target styling selectors, specifically `.dashboard` and `.project-card`.

### 💻 Coder Responsibilities
- **Data Modeling**: Structure and maintain valid JSON in `app/project-data.json` containing the top-level `"projects"` key.
- **Application Logic**: Implement explicit JavaScript logic in `app/index.html` to dynamically fetch `project-data.json`, iterate through the data, and render semantic HTML project cards.
- **Environment Automation**: Author a comment-free, strict JSON file in `.vscode/launch.json` configured to launch a python-based static web server from the `app` directory and open the dashboard frontend.
- **Robustness**: Build empty-state, parsing error, and loading handlers in the script.

---

## 3. Ordered Implementation Steps & Phase Divisions

The implementation is structured into three consecutive phases to optimize parallelism and maintain a clean linear dependency flow.

```
┌────────────────────────────────────────────────────────┐
│ Phase 1: Foundation (Data Modeling & Workspace Setup)  │
│ - Create app/project-data.json [Coder]                 │ (Parallel Work)
│ - Create .vscode/launch.json  [Coder]                 │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ Phase 2: Core Skeleton (Semantic HTML Markup)          │
│ - Create app/index.html       [Coder & Designer]       │ (Sequential/Collaborative)
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ Phase 3: Visual Polish (Polished CSS Styling)          │
│ - Create app/styles.css       [Designer & Coder]       │ (Sequential/Collaborative)
└────────────────────────────────────────────────────────┘
```

### Phase 1: Foundation (Data Modeling & Workspace Setup)

#### Step 1.1: Author Project Data Structure
- **File**: `app/project-data.json`
- **Assigned Agent**: **Coder**
- **Details**: Create a fully compliant JSON file with a top-level `"projects"` key pointing to an array of project objects. Each project object must contain the following fields: `name`, `owner`, `status`, `recentActivity`, and `priority`.
- **Validation**: Ensure zero JSON syntax errors (validated via `python3 -m json.tool`).

#### Step 1.2: Configure VS Code Workspace Runner
- **File**: `.vscode/launch.json`
- **Assigned Agent**: **Coder**
- **Details**: Create a strict JSON configuration with no comments. Set up a launch configuration named `"Run Project Pulse Dashboard"`, setting `cwd` to `${workspaceFolder}/app`, executing `python3 -m http.server 5500`, and adding a `serverReadyAction` to open `http://localhost:%s/index.html`. This ensures the web page opens directly instead of a directory listing.
- **Validation**: Ensure zero JSON syntax errors (validated via `python3 -m json.tool`).

---

### Phase 2: Core Skeleton (HTML Markup)

#### Step 2.1: Implement Semantic Markup and Fetch Logic
- **File**: `app/index.html`
- **Assigned Agents**: **Coder** (logic and structure) & **Designer** (accessibility and hierarchy)
- **Details**:
  - Create the index document with the exact `<title>Project Pulse</title>` in the `<head>` block.
  - Reference `styles.css` using a `<link>` tag.
  - Reference or link to `project-data.json` via a client-side `fetch` command.
  - Implement a container element with the class name `dashboard` to hold the project elements.
  - Create standard JavaScript logic to parse `project-data.json` and dynamically generate individual card elements containing classes named `project-card`.
  - Ensure fields for `status`, `recentActivity`, and `priority` are dynamically mapped to visible elements on the card.
- **Validation**: Ensure semantic validity and confirm correct loading/fetching of data without syntax issues.

---

### Phase 3: Visual Polish (CSS Styling)

#### Step 3.1: Design and Implement Polished Styling
- **File**: `app/styles.css`
- **Assigned Agents**: **Designer** (styling properties and layouts) & **Coder** (sanity checks and refactoring)
- **Details**:
  - Implement the `.dashboard` selector to style the page grid/layout.
  - Implement the `.project-card` selector with visual card styling, incorporating explicit rules for `border-radius` (for modern rounded corners) and `box-shadow` (for soft, realistic elevation).
  - Create intuitive status badges (e.g., green for Active, orange for Paused, blue for Completed) and visual indicators representing High, Medium, and Low priorities.
  - Define fluid, responsive grid layouts so cards resize and reflow elegantly depending on viewport width.
- **Validation**: Inspect selectors, verify grid responsiveness, and check visual contrast scores.

---

## 4. Dependencies & Parallel Work Decisions

### Work That Can Run in Parallel
During **Phase 1**, the creation of `app/project-data.json` and `.vscode/launch.json` can run in **parallel** because they do not share any technical file dependencies or schema overlap. The Coder agent can finalize both configurations simultaneously.

### Work That Must Run Sequentially
1. **Phase 1 ➔ Phase 2**: HTML markup cannot be accurately finalized until the structured fields inside `project-data.json` are established.
2. **Phase 2 ➔ Phase 3**: CSS development depends entirely on the DOM structure. CSS class selectors like `.dashboard` and `.project-card` must align with the exact markup structure created in `index.html`.

---

## 5. Edge Cases to Handle

To deliver a production-ready application, the Coder and Designer must proactively handle the following edge cases:

| Edge Case | Risk | Mitigation Strategy |
| :--- | :--- | :--- |
| **JSON Parse Failures** | Dynamic fetch breaks, displaying a blank screen. | **Coder**: Implement a robust `catch` block on the fetch promise that displays a friendly error message on the page (e.g. "Unable to load project data. Please verify data format."). |
| **Empty Projects List** | Dashboard looks broken or abandoned. | **Coder**: Check if `projects.length === 0` and display an elegant "No active projects" illustration or placeholder message. |
| **Extremely Long Text** | Long titles, names, or activities cause cards to break bounds. | **Designer**: Implement `overflow: hidden; text-overflow: ellipsis;` or `word-wrap: break-word` on text containers to keep cards uniform. |
| **Varying Status/Priority Labels**| Missing badge designs for unexpected statuses. | **Designer**: Define a fallback grayscale or neutral color treatment for undefined status types or priorities. |
| **Port Conflicts (5500 in use)** | Server fails to boot or runs on an unexpected fallback port. | **Coder**: Utilize `serverReadyAction` with regex port capturing (e.g., `http://localhost:%s/index.html`) to dynamically map to whatever port Python binds to if 5500 is blocked. |

---

## 6. Validation Expectations

The Orchestrator or learner will verify the success of each phase using the following strict tests:

### 1. Static Configuration Verification (Syntax & Keyphrase checks)
Run the script `./scripts/validate-exercise.sh` locally to execute automated validation checks:
- Confirm that `.vscode/launch.json` and `app/project-data.json` parse as strict JSON without comments.
- Verify `app/index.html` has the `<title>Project Pulse</title>` and explicitly links `styles.css` and `project-data.json`.
- Verify `app/styles.css` has selectors for `.dashboard` and `.project-card` that use both `border-radius` and `box-shadow`.
- Verify `app/project-data.json` utilizes the top-level `"projects"` key and maps the fields `name`, `owner`, `status`, `recentActivity`, and `priority`.

### 2. Runtime Execution Check
1. Open the VS Code **Run and Debug** view in the sidebar.
2. Select the configuration named **"Run Project Pulse Dashboard"**.
3. Press the green Play arrow.
4. Verify that:
   - The integrated terminal boots up a server serving from the `/app` directory.
   - Your system's default browser automatically opens directly to `http://localhost:5500/index.html` (or mapped dynamic port).
   - The browser presents a highly polished, styled dashboard containing the project cards rather than a raw index directory tree.

---

## 7. Open Questions

1. **Local vs. External Icons**: Do we want to include custom icons (like owner avatars, priority alert markers)? If so, we should plan to fetch them from a standard, stable public CDN or design SVG symbols inline.
2. **Dynamic Project Count**: Is there a maximum project limit before pagination or a vertical scroll pattern should be styled by the Designer? (Recommended: standard CSS grid auto-fill pattern).
