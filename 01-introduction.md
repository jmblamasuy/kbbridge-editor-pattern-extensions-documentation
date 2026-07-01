# Introduction

## The problem

GeneXus patterns (Work With, and many third-party patterns) have rich, IDE-integrated
editors in the GeneXus desktop IDE — dynamic dropdowns, computed node captions, context
commands. When GeneXus pattern instances are edited inside **KB Editor** (the GeneXus visual
editor for VS Code / VSCodium), those smarts are not built in: KB Editor knows the pattern
**schema** (from the pattern's `*Instance.xml`) but not the **dynamic behavior**, which in
the IDE is implemented in .NET.

## The solution

KB Editor exposes a **`PatternExtensionAPI`**. A small VS Code extension — written by the
pattern's provider — registers **providers** that supply that dynamic behavior. KB Editor
then calls those providers while the user edits, giving pattern instances the same
first-class experience they have in the GeneXus IDE.

A few mechanisms cover the editor behavior of essentially every pattern:

1. **Custom Types** — compute the valid values for a `custom(<Id>)` property on demand.
2. **Captions** — override a node's label based on its data.
3. **Custom Actions** — add context commands to nodes.
4. **Node icons** — supply a node's icon as a bundled resource.

If the provider extension is not installed, KB Editor still works (graceful degradation):
dropdowns become plain text, captions fall back to defaults, and there are no extra commands.

## Who this is for

- **Pattern vendors** who want their pattern to feel native in KB Editor.
- **GeneXus teams** porting a pattern's editor behavior from .NET to TypeScript.

## Glossary

| Term | Meaning |
|---|---|
| **KB Editor** | GeneXus visual editor for VS Code / VSCodium (`kbbridge.genexus-visual-editor`). |
| **Pattern** | A GeneXus pattern (e.g. Work With) defined by `.Pattern` + `*Instance.xml`. |
| **Pattern instance** | A `.gxPattern` file: a tree of pattern element types over a transaction/object. |
| **Custom type** | A property type `custom(<Id>)` whose values are computed by a provider. |
| **Caption** | The label shown for a node in the editor tree. |
| **Command / action** | A context action attached to a node. |
| **SDK** | [`@kbbridge/genexus-sdk`](https://github.com/jmblamasuy/kbbridge-editor-genexus-sdk) — the TypeScript contract that mirrors the GeneXus .NET pattern SDK. |

Next: [GeneXus pattern concepts](02-genexus-pattern-concepts.md).
