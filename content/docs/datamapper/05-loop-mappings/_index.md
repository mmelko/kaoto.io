---
title: "Loop Mappings"
description: "Iterate over collections with for-each and group items with for-each-group"
date: 2026-09-25
weight: 5
---

## Overview

Loop mappings iterate over a source collection and apply a set of field mappings to each item. The DataMapper supports two loop instructions:

- **`for-each`** — Iterate over every item in a source collection and map its fields to the target
- **`for-each-group`** — Group the items of a source collection by a key or pattern, then map each group

Collection fields are identified by the layer icon <img src="datamapper-layer.png" alt="Layer icon" style="display: inline; height: 1.2em; vertical-align: middle;"> in the document tree.

---

## For-Each Mapping

Use `xsl:for-each` when you want to transform each item of a source collection into a corresponding target item.

### Steps

1. **Identify the target collection field** — Look for the layer icon <img src="datamapper-layer.png" alt="Layer icon" style="display: inline; height: 1.2em; vertical-align: middle;"> on the target field

2. **Click the `⋮` menu** on the target collection field and select **"Wrap with Instruction" → "Wrap with for-each"**
{{< image-sh src="datamapper-for-each-for-each.png" text="Select wrap with for-each" >}}

3. **Specify the source collection** — Choose which source collection to iterate over
{{< image-sh src="datamapper-for-each-condition.png" text="Select source collection to iterate" >}}

4. **Map the collection item fields** — Create mappings for individual fields within each collection item
{{< image-sh src="datamapper-for-each-mappings.png" text="Map fields for each collection item" >}}

> [!IMPORTANT]
> Inside a for-each mapping, field paths are relative to the current collection item. For example, if iterating over `Items`, reference `Name` instead of `Items/Name`.

---

## Sorting For-Each Results

Add `xsl:sort` to a `for-each` to control the output order.

### Steps

1. **Create a for-each mapping** as described above

2. **Click the `⋮` menu** on the `for-each` node and select **"Configure Sort"**
{{< image-sh src="datamapper-configure-sort.png" text="Select Configure Sort from the for-each context menu" >}}

3. **Configure sort keys** — Enter the XPath expression for the field to sort by, or type ahead to select a field
{{< image-sh src="datamapper-configure-sort-modal.png" text="Configure Sort modal with sort key and options" >}}

4. **Add additional sort keys** (optional) — Click **"Add sort key"** to add secondary sort criteria; items are sorted by each key in order
{{< image-sh src="datamapper-add-sort-key.png" text="Add sort key" >}}

5. **Configure Ascending/Descending** (optional) — Toggle the direction per sort key
{{< image-sh src="datamapper-asc-desc.png" text="Ascending/Descending toggle" >}}

6. **Re-order sort keys** (optional) — Drag the handle to reorder
{{< image-sh src="datamapper-dnd-sort-key-order.png" text="Re-order sort keys by drag-and-drop" >}}

7. **Expand advanced properties** (optional) — Click the slider button on a sort key for data-type and case-order options
{{< image-sh src="datamapper-advanced-sort-key-properties.png" text="Sort key advanced properties" >}}
{{< image-sh src="datamapper-advanced-sort-key-properties-expanded.png" text="Advanced properties expanded" >}}

8. **Click Save** to apply
{{< image-sh src="datamapper-sort-save.png" text="Save sort configuration" >}}

---

## Multiple For-Each Mappings

Merge multiple source collections into a single target collection by stacking multiple `for-each` mappings under the same target field.

### Steps

1. **Create the first for-each mapping** as described above

2. **Add another for-each** — Click **"Add Mapping Instruction"** in the placeholder below the first mapping and select **"Wrap with for-each"**
{{< image-sh src="datamapper-wrap-with-for-each.png" text="Add second for-each mapping" >}}

3. **Configure the second collection** — Select the source collection and map its fields
{{< image-sh src="datamapper-map-2nd-for-each-children.png" text="Configure second collection and map its fields" >}}

{{< video src="./dm_multiplemappings.mp4" subtitles="./dm_multiplemappings.vtt" >}}

> [!TIP]
> This is useful for merging items from two different source arrays into a single output array.

---

## For-Each-Group Mapping

Use `xsl:for-each-group` when you need to group items from a source collection before mapping them. Each group is processed as a unit — for example, grouping order lines by product category and emitting one summary element per category.

### Grouping strategies

| Strategy | XSLT attribute | Description |
|---|---|---|
| **Group By** | `group-by` | Group items that produce the same value for the grouping expression. The most common strategy. |
| **Group Adjacent** | `group-adjacent` | Group consecutive items that produce the same value. Resets when the value changes. |
| **Group Starting With** | `group-starting-with` | Start a new group each time an item matches the XPath pattern. |
| **Group Ending With** | `group-ending-with` | End the current group each time an item matches the XPath pattern. |

### Steps

1. **Click the `⋮` menu** on the target collection field and select **"Wrap with Instruction" → "Wrap with for-each-group"**

<!-- MEDIA PLACEHOLDER: Screenshot showing the context menu open on a collection target field with "Wrap with Instruction" flyout expanded and "Wrap with for-each-group" highlighted. -->

2. **Enter the source collection XPath** in the inline input — this is the population to group (the `select` attribute of `xsl:for-each-group`)

3. **Click the `⋮` menu** on the `for-each-group` node and select **"Configure for-each-group"** to open the configuration modal

<!-- MEDIA PLACEHOLDER: Screenshot showing the ForEachGroup configuration modal with the strategy dropdown and grouping expression field visible. -->

4. **Choose a grouping strategy** from the dropdown — select one of the four strategies described above

5. **Enter the grouping expression** — the XPath expression that produces the grouping key for each item (for example, `Category` to group by the `Category` child element). Use the **fx** icon to open the XPath Editor for complex expressions.

6. **Add sort keys** (optional) — Sort the groups before processing using the Sort keys section, which works the same as [Sorting For-Each Results](#sorting-for-each-results) above

7. **Click Save**

8. **Map fields inside the group** — Inside the `for-each-group` scope, map fields as normal. Field paths are relative to the current group's context item. Use `current-group()` to reference all items in the current group.

> [!TIP]
> Once inside a `for-each-group` scope, the **Inner Instruction** submenu offers **"Inner for-each current-group()"** to iterate over the members of the current group for detail-level mappings.

<!-- MEDIA PLACEHOLDER: Screencast showing the full for-each-group workflow: wrap → configure modal (strategy + grouping expression) → map fields inside the group. The dm-for-each-group.gif from the 2.12 release blog post may be reusable here. -->

### Nesting groups

For hierarchical groupings, add an inner `for-each-group` inside an existing `for-each-group` scope:

1. **Click the `⋮` menu** on a field inside the outer group and select **"Inner Instruction" → "Inner for-each-group"**
2. Configure the inner grouping expression and strategy
3. Map fields for each inner group

---

## Next Steps

1. **[Variables](../06-variables/)** — use `xsl:variable` as mapping sources inside loops
2. **[XPath Editor](../07-xpath-editor/)** — write complex grouping and iteration expressions
