# draw-io Skill - Test Cases

## 1. Create / Edit Workflow

> **Common prerequisite**: For all create/edit cases, verify that both reference files (xml-reference.md, layout-guide.md) are loaded **before** starting work.

### 1-1. New Domain Model Diagram (Multiple Aggregates, Composition, Cross-Aggregate References)

- **Input**: "Create a domain model diagram with an Order aggregate (Order → OrderItem), a Customer aggregate (Customer → ShippingAddress), and a Product aggregate (Product). Group by aggregate, and show cross-aggregate references: Order → Customer and OrderItem → Product."
- **Verification points**:
  - Are both reference files (xml-reference.md, layout-guide.md) loaded before starting?
  - Is `Glob` used to search for existing `.drawio` files?
  - If no existing file, is `Write` used to create a new one?
  - Is `darkMode="0"` specified?
  - **Grouping**: Are aggregates grouped using swimlanes? Does each child element's `parent` point to the group ID?
  - **Composition**: Are relationships expressed with `diamondThin` + `startFill=1`? Is ◆ placed on the parent side?
  - **Cross-aggregate references**: Are references expressed with dashed lines (`dashed=1`)?
  - **Layout**: Are parent-child relationships arranged vertically and cross-aggregate references horizontally?

### 1-2. New ER Diagram (Stack Layout)

- **Input**: "Create an ER diagram with 3 tables: User, Order, and Product."
- **Verification points**:
  - Are both reference files loaded before starting?
  - Is Stack Layout (UML Class style) used?
  - Is the total height correctly calculated as startSize + (rows × 30)?
  - Are inter-table relationships expressed with appropriate edge styles?

### 1-3. Adding an Entity and Edge to an Existing Diagram

- **Prerequisite**: The `.drawio` file from 1-1 exists
- **Input**: "Add a ProductCategory entity to the domain model diagram and link it to Product."
- **Verification points**:
  - Are both reference files loaded before starting?
  - Is the existing file found via `Glob`?
  - Is the existing XML retrieved with `Read` and existing IDs checked?
  - Do the new IDs avoid duplicating existing ones?
  - Is `Edit` used to partially update the XML (not overwrite with `Write`)?
  - Are connection points specified with `exitX`/`exitY`/`entryX`/`entryY`?
  - Is `open -a "draw.io"` NOT executed after editing?

### 1-4. Multi-Page Diagram Creation

- **Input**: "Create a 2-page architecture diagram with an overview page and a detail page."
- **Verification points**:
  - Are both reference files loaded before starting?
  - Are two `<diagram>` tags generated?
  - Does each `<mxGraphModel>` have `darkMode="0"`?
  - Does each page have an appropriate `name` and unique `id`?

## 2. Read Workflow

### 2-1. Reading an Existing Diagram

- **Prerequisite**: A `.drawio` file exists
- **Input**: "Check the domain model."
- **Verification points**:
  - Is xml-reference.md loaded before Glob? (step 1 → step 2 order)
  - Are `Glob` (search for `.drawio`) and `Read` (load XML) executed in order?
  - Are entity names extracted from the `value` attribute?
  - Are relationships extracted from `source`/`target`?
  - Is the output a structured summary?

## 3. Reference File Compliance (xml-reference.md / layout-guide.md)

### 3-1. Color Palette Compliance

- **Input**: "Create a 4-layer architecture diagram with Presentation, Use Case, Domain, and Infrastructure layers."
- **Verification points**:
  - Are the fillColor / strokeColor values from the xml-reference.md color palette (7 colors: blue, green, yellow, orange, purple, red, gray)?
  - Is the color usage meaningful (e.g., domain layer = blue, infrastructure = green)?

### 3-2. Edge Label Generation

- **Input**: "Add a labeled relationship '1 to many' between Order and Customer."
- **Verification points**:
  - Is an mxCell for the edge label generated with `edgeLabel` style?
  - Is `parent` set to the target edge's ID?
  - Is `connectable="0"` set?

### 3-3. Database Shape Generation

- **Input**: "Add a DB server in cylinder shape to the architecture diagram."
- **Verification points**:
  - Is `shape=cylinder3` style used?
  - Are fillColor/strokeColor set to infrastructure green (#d5e8d4 / #82b366)?

### 3-4. Element Enumeration Step

- **Input**: "Create a domain model diagram for an order management system. Include Order, Customer, Product, and Delivery."
- **Verification points**:
  - Is there a step to enumerate boxes, relationships, and groups before generating XML (SKILL.md step 4)?

## 4. XML Constraints & Edge Cases

> **Cross-cutting check**: For all `.drawio` files generated in sections 1 and 3, verify that the following constraints are met.

### 4-1. No `--` in XML Comments

- **Verification points**:
  - Is the NG pattern `<!-- Order ---> Customer -->` not generated?
  - Is `--` avoided, as in `<!-- Order to Customer -->`?

### 4-2. `darkMode="0"` Applied

- **Verification points**:
  - Is `darkMode="0"` set on all `<mxGraphModel>` elements?
- **Note**: The draw.io desktop app may remove `darkMode="0"` when saving. Verify in the XML immediately after `Write`/`Edit`.

### 4-3. ID Uniqueness

- **Verification points**:
  - Are prefixed IDs like `box1`, `edge1` free of duplicates?
  - Do IDs not conflict with existing ones during editing?

### 4-4. Line Break Characters

- **Input**: "Create a diagram with entities labeled in both Japanese and English (e.g., 注文 / Order)"
- **Verification points**:
  - Are line breaks within labels expressed as `&#xa;`?

### 4-5. Parent-Child Coordinate Calculation

- **Verification points**:
  - Are child element coordinates relative to the parent?
  - Does the calculation follow layout-guide.md (absolute = parent + relative)?

### 4-6. vertex/edge Attributes

- **Verification points**:
  - Do shapes (boxes, etc.) have `vertex="1"`?
  - Do connection lines have `edge="1"`?

## 5. Layout Quality

> **Cross-cutting check**: Open diagrams generated in sections 1 and 3 in the draw.io desktop app and visually verify layout quality.

### 5-1. No Element Overlap

- **Verification points**:
  - Do coordinate calculations prevent boxes from overlapping?
  - Is adequate spacing maintained?

### 5-2. Minimal Edge Crossing

- **Verification points**:
  - Are connection points (exitX/exitY/entryX/entryY) appropriately distributed?
  - Is edge crossing minimized?

### 5-3. Layout Principle Compliance

- **Verification points**:
  - Vertical direction = parent-child relationships (parent above, child below)
  - Horizontal direction = aggregate relationships
  - Does it follow layout principles?

### 5-4. Service and Repository Placement

- **Input**: Create a domain model diagram including domain services and repositories
- **Verification points**:
  - Are services and repositories placed at the edge (right side)?
  - Is the placement designed to avoid edge crossing?
