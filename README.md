# Pattern Extensibility for KB Editor

**KB Editor** is the GeneXus visual editor that runs inside VS Code / VSCodium (extension id
`kbbridge.genexus-visual-editor`). It opens GeneXus **pattern instances** (`.gxPattern`) as
an editable tree + properties panel, and exposes a **`PatternExtensionAPI`** so that
**pattern providers** can add a first-class editing experience for their own patterns:

- **Custom Types** — dynamic dropdown values for `custom(...)` properties.
- **Captions** — custom labels/icons for tree nodes.
- **Custom Actions** — context commands on nodes.

This documentation set is the conceptual introduction. The hands-on, build-it guides live in
each project's `ai/` folder.

## Start here

| If you want to… | Go to |
|---|---|
| Understand the idea & terms | [01-introduction.md](01-introduction.md) |
| Learn how GeneXus patterns are defined | [02-genexus-pattern-concepts.md](02-genexus-pattern-concepts.md) |
| Understand the extensibility architecture | [03-extensibility-architecture.md](03-extensibility-architecture.md) |
| See the three mechanisms | [04-extensibility-mechanisms.md](04-extensibility-mechanisms.md) |
| Port an existing .NET pattern | [05-csharp-to-typescript-workflow.md](05-csharp-to-typescript-workflow.md) |
| Build your first extension | [06-getting-started.md](06-getting-started.md) |

## The projects

- **[kbbridge-editor-genexus-sdk](https://github.com/jmblamasuy/kbbridge-editor-genexus-sdk)** — the public SDK (`@kbbridge/genexus-sdk`, MIT) you code against.
- **[kbbridge-editor-pattern-starter](https://github.com/jmblamasuy/kbbridge-editor-pattern-starter)** — a blank, ready-to-build template. Clone it and start.
- **[kbbridge-editor-pattern-genexus-workwith](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith)** — a complete worked example: a faithful port of the
  GeneXus *Work With* pattern (and a real, installable extension).
- **[kbbridge-editor-pattern-extensions-releases](https://github.com/jmblamasuy/kbbridge-editor-pattern-extensions-releases)** — the download platform/catalog where pattern
  extensions for KB Editor are published (`.vsix` via Releases).
- **[kbbridge-editor-pattern-extensions-documentation](https://github.com/jmblamasuy/kbbridge-editor-pattern-extensions-documentation)** — this documentation (concepts,
  architecture, the three mechanisms, C#→TypeScript workflow).

> For LLMs / AI assistants: see [`llms.txt`](llms.txt).
