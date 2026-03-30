# ASP.NET Core UI Components Skills

Skills for Syncfusion ASP.NET Core components, designed for use with AI coding assistants.

This repository contains 25+ AI-ready skill guides for working with Syncfusion ASP.NET Core components. Each skill includes a `SKILL.md` file that AI coding assistants can read automatically, plus a `references/` subfolder with detailed documentation covering setup, usage patterns, customization, and troubleshooting.

These skills provide focused, AI-ready guidance for building ASP.NET Core apps with Syncfusion components - including Grid, Charts, Scheduler, Rich Text Editor, Gantt Chart, and more. Each skill includes practical examples, configuration tips, and reference links to help with setup, customization, and common troubleshooting.

## Quick Start

### Option 1: Using npx (Recommended)

```bash
npx skills add https://github.com/syncfusion/aspnetcore-ui-components-skills
```

This will automatically add the skills to your workspace.

### Option 2: Manual Installation

**1. Clone this repository**
```bash
git clone https://github.com/syncfusion/aspnetcore-ui-components-skills.git
```

**2. Add it to your VS Code workspace**

Open your `.code-workspace` file (or create one) and add this repo as a second root folder:
```json
{
	"folders": [
		{ "path": "/path/to/your-aspnetcore-app" },
		{ "path": "/path/to/aspnetcore-ui-components-skills" }
	]
}
```

**3. Start asking questions**

Your AI assistant will automatically detect and apply the relevant skill based on your prompt:
```text
How do I add grouping to the Syncfusion DataGrid in ASP.NET Core?
How do I apply a dark theme to Syncfusion ASP.NET Core components?
```

No configuration required. Skills are loaded automatically from the workspace.

---

## Prerequisites

