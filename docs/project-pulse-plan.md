# Project Pulse dashboard implementation plan

## Summary

Build a polished, accessible static dashboard that helps Mona's contributors scan project names, owners, status, recent activity, and priority. Keep the app dependency-free and use `app/project-data.json` as the source of project data. The VS Code launch configuration must serve the app directory and open the dashboard page rather than a directory listing.

## Responsibilities and file assignments

| Owner | Files | Responsibility |
| --- | --- | --- |
| Designer | `app/styles.css` | Define the dashboard's visual hierarchy, responsive card layout, readable spacing and typography, status and priority treatments, and accessible contrast/focus states. Coordinate the class and layout contract with Coder. |
| Coder | `app/project-data.json` | Create representative, contributor-friendly project records in a top-level `projects` array. Each record includes `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| Coder | `app/index.html` | Build the page titled `Project Pulse`, link `styles.css`, load `project-data.json`, and render visible project cards with the `project-card` class. Show every required field and handle data-loading errors visibly. |
| Coder | `.vscode/launch.json` | Add strict JSON for the **Run Project Pulse Dashboard** launch configuration. Run `python3 -m http.server 5500` with the app directory as its working directory, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. |
| Orchestrator | No implementation files | Coordinate the phases and file ownership, resolve the HTML/CSS/data contract, ensure handoffs are clear, and validate the integrated runnable dashboard. |
| Planner | `docs/project-pulse-plan.md` | Identify requirements, ownership, sequencing, dependencies, parallel work, and validation expectations before implementation. |

## Ordered implementation steps

1. **Confirm the shared contract.** Orchestrator shares this plan and agrees with Designer and Coder on the dashboard structure, card class names, field labels, and data shape. `projects` is the top-level JSON key; each project has the five required fields.
2. **Work in parallel on independent files.** Designer creates `app/styles.css` using a polished, responsive card-based layout and a `.dashboard` and `.project-card` selector. Coder creates `app/project-data.json` with multiple realistic records. Keep CSS hooks and data field names consistent with the agreed contract.
3. **Implement the page.** After the contract and parallel outputs are available, Coder creates `app/index.html`, links the stylesheet, loads the JSON, and renders each record as a `.project-card`. Include clear loading and visible error states if the data cannot be loaded or parsed.
4. **Add the runnable preview configuration.** Coder creates `.vscode/launch.json` as strict JSON. Configure the launch name, command, app working directory, and server-ready URL specified above.
5. **Integrate and validate.** Orchestrator reviews the four deliverables together, runs the checks below, starts the preview, confirms the dashboard is displayed, and reports any remaining issues.

## Dependencies and parallel work decisions

- The shared field names and CSS/HTML hooks must be agreed before implementation to avoid mismatches.
- Designer's stylesheet and Coder's JSON data can be created in parallel because they have separate file ownership; both must follow the shared contract.
- Coder's HTML depends on the agreed contract and should follow the stylesheet/data handoff so its selectors and rendered fields match them.
- Coder can create `.vscode/launch.json` independently of the HTML and CSS once the required command, working directory, and URL are fixed. Integrate and test it with the app after all files exist.
- Final preview validation must run after all four assigned files are present.

## Validation expectations

- Confirm `app/index.html` has the exact title `Project Pulse`, links `styles.css`, and loads `project-data.json`.
- Confirm the page renders a visible `.project-card` for each project and displays its name, owner, status, recent activity, and priority.
- Confirm loading and data errors are communicated visibly rather than silently producing an empty dashboard.
- Confirm `app/styles.css` includes `.dashboard` and `.project-card`, readable spacing, responsive layout, rounded corners, shadows, and accessible contrast/focus treatments.
- Parse `app/project-data.json` as JSON; confirm it has a top-level `projects` array and every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Parse `.vscode/launch.json` as strict JSON; confirm it defines **Run Project Pulse Dashboard**, runs `python3 -m http.server 5500` from the `app/` directory, and `serverReadyAction` opens `http://localhost:%s/index.html`.
- Run the launch configuration and confirm the browser shows the Project Pulse UI from `index.html`, not the server directory listing. Stop the preview server after checking.

## Risks and assumptions

- The dashboard is a static app with no framework or package-install step; data is loaded from a local JSON file over the preview HTTP server.
- `recentActivity` and `priority` are rendered as supplied data. Keep their wording concise and use clear text labels in addition to color.
- The launch configuration must start the server with `app/` as its working directory; serving the repository root could open a directory listing or the wrong page.
