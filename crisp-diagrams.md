# Crisp Diagrams for Confluence: Setup & Usage

Mermaid, PlantUML, D2, DBML and Excalidraw diagrams on Confluence Cloud pages, rendered as crisp SVG in your browser, with click-to-zoom, live preview, dark mode and images in PDF and Word exports. Free. Runs entirely on Atlassian Forge inside your own site: no render server, no external service, nothing leaves your instance.

> App key: `dev.katabarwalabs.diagrams-as-code`.

## What you get

| Macro | Language | Notes |
|-------|----------|-------|
| Mermaid Diagram | Mermaid 12 | Flowchart, sequence, class, state, ER, Gantt, mindmap, timeline, pie, git graph and every other Mermaid type |
| PlantUML Diagram | PlantUML 1.2026 | Runs in the browser (no Graphviz install, no plantuml.com). C4, Kubernetes, Azure, GCP, Elastic and ArchiMate standard libraries are bundled |
| D2 Diagram | D2 | Dagre or ELK layout, sketch mode, themes |
| DBML Diagram | DBML | A database definition becomes an entity relationship diagram |
| Excalidraw Drawing | Excalidraw | Hand-drawn style sketches, rendered as SVG with working links |

Every macro renders the same way: vector SVG that fills the page width, a hover toolbar with **Full screen**, **SVG** and **PNG**, and a full-screen view with scroll-to-zoom and drag-to-pan. Diagrams follow Confluence light and dark themes unless the diagram pins its own theme.

## Requirements

- A Confluence Cloud site. Anyone who can edit a page can add a diagram; a Confluence administrator installs the app.
- No configuration, no account, no API key.

## Install

1. From the Atlassian Marketplace listing, select **Get it now** and choose your Confluence site.
2. Approve the scopes shown below.
3. Open any page in the editor, type `/` and choose **Mermaid Diagram**, **PlantUML Diagram**, **D2 Diagram**, **DBML Diagram** or **Excalidraw Drawing**.

## Permissions requested

| Scope | Why |
|-------|-----|
| `storage:app` | Drawings larger than about 200 KB (Excalidraw with embedded images) are stored in the app's Forge storage inside your tenant instead of in the page body. |
| `read:content-details:confluence`, `write:attachment:confluence` | When you save a diagram, the editor attaches a PNG image of it to the page as you (one file per diagram, re-versioned on each change) so PDF and Word exports show the picture. |
| `read:attachment:confluence` | The export function looks that PNG up when Confluence builds a PDF or Word export. |

Content permissions: the D2 engine is WebAssembly running in a Web Worker created from a blob URL (`unsafe-eval`, `blob:`), and every engine styles its SVG inline (`unsafe-inline` styles). The app declares no external domains, so it is eligible for the **Runs on Atlassian** marker.

## Usage

**Write a diagram.** The editor is a split view: source on the left, live preview on the right, resizable. Pick a starter template from **Start from**, or type. Errors show the line number. Press **Save**; **Save anyway** keeps a diagram that currently has an error so you can come back to it.

**Switch language.** Every macro has a **Language** selector, so a Mermaid macro can become a PlantUML one without re-adding it. Text you typed in each language is kept while the editor is open.

**Paste from anywhere.** Paste a fenced code block (` ```mermaid `, ` ```plantuml `, ` ```d2 `, ` ```dbml `) from an AI chat, a README or your notes: the fence is removed and the language switches to match. Paste a **mermaid.live**, **mermaid.ink**, **plantuml.com** or **Kroki** link and the diagram is imported. Pasting one of those links directly into the page body creates the macro automatically.

**Read a big diagram.** Click the diagram, or hover and choose **Full screen**. Scroll to zoom, drag to pan, double-click to fit. **SVG** and **PNG** download the diagram; PNG is rendered at up to 2x for print.

**Draw.** The Excalidraw Drawing macro opens the Excalidraw canvas. Links you add to shapes stay clickable in the rendered diagram.

**Export to PDF or Word.** Save a diagram once after installing the app and exports include the rendered image. A diagram that has never been saved with this app exports as its source code plus a note, never as an error box.

## How to test it (verify functionality)

1. Create a page, type `/mermaid`, choose **Mermaid Diagram**, pick the **Sequence** template and **Save**. The page shows an SVG sequence diagram; hover it and choose **Full screen**, then scroll to zoom.
2. Edit the same macro, change **Language** to **PlantUML**, pick the **C4 context** template and **Save**. It renders without any external request (watch the browser network panel: only your Confluence domain).
3. Add an **Excalidraw Drawing**, draw a few shapes, add a link to one, **Save**. Click the link in the rendered drawing: Confluence asks before opening it.
4. Paste `https://mermaid.live/edit#pako:...` (any diagram from mermaid.live) into the page body. A Mermaid macro appears with the diagram imported.
5. Switch Confluence to dark mode (profile menu, **Theme**). The diagrams re-render in dark colours.
6. Export the page to PDF (**...** menu, **Export**, **PDF**). The PDF contains the rendered diagrams as images.

## Where your diagrams live

The diagram source (or the drawing) is stored in the macro on the page, so it is versioned, copied and exported with the page. Drawings over about 200 KB are stored in Forge storage in your tenant, keyed by a random diagram id kept in the macro. The PNG used for exports is an ordinary page attachment named `diagram-<id>.png`. Uninstalling the app leaves your pages and attachments in place; the macros show as unknown until it is reinstalled.

## Notes on scope (honesty first)

- PlantUML sprite packs that are very large (AWS, IBM, Material, tupadr3, logos, office, osa) are not bundled; a diagram that includes one renders with a warning and without those sprites.
- Each text diagram is limited to 50,000 characters (Mermaid's own limit, applied to every language).
- A bare fenced code block pasted into the page body does not become a macro (Confluence only auto-converts links); paste it into the macro editor instead.
- Confluence only. A Jira issue panel is on the roadmap.

## Support

support@llmgraph.ai. Tell us what does not fit and we will build it.
