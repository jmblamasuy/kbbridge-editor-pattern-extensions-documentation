# Extensibility architecture

```
Your extension  ──codes against──▶  @kbbridge/genexus-sdk  ──implemented by──▶  KB Editor
(registers providers)               (PatternExtensionAPI,                       (parses .gxPattern,
                                     IPattern* interfaces,                        renders tree,
                                     PatternInstanceElement)                      calls your providers)
```

Your extension depends only on the **SDK contract** (vendored, MIT, no runtime deps), never
on KB Editor internals. KB Editor hands you the API object at runtime.

## Activation & registration

1. VS Code activates your extension. Your `package.json` declares
   `"extensionDependencies": ["kbbridge.genexus-visual-editor"]`, so KB Editor is present.
2. You obtain the API and register providers per pattern type:
   ```ts
   const kb = vscode.extensions.getExtension('kbbridge.genexus-visual-editor');
   const api = await kb.activate();   // resolves KB Editor's exported API
   api.patternAPI.registerCustomTypeSupport('WorkWith', customTypes);
   api.patternAPI.registerEditorHelper('WorkWith', editorHelper);
   ```
   > **Tolerate activation ordering.** `extensionDependencies` *should* make KB Editor activate
   > first, but on some setups it doesn't — your extension may run before KB Editor has
   > populated its exports, so `api.patternAPI` is momentarily missing. Don't give up on the
   > first try: retry acquiring it for a few seconds (the starter and Work With `activate()`
   > ship an `acquirePatternAPI` helper that does exactly this).
3. You dispose (unregister) on deactivate.

One extension can register **several** pattern types.

## How KB Editor calls you

- For a `custom(<Id>)` property → `getTypeEditor(Id)` then `getValues(context)`.
- When rendering a node → `customShowElement(element, context)` (caption/icon) and
  `getCommands(element)` (context commands).
- When the user runs a command → the command's `exec()`.

Context objects carry the current node, property, values, pattern type, instance name, file
path, and (optionally) a KB object cache.

## Graceful degradation

Missing provider ⇒ KB Editor falls back: plain text for custom types, default captions, no
commands. Provider callbacks must **fail soft** (return empty / `{handled:false}`), never
throw.

## One package or two?

For a single pattern, **one package** is enough. If you ship **many** patterns and want to
share code, you can split into a shared helper library + a patterns extension. That is an
optimization, not a requirement.

Deeper, build-it version: each project's [`ai/ARCHITECTURE.md`](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith/blob/main/ai/ARCHITECTURE.md) and [`ai/SDK-REFERENCE.md`](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith/blob/main/ai/SDK-REFERENCE.md).

Next: [The extensibility mechanisms](04-extensibility-mechanisms.md).
