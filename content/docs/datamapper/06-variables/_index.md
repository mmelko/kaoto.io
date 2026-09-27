---
title: "Variables"
description: "Use xsl:variable definitions as mapping sources and reference them in XPath expressions"
date: 2026-09-25
weight: 6
---

## Overview

The DataMapper supports `xsl:variable` as first-class mapping sources. Variables appear in the **Variables** panel on the source side and can be dragged onto target fields just like schema fields.

Variables have two scopes:

- **Global (template-level)** — defined at the top of the `xsl:template`, available anywhere in the mapping. Created from the Variables panel header.
- **Local (node-scoped)** — defined immediately before a specific target field, available only to that field and its following siblings within the same scope. Created from the `⋮` mapping context menu on a target field.

> [!NOTE]
> The XSLT following-sibling rule applies: a local variable can only be referenced by fields that come after it in the same scope. The DataMapper enforces this during drag-and-drop — drops that would violate the rule are rejected.

---

## The Variables Panel

The **Variables** panel sits in the source tree, below the Source Body and Parameters sections. It lists all variables available at the current mapping scope.

<!-- MEDIA PLACEHOLDER: Screenshot showing the Variables panel in the source tree with one global variable and one local variable visible, with the "+" button highlighted in the panel header. The dm-variables.gif from the 2.12 release blog post may be reusable here. -->

Each variable row shows:
- The variable name prefixed with `$`
- A scope hint in parentheses for local variables (e.g. `(ShipTo)` meaning the variable is defined inside the `ShipTo` field's scope)
- For global variables: an inline XPath input and **fx** button to set or edit the variable's value expression
- A delete (trash) button

---

## Add a Global Variable

A global variable is available throughout the entire mapping. Use it to compute a value once and reference it in multiple target fields.

### Steps

1. **Click the `+` button** in the Variables panel header

<!-- MEDIA PLACEHOLDER: Screenshot showing the Variables panel header with the "+" (Add global variable) button highlighted. -->

2. **Type the variable name** and press **Enter** to confirm (or **Escape** to cancel)

3. **Enter the value expression** in the inline XPath input that appears on the variable row, or click the **fx** icon to open the [XPath Editor](../07-xpath-editor/)

> [!TIP]
> Variable names must be valid XPath `NCName`s (no spaces, no `$` prefix — the `$` is added automatically when referencing the variable in expressions).

---

## Add a Local Variable

A local variable is scoped to a specific target field — it is inserted as an `xsl:variable` immediately before that field in the generated XSLT and is available only within that field's scope.

### Steps

1. **Click the `⋮` menu** on the target field where you want to define the variable
2. **Select "Add variable"**

<!-- MEDIA PLACEHOLDER: Screenshot showing the ⋮ context menu open with "Add variable" highlighted. -->

3. **Type the variable name** and press **Enter** to confirm

> [!NOTE]
> Local variables do not have an inline expression input in the Variables panel — their value is set by mapping source fields onto the variable row, just like any other target field.

> [!TIP]
> "Add variable" only appears in the `⋮` menu on container fields (fields that have children). It is not available on leaf fields or on instruction nodes like `for-each`.

---

## Use a Variable as a Mapping Source

Once a variable exists in the Variables panel, drag it onto a target field to create a `Var://` reference in the generated XPath.

### Steps

1. **Locate the variable** in the Variables panel
2. **Drag it** onto a target field — the mapping line shows a `Var://` prefix to indicate the source is a variable reference
3. The generated XPath expression references the variable as `$variableName`

<!-- MEDIA PLACEHOLDER: Screenshot showing a variable row being dragged onto a target field, with the resulting Var:// mapping line visible. -->

> [!TIP]
> You can also type `$variableName` directly in the inline XPath input or in the XPath Editor rather than dragging.

---

## Rename a Variable

Renaming a variable updates all mapping expressions that reference it.

**Double-click** the variable name in the Variables panel, or **right-click** the variable row and select **"Rename"**. Type the new name and press **Enter**.

> [!IMPORTANT]
> Renaming is safe — the DataMapper automatically updates all XPath expressions that reference the old name throughout the mapping tree.

---

## Delete a Variable

Click the **trash icon** on the variable row. A confirmation dialog appears.

> [!WARNING]
> Deleting a variable removes all mappings that reference it. This cannot be undone.

---

## Hide and Show Variables

Click the **eye icon** in the Variables panel header to toggle visibility of the variable list. This collapses the panel visually but does not affect the generated XSLT.

---

## Next Steps

1. **[XPath Editor](../07-xpath-editor/)** — write complex expressions that reference variables
2. **[Loop Mappings](../05-loop-mappings/)** — variables are particularly useful inside `for-each` and `for-each-group` scopes to capture intermediate values
