---
name: authoring-codelabs
description: >-
  Authors, updates, tests, and previews Google Codelabs using Markdown and the CLaaT (Codelabs as Code) tool. Use when creating new codelabs, updating existing tutorials, or generating step-by-step developer guides.
---

# Authoring Codelabs

Cheatsheet and guidelines for writing, testing, and previewing Codelabs using Markdown and the open-source **CLaaT (`claat`)** engine.

## Codelab Structure and Metadata

All Markdown codelabs are written as a single Markdown file (e.g. `codelab.md` or `index.lab.md`).
The file begins with a YAML frontmatter block enclosed in `---` delimiters.

### Frontmatter Specification

```yaml
---
id: gemini-interactions-java-sdk
summary: Learn how to build multimodal applications and managed agents with the Gemini Interactions Java SDK.
authors: Guillaume Laforge
categories: cloud,machinelearning,java
tags: cloud/java,kiosk,web
status: Published
feedback_link: https://github.com/example/repo/issues
analytics_account: G-XXXXXXXXXX
---
```

### Frontmatter Fields

*   **`id`** *(required)*: Lowercase, hyphen-separated, URL-safe string. Serves as the generated output folder name in `claat` and the URL path.
*   **`summary`** *(required)*: Single line, under 200 characters, summarizing the tutorial.
*   **`authors`**: Name(s) of the author(s).
*   **`categories`**: Comma-separated list of categories / technologies (e.g. `cloud,machinelearning,java`). Used for card grouping on landing pages.
*   **`tags`** / **`environments`**: Discoverability and target environment tags (e.g. `web`, `kiosk`).
*   **`status`**: Progress indicator (`Draft`, `Published`, `Deprecated`, `Hidden`).
*   **`feedback_link`**: URL where readers can report bugs or provide feedback.
*   **`analytics_account`**: Google Analytics 4 measurement ID (e.g. `G-XXXXXXXXXX`) or legacy property ID.
*   *(Note: Do not define `duration` in the frontmatter; overall duration is automatically calculated from individual step durations).*

### Spacing & Formatting Rules
*   **Blank line before H1**: Ensure a blank line exists between the closing `---` and the main `# Title` header.
*   Only **one H1 (`#`)** heading is allowed in the entire document (the codelab title).

---

## Step Architecture & Navigation

*   **Step Headings**: Each step is defined by an H2 heading (`##`). Step names should be **imperative verb phrases without manual numbers** (e.g. `## Set up: Project & API Key`, **NOT** `## 2. Set up...`).
    > ⚠️ **Do NOT prefix step headings with numbers** (e.g., do not write `## 1. Welcome` or `## 8. Leverage...`). The web component automatically renders the step number (`${step + 1}. ${label}`). If you include a number in the heading, it will be duplicated (e.g., `8. 8. Leverage...`).
*   **Subsections**: Use Heading 3 (`###`) and Heading 4 (`####`) within steps.
*   **Step Duration**: Every H2 step must specify an estimated duration on the line immediately following the heading:
    ```markdown
    ## Set up: Project & API Key
    Duration: 05:00
    ```
    *Format: `mm:ss` or `hh:mm:ss`.*
*   **Introduction Step**: First step should be `## Welcome` or `## Before you begin`.
    *   Include `What you'll do` and `What you'll need` subsections.
*   **Penultimate Step**: Should be `## Clean up` (or `## Clean up resources`) to delete any cloud or local resources created.
*   **Final Step**: Should be `## Congratulations` with pointers and links to documentation and references.

---

## Custom Elements & Callouts

### 1. Info Boxes (Asides)
Use callouts to emphasize supplementary notes, tips, and warnings (recommended max 2–3 per step).

*   **Positive (Green / Tip)**: Tips, shortcuts, and best practices.
    ```markdown
    > aside positive
    > **Tip**: You can use auto-numbering by starting list items with `1.`.
    ```
