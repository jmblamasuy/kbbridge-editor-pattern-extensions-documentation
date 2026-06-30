# C# → TypeScript workflow

If your pattern already has a GeneXus **.NET** implementation, the fastest faithful path is
to **transcribe** the editor classes to TypeScript against `@kbbridge/genexus-sdk`, which
mirrors the GeneXus .NET pattern SDK.

## Method

1. Put the pattern's .NET sources in a **git-ignored `reference/`** folder (never published).
2. Find the three editor classes — they are usually small even when the whole pattern is
   large:
   - the **CustomTypeSupport** (custom types),
   - the **EditorHelper** (`CustomShowElement` + `GetCommands`),
   - each **PatternEditorCommand** subclass.
3. Transcribe class-by-class, annotating each TS class with `// Equivalent to <C# file>`.

> Only the editor behavior matters. Code generation, build/delete processes, validators and
> the instance object model are **out of scope** — KB Editor never runs them.

## The .NET → SDK mapping

| GeneXus .NET (`Artech.Packages.Patterns.*`) | `@kbbridge/genexus-sdk` (TypeScript) |
|---|---|
| `PatternCustomTypeSupport.GetTypeEditor` | `IPatternCustomTypeSupport.getTypeEditor` |
| `PatternCustomTypeEditor.GetComboValues(...)` | `IPatternCustomTypeEditor.getValues(ctx): CustomTypeValue[]` |
| `PatternEditorHelper.CustomShowElement(el, ref caption, ref icon): bool` | `customShowElement(el, ctx): {handled, caption, icon}` |
| `PatternEditorHelper.GetCommands(el)` | `getCommands(el): PatternEditorCommand[]` |
| `PatternEditorCommand` (`Text`/`Query`/`Exec`) | `PatternEditorCommandBase` (`text`/`query`/`exec`) |
| `element.Type` | `element.elementType` |
| `element.Attributes.GetPropertyValue<T>("x")` | `element.attributes['x']` |
| `Children.AddNewElement(tag)` | `new PatternInstanceElement({tag,attributes,children}, parent, i, path)` + `parent.addChild(...)` |
| IDE dialogs (`GenexusUIServices.*`) | VS Code APIs (`QuickPick`, `showInputBox`, …) |

## Worked example

The `kbbridge-editor-pattern-genexus-workwith` project is a complete, faithful transcription of the GeneXus
*Work With* editor behavior. Read its `ai/EXAMPLES.md` for a file-by-file walkthrough.

## Rules

- Never ship the .NET sources (or anything decompiled) — only your original TypeScript.
- Transcribe from **your** pattern's sources (or a pattern you have the rights to). If
  porting a GeneXus-distributed pattern, add a `NOTICE` attributing GeneXus and confirm
  distribution terms.

Next: [Getting started](06-getting-started.md).
