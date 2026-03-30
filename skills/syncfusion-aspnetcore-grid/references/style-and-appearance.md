# Style and Appearance — Syncfusion ASP.NET Core Grid

## Table of Contents
- [Overview](#overview)
- [CSS Variables and Theme Customization](#css-variables-and-theme-customization)
- [Header Styling](#header-styling)
- [Row and Alternating Row Styling](#row-and-alternating-row-styling)
- [Sorting Style](#sorting-style)
- [Filtering Style](#filtering-style)
- [Editing Style](#editing-style)
- [Grouping Style](#grouping-style)
- [Paging Style](#paging-style)

## When to Use This

- Customize grid colors and themes
- Apply CSS variables for styling
- Style specific grid elements
- Match grid to application branding
- Create alternating row colors
- [Aggregates Style](#aggregates-style)
- [Toolbar Style](#toolbar-style)
- [Selection Style](#selection-style)
- [Custom CSS Classes on Cells and Rows](#custom-css-classes-on-cells-and-rows)

---

## Overview

Grid appearance is fully customizable via CSS variable overrides and class targeting. The grid uses Bootstrap-compatible or Fluent2 theme CSS classes prefixed with `.e-grid`.

---

## CSS Variables and Theme Customization

Override Syncfusion CSS variables to change the look globally. Place overrides in your site CSS:

```css
:root {
    --color-sf-primary: #007bff;          /* Primary color */
    --color-sf-base-bg: #ffffff;           /* Grid background */
    --color-sf-grid-header-bg: #f8f9fa;   /* Header background */
}
```

Or target specific grid instances:
```css
#Grid .e-gridheader {
    background-color: #2d2d2d;
    color: #ffffff;
}
```

---

## Header Styling

```css
/* Header background and text */
.e-grid .e-headercell {
    background-color: #4a90e2;
    color: white;
    font-weight: 700;
}

/* Header border */
.e-grid .e-headercell .e-headercelldiv {
    border-right: 1px solid #357abd;
}
```

---

## Row and Alternating Row Styling

```css
/* All rows */
.e-grid .e-row {
    background-color: #ffffff;
}

/* Alternating rows (zebra) */
.e-grid .e-altrow {
    background-color: #f5f5f5;
}

/* Hover state */
.e-grid .e-row:hover .e-rowcell {
    background-color: #e8f0fe;
}
```

Apply row CSS class conditionally using `rowDataBound`:
```javascript
function rowDataBound(args) {
    if (args.data.Freight > 100) {
        args.row.classList.add('high-freight');
    }
}
```

---

## Sorting Style

```css
/* Sort icon color */
.e-grid .e-sortnumber {
    color: #007bff;
    background-color: transparent;
}

/* Sorted column header highlight */
.e-grid .e-columnheader .e-headercell.e-focus {
    background-color: #dbe9ff;
}
```

---

## Filtering Style

```css
/* Filter bar input */
.e-grid .e-filterbar .e-filterbarcell input {
    border-bottom: 2px solid #007bff;
    background: transparent;
}

/* Filter icon */
.e-grid .e-filtermenudiv .e-icons {
    color: #007bff;
}
```

---

## Editing Style

```css
/* Edit row highlight */
.e-grid .e-editedrow .e-rowcell {
    background-color: #fff3cd;
}

/* Added row */
.e-grid .e-addedrow .e-rowcell {
    background-color: #d4edda;
}
```

---

## Grouping Style

```css
/* Group caption row */
.e-grid .e-groupcaption {
    background-color: #e9ecef;
    font-weight: 600;
}

/* Group drop area */
.e-grid .e-groupdroparea {
    background-color: #f0f4ff;
    border: 1px dashed #007bff;
}
```

---

## Paging Style

```css
/* Active page button */
.e-grid .e-pager .e-currentitem {
    background-color: #007bff;
    color: white;
}

/* Pager container */
.e-grid .e-pagercontainer {
    background-color: #f8f9fa;
}
```

---

## Aggregates Style

```css
/* Footer aggregate row */
.e-grid .e-summaryrow .e-summarycell {
    background-color: #f0f4ff;
    font-weight: 700;
    color: #333;
}
```

---

## Toolbar Style

```css
/* Toolbar background */
.e-grid .e-toolbar {
    background-color: #343a40;
}

/* Toolbar button */
.e-grid .e-toolbar .e-btn {
    color: #ffffff;
}
```

---

## Selection Style

```css
/* Selected row */
.e-grid .e-selectionbackground {
    background-color: #cce5ff !important;
}

/* Selected cell */
.e-grid .e-cellselectionbackground {
    background-color: #b8daff !important;
}
```

---

## Custom CSS Classes on Cells and Rows

Apply CSS per cell using `queryCellInfo`:
```javascript
function queryCellInfo(args) {
    if (args.column.field === 'Freight' && args.data.Freight > 100) {
        args.cell.classList.add('high-value');
    }
}
```

Apply CSS per row using `rowDataBound`:
```javascript
function rowDataBound(args) {
    if (!args.data.Verified) {
        args.row.style.backgroundColor = '#ffcccc';
    }
}
```