*   **Negative (Orange / Warning)**: Crucial warnings, prerequisites, or billing notes.
    ```markdown
    > aside negative
    > **Caution**: Forgetting to run the cleanup commands will result in charges to your Cloud billing account.
    ```

*Note: CLaaT also supports definition list syntax (`Positive\n: ...`) and HTML tags (`<aside class="positive">`), but the blockquote syntax (`> aside positive`) is the most portable.*

### 2. Download Buttons
To render a prominent download button, wrap a standard Markdown link inside `<button>` tags:
```markdown
<button>[Download Project Code](https://example.com/project.zip)</button>
```

### 3. YouTube Embeds
Embed videos using the `<video>` element with the YouTube video ID:
```markdown
<video id="8gd_fiHQW_E"></video>
```
*Tip: If users might consume this codelab in quiet environments, consider animated GIFs or screenshots in place of audio-dependent videos.*

### 4. Interactive Surveys
Gather attendee profiles or feedback at the start of a lab. Codelabs render surveys from a single-cell markdown table checklist:
```markdown
| -------------------------------------------------------------------------------- |
| ### What is your level of experience with Java?                                  |
|                                                                                  |
| - [ ] Beginner                                                                   |
| - [ ] Intermediate/Advanced                                                      |
| -------------------------------------------------------------------------------- |
```

### 5. Markdown Fragment Imports
Import reusable markdown fragments (such as common prerequisites or environment setup):
```markdown
<<shared/_credits_callout.md>>
```
*Note: Shared fragment filenames should begin with a leading underscore `_` to prevent them from compiling standalone.*

### 6. Command Output & Logs
For terminal outputs, logs, or non-executable blocks, use the `console` language identifier to disable code syntax highlighting:
```console
Operation "operations/..." finished successfully.
```

---

## Media Assets & Auxiliary Pages

*   **Images**: Store images in a relative `img/` subdirectory:
    ```markdown
    ![System Architecture](img/architecture.png)
    ```
*   **Audio**: Use the HTML5 `<audio>` element:
    ```html
    <audio controls src="img/narration.mp3"></audio>
    ```
*   **Large Reports / Complex Logs**: Avoid embedding massive text dumps directly or wrapping them in `<details>` tags (which can interfere with markdown block parsing). Extract large auxiliary reports into a separate Markdown file in the same directory and link to it:
    ```markdown
    [View the Detailed Benchmark Report (benchmarks.md)](benchmarks.md)
    ```

---

## Building & Previewing Locally (CLaaT)

You can validate, compile, and preview your codelab locally using `claat`:

### 1. Export HTML
```bash
claat export codelab.md
```
This generates a `./<id>/` directory containing `index.html` and slurped assets.

To export to a custom directory:
```bash
claat export -o ./dist codelab.md
```

### 2. Local Preview Server
Run the built-in local development server:
```bash
claat serve
```
Open `http://localhost:9090` in your browser to interact with the rendered codelab.

---

## Pre-flight & Quality Checklist

Before publishing your codelab, verify:

### Header & Metadata
- [ ] `id` is lowercase, hyphen-separated, and URL-safe.
- [ ] `summary` is concise and under 200 characters.
- [ ] A blank line exists between the closing `---` and the `# Title` heading.
- [ ] Only one H1 (`#`) heading exists in the entire document.

### Structure & Content
- [ ] First step is `## Welcome` or `## Before you begin` (with "What you'll do" and "What you'll need").
- [ ] Every step has a duration estimate (e.g. `Duration: 05:00`) immediately following the H2.
- [ ] Step titles are imperative verb phrases.
- [ ] Paragraphs are concise (typically 3 sentences or fewer).
- [ ] Code blocks use triple backticks with explicit language identifiers (`go`, `java`, `bash`, `console`).
- [ ] Penultimate step is `## Clean up` (deleting all created cloud/local resources).
- [ ] Final step is `## Congratulations` with reference documentation links.
- [ ] Successfully validated and previewed with `claat export <file>` and `claat serve`.
