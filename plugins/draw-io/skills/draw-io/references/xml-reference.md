# XML Patterns & Style Reference

Reference for draw.io mxCell patterns, style attributes, and color palette.

## XML Comment Constraints

**NG examples:**
```xml
<!-- Order ---> Customer -->  ❌ Error
<!-- Order ---- Customer --> ❌ Error
```

**OK examples:**
```xml
<!-- Order to Customer -->   ✅ Separate with English word
<!-- Order → Customer -->    ✅ Use arrow symbol
<!-- Order - Customer -->    ✅ Single hyphen is fine
```

**Recommendation:**
- Use `to` consistently for edge (relationship) comments for clarity
- Non-ASCII characters are fine (only `--` needs attention)

## Basic mxCell Patterns

### Rectangle (Box)

```xml
<mxCell id="box1" value="Label" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="120" height="60" as="geometry" />
</mxCell>
```

### Text

```xml
<mxCell id="text1" value="Text content" style="text;html=1;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;whiteSpace=wrap;rounded=0;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="100" height="30" as="geometry" />
</mxCell>
```

### Arrow (Edge)

```xml
<mxCell id="edge1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" parent="1" source="box1" target="box2">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

## Extended mxCell Patterns

### Dashed Arrow

```xml
<mxCell id="edge2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;dashed=1;" edge="1" parent="1" source="box1" target="box2">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Edge Label

```xml
<mxCell id="edge-label" value="Label" style="edgeLabel;html=1;align=center;verticalAlign=middle;resizable=0;points=[];" vertex="1" connectable="0" parent="edge1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Group (swimlane)

```xml
<mxCell id="group1" value="Group Name" style="swimlane;fontStyle=1;startSize=26;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">
  <mxGeometry x="40" y="80" width="200" height="150" as="geometry" />
</mxCell>
<!-- Child elements use parent="group1" -->
<mxCell id="child1" value="Child Element" style="rounded=1;whiteSpace=wrap;html=1;" vertex="1" parent="group1">
  <mxGeometry x="20" y="40" width="80" height="40" as="geometry" />
</mxCell>
```

### Database (Cylinder)

```xml
<mxCell id="db1" value="DB Name" style="shape=cylinder3;whiteSpace=wrap;html=1;boundedLbl=1;backgroundOutline=1;size=15;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="60" height="80" as="geometry" />
</mxCell>
```

### Stack Layout (UML Class style)

Rectangle with a field list, used for ER diagrams and class diagrams.

```xml
<mxCell id="class1" value="ClassName" style="swimlane;fontStyle=1;childLayout=stackLayout;horizontal=1;startSize=30;horizontalStack=0;resizeParent=1;resizeParentMax=0;resizeLast=0;collapsible=1;marginBottom=0;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="160" height="90" as="geometry" />
</mxCell>
<mxCell id="class1-row1" value="+ field1: String" style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;spacingRight=4;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;" vertex="1" parent="class1">
  <mxGeometry y="30" width="160" height="30" as="geometry" />
</mxCell>
<mxCell id="class1-row2" value="+ method(): void" style="text;strokeColor=none;fillColor=none;align=left;verticalAlign=middle;spacingLeft=4;spacingRight=4;overflow=hidden;points=[[0,0.5],[1,0.5]];portConstraint=eastwest;rotatable=0;" vertex="1" parent="class1">
  <mxGeometry y="60" width="160" height="30" as="geometry" />
</mxCell>
```

**Height calculation:**
- Header: `startSize` (30px)
- Each row: 30px
- Total height = startSize + (number of rows × 30)

## Custom Properties (Metadata)

To add custom attributes (metadata) to a shape, wrap the mxCell in an `<object>` tag:

```xml
<object label="DB Server" zone="3" type="database" id="db1">
  <mxCell style="shape=cylinder3;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">
    <mxGeometry x="100" y="100" width="60" height="80" as="geometry" />
  </mxCell>
