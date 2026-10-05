---
name: add-project
description: Add a project card to the GitHub Pages index. Use when the user asks to add, list, or feature a project/repository on the wartafak.github.io page.
---

# Add Project to Pages Index

Add one `<li class="project-card">` per requested project to `index.html`, with metadata fetched via `gh` and tags derived from inspecting the repo.

## Steps

### 1. Resolve the repo

The user names the project(s). If they give only a repo name, assume owner `Wartafak`. If the repo can't be resolved, stop and ask — never invent a URL.

Done when: every requested project maps to an existing `owner/repo`.

### 2. Fetch metadata

```bash
gh repo view <owner/repo> --json name,description,url,primaryLanguage,repositoryTopics
```

Done when: you have the canonical `url`, `name`, and `description` for each project. If the description is empty, ask the user for a one-line description instead of writing one yourself.

### 3. Derive tags

Inspect the repo — primary language, GitHub topics, README, and manifest (`Cargo.toml`, `package.json`, `go.mod`, `pyproject.toml`) — then add as many tags as make sense:

- Start with the primary language (e.g. `Rust`, `TypeScript`).
- Add further tags from repo topics or inspection — only tags the evidence supports (e.g. `CLI` only if it's actually a command-line tool: README says so, or the manifest defines a binary; `DevTools`, `Web`, `Action`, `Library` likewise).
- No hard limit, but every tag must earn its place: no near-duplicate synonyms for the same idea.

Done when: every tag on the card is backed by something you observed in the repo.

### 4. Insert the card

Add the card after the last existing `<li class="project-card">`, before the `<!-- Add additional ... -->` comment, following this exact shape:

```html
<li class="project-card">
  <h2 class="project-title">
    <a href="<url>"><name></a>
  </h2>
  <p class="project-description">
    <description>
  </p>
  <div class="tags">
    <span class="tag"><tag1></span>
    <span class="tag"><tag2></span>
  </div>
</li>
```

Skip the card if a card linking to the same URL already exists — tell the user instead.

Done when: one new card per requested project, each with correct name, URL, description, and tags.

### 5. Refresh the sitemap and publish

Set `<lastmod>` in `sitemap.xml` to today's date (`YYYY-MM-DD`), then commit and push:

```bash
git add -A && git commit -m "Add <name> to projects index" && git push
```

Done when: `git status` is clean and the push succeeds.
