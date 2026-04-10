# ASP.NET Core UI Components Skills

Skills for Syncfusion ASP.NET Core components, designed for use with AI coding assistants.

This repository contains 40+ AI-ready skill guides for working with Syncfusion ASP.NET Core components. Each skill includes a `SKILL.md` file that AI coding assistants can read automatically, plus a `references/` subfolder with detailed documentation covering setup, usage patterns, customization, and troubleshooting.

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
How do I add grouping to the Syncfusion Grid in ASP.NET Core?
How do I configure the Scheduler for week view?
How do I apply a dark theme to Syncfusion ASP.NET Core components?
```

No configuration required. Skills are loaded automatically from the workspace.

---

## Prerequisites

- An AI coding assistant that supports skills/context files (e.g., Syncfusion Code Studio, GitHub Copilot, Cursor, or similar tools)
- [.NET 8 SDK or later](https://dotnet.microsoft.com/download/dotnet)
- A Syncfusion license key ([free community license available](https://www.syncfusion.com/products/communitylicense))
- Basic knowledge of ASP.NET Core Razor Pages and MVC models.

## How These Skills Work

Each `SKILL.md` file contains a `description` field in its YAML frontmatter. AI coding assistants read this description to decide when to automatically apply a skill during a conversation. When you ask about a specific Syncfusion Component - for example, "How do I add sorting to my Grid?" - the AI assistant detects the match and loads the corresponding skill to guide its response.

You can also reference a skill explicitly by mentioning the component or control by name in your prompt.

### Example Prompts

```text
How do I bind data to the Syncfusion Grid in ASP.NET Core?
```
→ The AI assistant loads the Grid skill and uses its getting-started and data-binding reference docs.

```text
Show me how to implement master-detail in the TreeGrid.
```
→ The AI assistant loads the TreeGrid skill.

### Using Reference Files

Each `references/` subfolder contains deeper implementation guides. When the AI assistant loads a skill, it can also pull in these files when you ask follow-up questions:

```text
Show me how to export the DataGrid to Excel.
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
- **When to Use This Skill** - trigger phrases and scenarios that activate this skill
- **Component Overview** - NuGet package, namespace, key capabilities at a glance
- **Documentation and Navigation Guide** - links to all reference files in the skill

## Repository Structure

```text
README.md
skills/
    syncfusion-aspnetcore-common/
    syncfusion-aspnetcore-theme/
    syncfusion-aspnetcore-license/
    syncfusion-aspnetcore-charts/
    syncfusion-aspnetcore-grid/
    syncfusion-aspnetcore-scheduler/
    ... (40+ total skills)
```

## Skill Index

> **Tip:** Start with [Common](skills/syncfusion-aspnetcore-common/SKILL.md) if you are setting up a new project. For all other tasks, find the skill that matches the specific component below.

### Foundation & Setup

- [Common](skills/syncfusion-aspnetcore-common/SKILL.md) - installation, licensing, themes, NuGet packages
- [Theme](skills/syncfusion-aspnetcore-theme/SKILL.md) - Material, Bootstrap, Fabric, Tailwind themes, dark mode
- [License](skills/syncfusion-aspnetcore-license/SKILL.md) - license key registration and validation

### Grids & Data Management

- [Grid](skills/syncfusion-aspnetcore-grid/SKILL.md) - sorting, filtering, paging, grouping, editing, export
- [TreeGrid](skills/syncfusion-aspnetcore-tree-grid/SKILL.md) - hierarchical data, master-detail, export
- [Kanban](skills/syncfusion-aspnetcore-kanban/SKILL.md) - Kanban board for task management
- [Query Builder](skills/syncfusion-aspnetcore-query-builder/SKILL.md) - visual query building interface
- [Pivot Table](skills/syncfusion-aspnetcore-pivot-table/SKILL.md) - pivot data analysis and reporting

### Scheduling & Calendars

- [Scheduler](skills/syncfusion-aspnetcore-scheduler/SKILL.md) - calendar events, appointments, recurring events
- [DatePicker](skills/syncfusion-aspnetcore-datepicker/SKILL.md) - date selection and calendar display

### Editors & Input Components

