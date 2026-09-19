# Safety-Critical-Task-Analysis
A browser-based workbench for human factors safety critical task analysis (SCTA) in oil, gas and energy facilities.
# SCTA Workbench

A browser-based workbench for **human factors safety critical task analysis (SCTA)** in oil, gas and energy facilities.

One HTML file. No install, no server, no account, no data leaving your machine.

👉 **[Open the workbench](https://YOUR-USERNAME.github.io/scta-workbench/)**

---

## Why this exists

Most major accidents involve a task that someone carried out, checked, decided on or communicated. Technical failures get analysed in depth; the tasks around them usually do not. SCTA closes that gap — but in practice it is often done in a spreadsheet, or not at all, because the method is spread across guidance documents and there is no simple tool to work through it.

This workbench walks a team through the seven steps in order, keeps the worksheet readable, and produces an action register you can hand to the integrity or operations team.

It is aimed at process safety, operations and integrity engineers who already understand risk assessment but are not human factors specialists.

---

## What it does

| Step | In the workbench |
|---|---|
| **1. Major hazards** | Register the hazards the study protects against, with their source (safety case, HAZOP, bow tie) |
| **2. Safety critical tasks** | List tasks, link them to hazards, and screen each one for priority |
| **3. Task evidence** | Record how each task was studied — procedure read, operator interviewed, task observed, walk-through done |
| **4. Task breakdown** | Break the task into numbered steps and sub-steps |
| **5. Failures and influencing factors** | Apply failure prompts to each step; record consequences, existing controls, and the conditions that make failure more likely |
| **6. Safety measures** | Add measures against a five-level hierarchy, with a check that the measure suits the type of failure |
| **7. Review** | Benchmark the study against quality indicators, with gaps flagged automatically |

Plus a consolidated **action register**, CSV exports, and a print stylesheet that turns the whole study into a report.

---

## Quick start

**Use it online** — open the GitHub Pages link above.

**Use it offline** — download `index.html` and open it in any modern browser. It works with no internet connection (the typeface falls back to your system font).

**Try the worked example** — click **Example** in the title bar. It loads a study of sand erosion on a gas-lifted well flowline: two tasks, a task breakdown, three failures and a set of measures spanning the hierarchy. Use it to see the shape of a finished study before starting your own.

---

## How to run a study

1. **Set up.** Fill in the study reference, site, facility and team. Include at least one person who actually does the task being analysed.
2. **List the hazards.** Take them from documents that already exist. If a task cannot be traced to one of these hazards, leave it out.
3. **Identify the tasks.** Look for tasks that open the system up, that maintain a protective system and could leave it disabled, or that create the condition a protective system guards against. Include checking, handover and decision tasks — they are the ones most often missed.
4. **Gather evidence.** Read the procedure, then watch the task and talk to the people who do it. The difference between the two is usually where the risk sits.
5. **Break the task down.** Main steps in the order they happen; sub-steps only where a step is worth opening up. Two levels is normally enough.
6. **Analyse failures.** Take each step and apply the prompts. Record the influencing factors honestly, including the ones that are uncomfortable — time pressure, unclear responsibility, procedures nobody follows.
7. **Choose measures.** Work down the hierarchy. If everything you propose is a procedure or a training course, the analysis is not finished.
8. **Review and issue.** Work through the quality check, clear the automatic gaps, then export the action register into your normal tracking system.

---

## How the scoring works

Everything is transparent and can be changed in the source.

**Task priority** — three judgements of 1 to 4, added together:

- how bad it is if the task fails
- how much the outcome relies on the person rather than on engineered systems
- how often the task is carried out

| Total | Priority |
|---|---|
| 11–12 | Critical |
| 9–10 | High |
| 6–8 | Medium |
| 3–5 | Low |

**Failure risk** — severity (1–5) × likelihood (1–5), banded Low / Medium / High / Very high.

**Hierarchy of measures** — measures are tagged 1 to 5:

1. Remove the hazard
2. Remove the human contribution (automate)
3. Prevent or limit the consequence with an engineered barrier
4. Assure performance mechanically or electrically, for example an interlock
5. Improve the conditions the person works under

The Step 6 screen counts measures at each level, so a study that leans entirely on level 5 is visible at a glance.

**Failure types** — slip, lapse, mistake and violation. The workbench flags where a measure is a poor match for the failure type, for example relying on a procedure to prevent a slip.

---

## Saving, and using a cloud folder

There is no server behind this tool, so your study is never uploaded anywhere by the application itself. You choose where it is kept.

**Link it to a file in your cloud folder (recommended).** On the Study setup screen, choose **Link to a file…** and pick a location inside your synced OneDrive, SharePoint or Google Drive folder. From then on **Save file** writes straight into that file, and your sync client uploads it like any other document. Tick **Save to the file automatically** and it writes every twenty seconds while you work.

This is the simplest route and usually the right one for company data:

- the file inherits your organisation's access control, version history, retention and backup
- no account, token or IT registration is needed
- colleagues open it from the same shared folder and continue the study
- nothing passes through a third party

It uses the browser's File System Access API, so it needs **Chrome or Edge** over HTTPS. Firefox and Safari fall back to download-and-reopen, which still works — the study just isn't linked to a file.

**Without a linked file**, the study is held in your browser's local storage. It survives a refresh on the same machine and browser, but clearing browsing data clears it, so save a file at the end of each session.

**Other outputs:** **Print / PDF** produces the complete study as a report with the title block on every page. CSV exports are available for the worksheet and the action register.

### Connecting to OneDrive or SharePoint directly

If you want the tool to sign users in and write to SharePoint without a synced folder, add Microsoft Graph:

1. Ask IT to register a single-page application in Entra ID (Azure AD) with your Pages URL as the redirect URI.
2. Grant the delegated permission `Files.ReadWrite` (or `Sites.ReadWrite.All` for a specific SharePoint library).
3. Add MSAL.js to the page, sign the user in with the authorisation code flow with PKCE — a static site holds no client secret — and `PUT` the study JSON to the Drive item endpoint.

This gives proper corporate sign-in and audit, at the cost of an IT registration and one external script. For most teams the synced-folder route above achieves the same thing with none of that.

### What to avoid

Storing study data in a third-party backend such as Firebase, Supabase or a GitHub repository puts platform names, well references and hazard details outside your organisation's control. Check your information classification policy before considering it.

---

## Deploying your own copy

1. Create a repository, for example `scta-workbench`.
2. Add `index.html` (and this README) and push.
3. In **Settings → Pages**, set the source to the `main` branch, root folder.
4. Your copy appears at `https://YOUR-USERNAME.github.io/scta-workbench/` within a minute or two.
5. Update the link at the top of this README.

For a company deployment, the same file can be dropped onto an intranet share or SharePoint document library and opened directly.

---

## Customising it for your organisation

The vocabularies sit at the top of the script block in `index.html` and are plain arrays:

| Constant | What it controls |
|---|---|
| `TASK_CATS` | Task categories in the Step 2 drop-down |
| `GUIDEWORDS` | Failure prompts, grouped by type |
| `PIFS` | Performance influencing factors, grouped by category |
| `HIER` | The five levels of the measure hierarchy |
| `SEV_OPTS`, `REL_OPTS`, `FRQ_OPTS` | Screening scales in Step 2 |
| `QCHECKS` | The quality checklist in Step 7 |

Edit the text to match your own standards, matrices and terminology. Nothing else needs to change.

---

## Browser support

Any current version of Chrome, Edge, Firefox or Safari, on desktop or tablet. It uses standard HTML dialogs and local storage, both widely supported. Internet Explorer is not supported.

---

## Limitations

- This is a structured worksheet, not a quantified human reliability assessment. It will not produce human error probabilities.
- It supports the method; it does not replace competent facilitation. The quality of a study depends on who is in the room, especially whether the people who do the task took part.
- Risk bands and priority thresholds are defaults. Align them with your corporate risk matrix before using the output in a formal assessment.
- The tool does not check that your analysis is correct — only that it is complete against a set of structural checks.

Nothing in this tool constitutes engineering or regulatory advice. Findings should be reviewed and approved through your own management of change and risk assessment process.

---

## Method and further reading

The structure follows established industry guidance on human factors task analysis. The workbench does not reproduce any of it; the vocabularies are written in plain language for this tool. Practitioners should read the source material:

- Energy Institute, *Guidance on human factors safety critical task analysis*, 1st edition, 2011
- HSE, *Core Topic 3: Identifying human failures* (human factors inspectors' toolkit)
- HSE, *Reducing error and influencing behaviour* (HSG48)
- Energy Institute human factors briefing notes, including task analysis and safety critical task identification

---

## Contributing

Issues and pull requests are welcome. Useful contributions include additional guideword and influencing factor sets for specific sectors, translations, and improvements to the printed report layout.

Please keep the tool a single dependency-free file — that is what makes it usable on a platform with no internet access and no software installation rights.

---

## Licence

MIT. See `LICENSE`.

---

Built for process safety practitioners who would rather run the analysis than fight the tool.
