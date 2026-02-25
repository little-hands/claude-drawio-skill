---
name: draw-io
description: Create, read, edit, and open draw.io (.drawio) files. Supports architecture diagrams, ER diagrams, flowcharts, domain model diagrams, and more. Use when requests include "create a diagram", "make a flowchart", "draw an architecture diagram", "create an ER diagram", "open a .drawio file", "what's in this diagram?", etc. Do NOT use for Mermaid or PlantUML generation.
---

# draw.io Diagram Skill

Create, read, and edit diagrams in draw.io format (.drawio).

## Supported Diagram Types (Examples)

- Architecture diagrams
- Domain model diagrams
- Object diagrams
- ER diagrams
- Flowcharts
- Wireframes
- Sequence diagrams
- Class diagrams

Any other diagram type that can be expressed in draw.io is also supported.

## Workflow

### 1. Read (Reference during development)

Read domain model or architecture diagrams and use them as development context.

**Trigger examples:** "Check the domain model", "Read the design diagram", "Reference the architecture", "What's in this diagram?"

**Steps:**

1. **Load reference files**: Read the following
   - [references/xml-reference.md](references/xml-reference.md) — mxCell patterns, parsing examples
2. Use `Glob` to search for `.drawio` files
3. Use `Read` to load the XML
4. Extract information from `mxCell` attributes:
   - `value` attribute: labels (entity names, method names, etc.)
   - `parent` attribute: parent-child relationships (group membership)
   - `source`/`target` attributes: edge connections
   - `fillColor` in `style` attribute: element classification (color-based)
5. Output a structured summary

### 2. Create / Edit

Create new diagrams or update existing `.drawio` files.

**Trigger examples:** "Create a diagram", "Make a flowchart", "Draw an architecture diagram", "Create an ER diagram", "Update the diagram", "Add an entity", "Change a relationship"

**Steps:**

1. **Clarify requirements**: Confirm the diagram type and elements to include with the user
2. **Load reference files**: **Always** load the following before starting work
   - [references/xml-reference.md](references/xml-reference.md) — mxCell patterns, styles, color palette
   - [references/layout-guide.md](references/layout-guide.md) — layout principles, coordinate calculations
3. **Check existing files**: Use `Glob` to search for the target `.drawio` file
   - **File exists** → Use `Read` to get the XML and check existing IDs
   - **No file** → Proceed as a new creation
4. **List elements**: Enumerate boxes, relationships, and groups
5. **Generate / Edit XML**:
   - **New creation**: Combine the base structure template with mxCells, and save as `.drawio` using `Write`
   - **Editing existing**: Assign new IDs that don't conflict with existing ones, and update the XML using `Edit`
6. **Show path**: Display the file path to the user (do not open automatically)

### 3. Open in Desktop App

**Trigger examples:** "Open this file", "Open the .drawio file", "Open the diagram"

**Steps:**

1. Identify the file path (if not specified, use `Glob` to search for `.drawio` files)
2. Run `open -a "draw.io" <file-path>` using `Bash`

Prerequisite: draw.io must be installed via `brew install --cask drawio`

## XML Base Structure

**Important:** Always specify `darkMode="0"`. This disables Adaptive Colors, preventing color inversion when opened in a dark-mode editor, and maintains light-mode colors during SVG/PNG export.

```xml
<mxfile host="app.diagrams.net" agent="Claude" version="21.0.0">
  <diagram name="Diagram Name" id="unique-id">
    <mxGraphModel dx="1200" dy="800" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1169" pageHeight="827" math="0" shadow="0" darkMode="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Add elements here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

For multiple pages, add multiple `<diagram>` elements. Specify `darkMode="0"` in each `<mxGraphModel>`.

## Important Constraints

- **Always specify `darkMode="0"`** — omitting it causes color inversion in dark mode
- **Keep IDs unique** — manage with prefixes like `box1`, `edge1`
- **Use `&#xa;` for line breaks** — for text line breaks within XML
- **Specify parent-child relationships with the `parent` attribute** — coordinates of child elements within a group are relative to the parent
- **`vertex="1"` for shapes, `edge="1"` for edges**
- **Do not open after editing** — only display the file path; open only when the user explicitly asks
- **Do not include `--` in XML comments** — `<!-- Order ---> Customer -->` causes an XML error; write as `<!-- Order to Customer -->`

## Troubleshooting

### XML Parse Error

**Cause:** `--` is included in a comment
**Fix:** Avoid `--` as in `<!-- Order to Customer -->`. See the beginning of [references/xml-reference.md](references/xml-reference.md) for details

### Colors Inverted in Dark Mode

**Cause:** Missing `darkMode="0"` specification
**Fix:** Add `darkMode="0"` to `<mxGraphModel>`. For multiple pages, specify it on all pages

### Shapes Overlapping / Edges Crossing

**Cause:** Layout calculation issues
**Fix:** Check layout principles in [references/layout-guide.md](references/layout-guide.md). Distribute edge connection points using `exitX`/`exitY`/`entryX`/`entryY`

### Rendering Broken Due to Duplicate IDs

**Cause:** Multiple mxCells with the same ID exist
**Fix:** Check existing IDs before editing and assign unique IDs

### draw.io App Won't Open

**Cause:** draw.io desktop app is not installed
**Fix:** Install with `brew install --cask drawio`

## Reference Files

- **XML Patterns & Styles** → [references/xml-reference.md](references/xml-reference.md)
  Contents: mxCell patterns (rectangle, text, edge, group, DB, stack layout), style attributes, color palette, edge styles, custom properties, connection points, parsing examples for reading
  **Load for reading, creating, and editing**

- **Layout Guide** → [references/layout-guide.md](references/layout-guide.md)
  Contents: Layout principles, coordinate calculations, placement patterns, boundary coordinate calculations
  **Load for creating and editing**
