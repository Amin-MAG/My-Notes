# Project Rules — Obsidian Notes Vault

## Document Conventions (`/make-doc`)

When adding headers, frontmatter, or links to a document, follow these rules:

### Frontmatter
- Always include `title` and `tags` fields
- Do NOT use a `related` field — links go in the "See Also" section instead
- Example:
```yaml
---
title: Document Name
tags:
  - tag1
  - tag2
---
```

### Linking
- Use standard markdown links, NOT Obsidian `[[wikilinks]]` — the vault is published to GitHub Pages
- Format: `[Display Name](Filename.md)`
- Use relative paths from the document's location

### See Also Section
- Place a "See Also" section at the bottom of the document with links to related topics
- Use `#` heading level (e.g., `# See Also`) if the document uses `#` for its sections, or `##` if the document uses `##`
- Keep link descriptions concise — use `— short reason` only when it adds value
- Example:
```markdown
# See Also

- [Kubernetes](Kubernetes.md)
- [Distributed Systems](Distributed-Systems.md)
```

### Cross-Linking
- When linking document A to document B, also link B back to A
- Add the backlink in B's existing "See Also" section, or create one if it doesn't exist
