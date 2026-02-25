# drawio Skill - Test Cases

## 1. Create / Edit Workflow

> **Common prerequisite**: For all create/edit cases, verify that both reference files (xml-reference.md, layout-guide.md) are loaded **before** starting work.

### 1-1. New Domain Model Diagram (Multiple Aggregates, Composition, Cross-Aggregate References)

- **Input**: "Create a domain model diagram with an Order aggregate (Order → OrderItem), a Customer aggregate (Customer → ShippingAddress), and a Product aggregate (Product). Group by aggregate, and show cross-aggregate references: Order → Customer and OrderItem → Product."
- **Verification points**:
  - Are both reference files (xml-reference.md, layout-guide.md) loaded before starting?
  - Does a requirements clarification exchange occur?
  - Is `Glob` used to search for existing `.drawio` files?
  - If no existing file, is `Write` used to create a new one?
  - Does the generated XML conform to the base structure template?
  - Is `darkMode="0"` specified?
  - **Grouping**: Are aggregates grouped using swimlanes? Does each child element's `parent` point to the group ID?
  - **Composition**: Are Order→OrderItem and Customer→ShippingAddress expressed with `diamondThin` + `startFill=1`? Is ◆ placed on the parent side (Order, Customer)?
  - **Cross-aggregate references**: Are Order→Customer and OrderItem→Product expressed with dashed lines (`dashed=1`)?
  - **Layout**: Are parent-child relationships arranged vertically and cross-aggregate references horizontally?
  - **Edge distribution**: Do references from Order to Customer and from OrderItem to Product avoid crossing each other?

### 1-2. New ER Diagram (Stack Layout)

- **Input**: "Create an ER diagram with 3 tables: User, Order, and Product."
- **Verification points**:
  - Are both reference files (xml-reference.md, layout-guide.md) loaded before starting?
  - Is Stack Layout (UML Class style) used?
  - Header height = `startSize` (30px)?
  - Each row = 30px?
  - Is the total height correctly calculated as startSize + (rows × 30)?
  - Are inter-table relationships expressed with appropriate edge styles?

### 1-3. Adding an Entity to an Existing Diagram

- **Prerequisite**: The `.drawio` file from 1-1 exists
- **Input**: "Add a Product entity to the domain model diagram."
- **Verification points**:
  - Are both reference files (xml-reference.md, layout-guide.md) loaded before starting?
  - Is the existing file found via `Glob`?
  - Is the existing XML retrieved with `Read` and existing IDs checked?
  - Do the new IDs avoid duplicating existing ones?
  - Is `Edit` used to partially update the XML (not overwrite with `Write`)?
  - Does the added element's layout avoid overlapping existing elements?

### 1-4. Adding an Edge to an Existing Diagram

- **Prerequisite**: Product entity added in 1-3
- **Input**: "Add a relationship between Order and Product."
- **Verification points**:
  - Are both reference files (xml-reference.md, layout-guide.md) loaded before starting?
  - Are source/target specified correctly?
  - Is the arrow style appropriate (for composition, is `diamondThin` on the parent side)?
  - Are connection points specified with `exitX`/`exitY`/`entryX`/`entryY`?

### 1-5. Multi-Page Diagram Creation

- **Input**: "Create a 2-page architecture diagram with an overview page and a detail page."
- **Verification points**:
  - Are both reference files (xml-reference.md, layout-guide.md) loaded before starting?
  - Are two `<diagram>` tags generated?
  - Does each `<mxGraphModel>` have `darkMode="0"`?
  - Does each page have an appropriate `name` and unique `id`?

### 1-6. File Not Opened Automatically After Editing

- **Prerequisite**: Editing from 1-3 or 1-4 completed
- **Verification points**:
  - Is `open -a "draw.io"` NOT executed after editing?
  - Does the response end with only displaying the file path?

## 2. Read Workflow

### 2-1. Reading an Existing Domain Model Diagram

- **Prerequisite**: A `.drawio` file exists
- **Input**: "Check the domain model."
- **Verification points**:
  - **Is xml-reference.md loaded before Glob?** (step 1 → step 2 order)
  - Are `Glob` (search for `.drawio`) and `Read` (load XML) executed in order?
  - Are entity names extracted from the `value` attribute?
  - Are relationships extracted from `source`/`target`?
  - Is the output a structured summary?

### 2-2. Reading a Complex Diagram

- **Prerequisite**: A `.drawio` file with many entities, edges, and groups (e.g., multi-page architecture diagram from 1-5, or a diagram with swimlanes)
- **Input**: "Check the architecture diagram and tell me the dependencies."
- **Verification points**:
  - Is the layer structure (swimlane groups) correctly read?
  - Is the element classification determined from `fillColor`?
  - Are group parent-child relationships reconstructed from the `parent` attribute?

### 2-3. Reading a Diagram with Custom Properties

- **Prerequisite**: A `.drawio` file with custom properties set via `<object>` tags. Prepare a file with XML like:
  ```xml
  <object label="DB Server" zone="3" type="database" id="db1">
    <mxCell style="shape=cylinder3;..." vertex="1" parent="1">
      <mxGeometry x="100" y="100" width="60" height="80" as="geometry" />
    </mxCell>
  </object>
  ```
- **Input**: "What's in this diagram?"
- **Verification points**:
  - Are `<object>` tag attributes (zone, type, etc.) extracted?
  - Is the custom property information included in the summary?

## 3. Open Workflow

### 3-1. Open in Desktop App

- **Prerequisite**: draw.io desktop app is installed
- **Input**: "Open this file." (specifying a `.drawio` file)
- **Verification points**:
  - Is `open -a "draw.io" <file-path>` executed?
  - Does the file display in the draw.io app?