</object>
```

**Use cases:**
- Add additional information (English names, descriptions, etc.) to domain models
- Metadata for integration with external tools
- Tagging for search and filtering

**Extraction when reading:**
- Get custom properties from `<object>` tag attributes
- The `label` attribute is the display label; others are arbitrary metadata

## Customizing Connection Points

Customize connection points (edge attachment points) on shapes:

```xml
style="...;points=[[0,0.5],[1,0.5],[0.5,0],[0.5,1]];"
```

**Coordinate meaning:**
- `[0,0]` = top-left, `[1,0]` = top-right
- `[0,1]` = bottom-left, `[1,1]` = bottom-right
- `[0.5,0.5]` = center

**Examples:**
- `points=[[0,0.5],[1,0.5]]` - left and right center only
- `points=[[0.5,0],[0.5,1]]` - top and bottom center only

## Color Palette

| Usage                        | fillColor | strokeColor | Example                        |
|------------------------------|-----------|-------------|--------------------------------|
| Blue (default)               | #dae8fc   | #6c8ebf     | Domain layer, primary elements |
| Green (success/infra)        | #d5e8d4   | #82b366     | Infrastructure layer, done     |
| Yellow (warning/use case)    | #fff2cc   | #d6b656     | Use case layer, caution        |
| Orange (highlight)           | #ffe6cc   | #d79b00     | Highlights                     |
| Purple (presentation)        | #e1d5e7   | #9673a6     | Presentation layer             |
| Red (error)                  | #f8cecc   | #b85450     | Errors, danger                 |
| Gray (neutral)               | #f5f5f5   | #666666     | Backgrounds, containers        |

## Style Attribute Reference

| Attribute       | Values              | Description                               |
|-----------------|---------------------|-------------------------------------------|
| `rounded`       | 0/1                 | Rounded corners                           |
| `whiteSpace`    | wrap                | Text wrapping                             |
| `html`          | 1                   | Enable HTML                               |
| `fillColor`     | #RRGGBB             | Fill color                                |
| `strokeColor`   | #RRGGBB             | Stroke color                              |
| `fontStyle`     | 0/1/2/3             | 0=normal, 1=bold, 2=italic, 3=both        |
| `fontSize`      | number              | Font size                                 |
| `fontColor`     | #RRGGBB             | Font color                                |
| `align`         | left/center/right   | Horizontal alignment                      |
| `verticalAlign` | top/middle/bottom   | Vertical alignment                        |
| `dashed`        | 1                   | Dashed line                               |
| `opacity`       | 0-100               | Opacity                                   |

## Edge Styles

| Attribute    | Value                      | Description  |
|--------------|----------------------------|--------------|
| `edgeStyle`  | orthogonalEdgeStyle        | Right-angle edge |
| `edgeStyle`  | entityRelationEdgeStyle    | For ER diagrams (goes straight from exit/entry for a fixed distance before turning) |
| `curved`     | 1                          | Curved line (use with `edgeStyle=none`) |
| `rounded`    | 1                          | Round corners (curves at right-angle bends) |
| `elbow`      | vertical/horizontal        | Preferred bend direction (vertical = vertical-first) |
| `startArrow` | none/classic/block/oval/diamond | Start arrow |
| `endArrow`   | none/classic/block/oval/diamond | End arrow   |

### Choosing Edge Styles

The rule is based on which **faces** (sides) the edge connects:

- **Both left/right faces** (exitX=0 or 1, AND entryX=0 or 1) → `edgeStyle=entityRelationEdgeStyle;rounded=1` — goes straight horizontally before turning, avoids overlapping elements
- **All other combinations** (both top/bottom, or one top/bottom + one left/right) → `edgeStyle=orthogonalEdgeStyle;curved=1` — smooth right-angle curve

| Exit face | Entry face | Edge style |
|-----------|------------|------------|
| left/right | left/right | `entityRelationEdgeStyle` |
| top/bottom | top/bottom | `orthogonalEdgeStyle` + `curved=1` |
| top/bottom | left/right | `orthogonalEdgeStyle` + `curved=1` |
| left/right | top/bottom | `orthogonalEdgeStyle` + `curved=1` |

**Note:** `entityRelationEdgeStyle` in vertical layouts causes unwanted horizontal detours — always use `orthogonalEdgeStyle` when either face is top/bottom.

```xml
<!-- Both left/right faces (e.g. Order → Customer, horizontal) -->
<mxCell id="edge1" style="edgeStyle=entityRelationEdgeStyle;rounded=1;html=1;dashed=1;endArrow=open;endFill=0;exitX=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="box1" target="box2">
  <mxGeometry relative="1" as="geometry" />
</mxCell>

<!-- Other face combinations (e.g. top/bottom involved) -->
<mxCell id="edge2" style="edgeStyle=orthogonalEdgeStyle;curved=1;html=1;dashed=1;endArrow=open;endFill=0;exitX=0.5;exitY=1;entryX=0.5;entryY=0;" edge="1" parent="1" source="box1" target="box2">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Composition (◆) Direction

**◆ goes on the parent side (the whole / containing side)**

```text
Parent (whole) ◆────── Child (part)

Example: Order ◆────── OrderItem
         (Order contains OrderItem)
```

**Style specification:**
```text
When source is the parent: specify ◆ on startArrow
startArrow=diamondThin;startFill=1;endArrow=none;

When target is the parent: specify ◆ on endArrow
endArrow=diamondThin;endFill=1;startArrow=none;
```

- `diamondThin` = hollow diamond (aggregation)
- `startFill=1` / `endFill=1` = filled (composition)

## Parsing Examples When Reading

### Extract structure from an architecture diagram

```xml
<mxCell id="domain-group" value="domain/" style="swimlane;..." />
<mxCell id="usecase-group" value="usecase/" style="swimlane;..." />
<mxCell id="arrow1" source="usecase-group" target="domain-group" edge="1" />
```

Parse result:

```text
Layer structure:
- domain/ (blue: #dae8fc) - core layer
- usecase/ (yellow: #fff2cc) - use case layer

Dependencies:
- usecase → domain (depends on)
```

### Extract terms from a domain model

```xml
<mxCell id="entity1" value="Order&#xa;Order" style="rounded=1;fillColor=#dae8fc;..." />
<mxCell id="entity2" value="Customer&#xa;Customer" style="rounded=1;fillColor=#dae8fc;..." />
<mxCell id="edge1" source="entity1" target="entity2" style="..." edge="1" />
```

Parse result:

```text
Entities:
- Order
- Customer

Relationships:
- Order → Customer
```

### Key parsing points

- Get labels from the `value` attribute (`&#xa;` is a line break, so you can separate display names)
- Read edge connections from `source`/`target` attributes
- Determine element classification (layer, type) from `fillColor` in `style`
- Reconstruct group parent-child relationships from the `parent` attribute
- If `<object>` tags are present, extract custom properties as well