- An AI coding assistant that supports skills/context files (e.g., Syncfusion Code Studio, GitHub Copilot, Cursor, or similar tools)
- [.NET 8 SDK or later](https://dotnet.microsoft.com/download/dotnet)
- A Syncfusion license key ([free community license available](https://www.syncfusion.com/products/communitylicense))
- Basic knowledge of ASP.NET Core (Razor Pages, MVC) and Syncfusion components

## How These Skills Work

Each `SKILL.md` file contains a `description` field in its YAML frontmatter. AI coding assistants read this description to decide when to automatically apply a skill during a conversation. When you ask about a specific Syncfusion Component - for example, "How do I add sorting to my DataGrid?" - the AI assistant detects the match and loads the corresponding skill to guide its response.

You can also reference a skill explicitly by mentioning the component or control by name in your prompt.

### Example Prompts

```text
How do I bind data to the Syncfusion DataGrid in ASP.NET Core Razor Pages?
```
→ The AI assistant loads the Grid skill and uses its getting-started and data-binding reference docs.

```text
Show me how to implement master-detail in the TreeGrid.
```
→ The AI assistant loads the TreeGrid skill.

### Using Reference Files

Each `references/` subfolder contains deeper implementation guides. When the AI assistant loads a skill, it can also pull in these files when you ask follow-up questions:

```text
Show me how to export the Grid to Excel.
```
→ The AI assistant uses `references/advanced-features.md` from the Grid skill for the detailed answer.

## Skill File Structure

Every skill folder follows this layout:

```text
skills/
└── syncfusion-aspnetcore-<component>/
		├── SKILL.md                  ← Loaded by AI assistant; contains When to Use, Component Overview, and navigation links
		└── references/
				├── getting-started.md    ← Installation, setup, NuGet packages, basic configuration
				├── advanced-features.md  ← In-depth feature guides and code samples
				└── ...                   ← Additional reference files per component
```

`SKILL.md` sections:
- **When to Use This Skill** — trigger phrases and scenarios that activate this skill
- **Component Overview** — NuGet package, namespace, key capabilities at a glance
- **Documentation and Navigation Guide** — links to all reference files in the skill

## Repository Structure

```text
README.md
skills/
		syncfusion-aspnetcore-common/
		syncfusion-aspnetcore-theme/
		syncfusion-aspnetcore-license/
		syncfusion-aspnetcore-diagram/
		syncfusion-aspnetcore-grid/
		syncfusion-aspnetcore-pivot-table/
        syncfusion-aspnetcore-rich-text-editor/
		... (current skills present in repo)
```

## Skill Index

> **Tip:** Start with [Common](skills/syncfusion-aspnetcore-common/SKILL.md) if you are setting up a new project. For other tasks, open the skill that matches the component below.

### Foundation & Setup

- [Common](skills/syncfusion-aspnetcore-common/SKILL.md) — installation, script references, themes, bunit setup and localization & globalization
- [Theme](skills/syncfusion-aspnetcore-theme/SKILL.md) — themes and visual styling
- [License](skills/syncfusion-aspnetcore-license/SKILL.md) — license key registration and validation

### Editors & AI

- [AI AssistView](skills/syncfusion-aspnetcore-ai-assistview/SKILL.md) — AI-powered assistant interface
- [Inline AI Assist](skills/syncfusion-aspnetcore-inline-ai-assist/SKILL.md) — inline AI completions and suggestions
- [Block Editor](skills/syncfusion-aspnetcore-blockeditor/SKILL.md) — block-based document editing
- [Rich Text Editor](skills/syncfusion-aspnetcore-rich-text-editor/SKILL.md) — WYSIWYG text editing with formatting
- [Image Editor](skills/syncfusion-aspnetcore-image-editor/SKILL.md) — image manipulation and editing

### Inputs & Form Controls

- [TextBox](skills/syncfusion-aspnetcore-textbox/SKILL.md) — text input usage and validation
- [NumericTextBox](skills/syncfusion-aspnetcore-numerictextbox/SKILL.md) — numeric input patterns
- [ComboBox](skills/syncfusion-aspnetcore-combobox/SKILL.md) — single-select dropdown
- [DropdownList](skills/syncfusion-aspnetcore-dropdownlist/SKILL.md) — dropdown list patterns
- [ListBox](skills/syncfusion-aspnetcore-list-box/SKILL.md) — list selection
- [MultiSelect](skills/syncfusion-aspnetcore-multiselect/SKILL.md) — multi-selection UI
- [Switch](skills/syncfusion-aspnetcore-switch/SKILL.md) — boolean toggle control
- [Spinner](skills/syncfusion-aspnetcore-spinner/SKILL.md) — loading indicators

### Grids & Data Management

- [Grid](skills/syncfusion-aspnetcore-grid/SKILL.md) — sorting, filtering, paging, grouping, editing, export
- [TreeGrid](skills/syncfusion-aspnetcore-tree-grid/SKILL.md) — hierarchical data, master-detail, export
- [Gantt Chart](skills/syncfusion-aspnetcore-gantt-chart/SKILL.md) — project timeline and dependencies
- [Pivot Table](skills/syncfusion-aspnetcore-pivot-table/SKILL.md) — pivot reporting and aggregation
- [Query Builder](skills/syncfusion-aspnetcore-query-builder/SKILL.md) — visual query building interface

### Navigation & Layout

- [Dialog](skills/syncfusion-aspnetcore-dialog/SKILL.md) — modal dialogs and confirmations
- [Ribbon](skills/syncfusion-aspnetcore-ribbon/SKILL.md) — Office-style ribbon interface

### Data Visualization

- [Diagram](skills/syncfusion-aspnetcore-diagram/SKILL.md) — flowcharts and diagrams

### Utilities & Platform

- [DatePicker](skills/syncfusion-aspnetcore-datepicker/SKILL.md) — date selection and format handling
- [Uploader](skills/syncfusion-aspnetcore-uploader/SKILL.md) — file uploads and configuration
- [Image Editor](skills/syncfusion-aspnetcore-image-editor/SKILL.md) — image editing tools
- [Text-to-Speech / Speech-to-Text](skills/syncfusion-aspnetcore-speech-to-text/SKILL.md) — audio input and transcription
- [Security](skills/syncfusion-aspnetcore-security/SKILL.md) — CSP, headers, and runtime protections

---

## Getting Help

- Check [Syncfusion documentation](https://ej2.syncfusion.com/aspnetcore/documentation) and sample apps
- Review skill `references/` folders for implementation details
- Open an [issue](https://github.com/syncfusion/aspnetcore-ui-components-skills/issues) if a skill needs correction or extension
- Visit [Syncfusion community forums](https://www.syncfusion.com/forums/aspnetcore-js2)

---

## License

These skills are educational resources. Syncfusion components require a valid license key. To acquire a license, see https://www.syncfusion.com/sales/pricing.