- [Rich Text Editor](skills/syncfusion-aspnetcore-rich-text-editor/SKILL.md) - WYSIWYG text editing with formatting
- [Image Editor](skills/syncfusion-aspnetcore-image-editor/SKILL.md) - image manipulation and editing
- [Inputs](skills/syncfusion-aspnetcore-inputs/SKILL.md) - text inputs, numeric inputs, and field components
- [Block Editor](skills/syncfusion-aspnetcore-blockeditor/SKILL.md) - block-based document editing
- [Markdown Converter](skills/syncfusion-aspnetcore-markdown-converter/SKILL.md) - convert between Markdown and HTML formats
- [Dropdowns](skills/syncfusion-aspnetcore-dropdowns/SKILL.md) - dropdown list, combo box, multi-select
- [DropDownList](skills/syncfusion-aspnetcore-dropdownlist/SKILL.md) - list selection and filtering
- [ComboBox](skills/syncfusion-aspnetcore-combobox/SKILL.md) - editable dropdown with autocomplete
- [MultiSelect](skills/syncfusion-aspnetcore-multiselect/SKILL.md) - multiple item selection
- [TextBox](skills/syncfusion-aspnetcore-textbox/SKILL.md) - text input and validation
- [NumericTextBox](skills/syncfusion-aspnetcore-numerictextbox/SKILL.md) - numeric input and formatting
- [ListBox](skills/syncfusion-aspnetcore-list-box/SKILL.md) - list selection component
- [Uploader](skills/syncfusion-aspnetcore-uploader/SKILL.md) - file upload and management
- [Speech-to-Text](skills/syncfusion-aspnetcore-speech-to-text/SKILL.md) - voice input and transcription

### Data Visualization & Charts

- [Charts](skills/syncfusion-aspnetcore-charts/SKILL.md) - line, area, bar, column, scatter, bubble charts
- [Accumulation Chart](skills/syncfusion-aspnetcore-accumulation-chart/SKILL.md) - pie, doughnut, funnel charts
- [Maps](skills/syncfusion-aspnetcore-maps/SKILL.md) - geographic mapping and location visualization

### Navigation & Layout

- [Popups](skills/syncfusion-aspnetcore-popups/SKILL.md) - dialogs, tooltips, and popup components
- [Ribbon](skills/syncfusion-aspnetcore-ribbon/SKILL.md) - Office-style ribbon interface
- [File Manager](skills/syncfusion-aspnetcore-file-manager/SKILL.md) - file browser and management
- [Tree View](skills/syncfusion-aspnetcore-tree-view/SKILL.md) - hierarchical tree navigation and display

### Gantt & Project Management

- [Gantt Chart](skills/syncfusion-aspnetcore-gantt-chart/SKILL.md) - project timeline, dependencies, and resource management

### Diagram & Visualization

- [Diagram](skills/syncfusion-aspnetcore-diagram/SKILL.md) - flowcharts, organizational charts, and diagramming

### Smart Components

- [Smart Paste](skills/syncfusion-aspnetcore-smart-paste/SKILL.md) - intelligent clipboard paste handling
- [Smart TextArea](skills/syncfusion-aspnetcore-smart-textarea/SKILL.md) - enhanced textarea with autocomplete

### AI Integration

- [AI AssistView](skills/syncfusion-aspnetcore-ai-assistview/SKILL.md) - AI-powered assistant interface
- [Chat UI](skills/syncfusion-aspnetcore-chat-ui/SKILL.md) - conversational UI and chat components
- [Inline AI Assist](skills/syncfusion-aspnetcore-inline-ai-assist/SKILL.md) - inline AI suggestions and completions

### Other UI Components

- [Spinner](skills/syncfusion-aspnetcore-spinner/SKILL.md) - loading spinners and progress indicators
- [Switch](skills/syncfusion-aspnetcore-switch/SKILL.md) - toggle switches and boolean controls

### Security

- [Security](skills/syncfusion-aspnetcore-security/SKILL.md) - CSP configuration and browser-level protections for ASP.NET Core applications.

---

## Getting Help

- Check the [Syncfusion ASP.NET Core documentation](https://www.syncfusion.com/aspnet-core-ui-controls)
- Review skill reference files in the `references/` folders
- Visit [Syncfusion community forums](https://www.syncfusion.com/forums/aspnetcore-js2)

---

## License

These skills are provided as educational resources for working with Syncfusion ASP.NET Core components. Syncfusion components require a valid license key. To acquire a license, you can quote a purchase at https://www.syncfusion.com/sales/pricing.