### 3-2. draw.io Not Installed

- **Prerequisite**: draw.io desktop app is not installed (temporarily uninstall with `brew uninstall --cask drawio` or use an environment without it)
- **Input**: "Open the .drawio file."
- **Verification points**:
  - Is an appropriate error message displayed?
  - Is an installation guide (`brew install --cask drawio`) provided?

## 4. Skill Trigger Boundaries

### 4-1. No Trigger for Mermaid

- **Input**: "Write a flowchart in Mermaid."
- **Verification points**:
  - drawio skill does **not** trigger
  - Is output in Mermaid syntax or handled appropriately?

### 4-2. No Trigger for PlantUML

- **Input**: "Write a class diagram in PlantUML."
- **Verification points**:
  - drawio skill does **not** trigger

### 4-3. Handling Diagram Types Not in the List

- **Input**: "Create a Gantt chart in draw.io."
- **Verification points**:
  - Not in the supported diagram list, but per "other types are also supported", is it handled?
  - Is a valid diagram generated in draw.io format?

### 4-4. Trigger for Object Diagrams

- **Input**: "Create an object diagram."
- **Verification points**:
  - Since "object diagram" is in the description, does the drawio skill trigger?
  - Is an appropriate diagram generated as a concrete example of a domain model?

## 5. Reference File Compliance (xml-reference.md / layout-guide.md)

### 5-1. Color Palette Compliance

- **Input**: "Create a 4-layer architecture diagram with Presentation, Use Case, Domain, and Infrastructure layers."
- **Verification points**:
  - Are the fillColor / strokeColor values from the xml-reference.md color palette (7 colors: blue, green, yellow, orange, purple, red, gray)?
  - Is the color usage meaningful (e.g., domain layer = blue, infrastructure = green)?

### 5-2. Edge Label Generation

- **Input**: "Add a labeled relationship '1 to many' between Order and Customer."
- **Verification points**:
  - Is an mxCell for the edge label generated with `edgeLabel` style?
  - Is `parent` set to the target edge's ID?
  - Is `connectable="0"` set?

### 5-3. Database Shape Generation

- **Input**: "Add a DB server in cylinder shape to the architecture diagram."
- **Verification points**:
  - Is `shape=cylinder3` style used?
  - Are fillColor/strokeColor set to infrastructure green (#d5e8d4 / #82b366)?

### 5-4. Element Enumeration Step

- **Input**: "Create a domain model diagram for an order management system. Include Order, Customer, Product, and Delivery."
- **Verification points**:
  - Is there a step to enumerate boxes, relationships, and groups before generating XML (SKILL.md step 4)?

## 6. XML Constraints & Edge Cases

> **Cross-cutting check**: For all `.drawio` files generated in sections 1 and 5, verify that the following constraints are met.

### 6-1. No `--` in XML Comments

- **Input**: Create a diagram containing entity names "Order" and "Customer"
- **Verification points**:
  - Is the NG pattern `<!-- Order ---> Customer -->` not generated?
  - Is the `--` avoided, as in `<!-- Order to Customer -->`?

### 6-2. `darkMode="0"` Applied

- **Input**: Check the XML of diagrams created in 1-1 through 1-5
- **Verification points**:
  - Is `darkMode="0"` set on all `<mxGraphModel>` elements?
- **Note**: The draw.io desktop app may remove the `darkMode="0"` attribute when saving. Verify in the XML immediately after `Write`/`Edit`, not in XML saved by the app.

### 6-3. ID Uniqueness

- **Input**: Create a diagram with many elements, or add entities
- **Verification points**:
  - Are prefixed IDs like `box1`, `edge1` free of duplicates?
  - Do IDs not conflict with existing ones during editing?

### 6-4. Line Break Characters

- **Input**: "Create a diagram with entities labeled in both languages (e.g., Order / Order)"
- **Verification points**:
  - Are line breaks within labels expressed as `&#xa;`?

### 6-5. Parent-Child Coordinate Calculation

- **Input**: Create a diagram placing child elements inside a group (swimlane)
- **Verification points**:
  - Are child element coordinates relative to the parent?
  - Does the calculation follow layout-guide.md (absolute coordinate = parent coordinate + relative coordinate)?

### 6-6. vertex/edge Attributes

- **Input**: Check the XML of diagrams created in 1-1 through 1-5
- **Verification points**:
  - Do shapes (boxes, etc.) have `vertex="1"`?
  - Do connection lines have `edge="1"`?

## 7. Layout Quality

> **Cross-cutting check**: Open diagrams generated in sections 1 and 5 in the draw.io desktop app and visually verify layout quality.

### 7-1. No Element Overlap

- **Input**: Create a diagram with 5 or more entities
- **Verification points**:
  - Do coordinate calculations prevent boxes from overlapping?
  - Is adequate spacing maintained?

### 7-2. Minimal Edge Crossing

- **Input**: Create a diagram with multiple relationships
- **Verification points**:
  - Are connection points (exitX/exitY/entryX/entryY) appropriately distributed?
  - Is edge crossing minimized?

### 7-3. Layout Principle Compliance

- **Input**: Create a domain model diagram
- **Verification points**:
  - Vertical direction = parent-child relationships (parent above, child below)
  - Horizontal direction = aggregate relationships
  - Does it follow layout principles?

### 7-4. Service and Repository Placement

- **Input**: Create a domain model diagram including domain services and repositories
- **Verification points**:
  - Are services and repositories placed at the edge (right side)?
  - Is the placement designed to avoid edge crossing?
