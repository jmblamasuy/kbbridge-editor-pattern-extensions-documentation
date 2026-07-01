# The extensibility mechanisms

Each mechanism is illustrated below with the GeneXus *Work With* pattern. Full, build-it
guides (interfaces, invocation flow, gotchas) live in the projects' `ai/` folders.

## 1. Custom Types — dynamic dropdowns

You implement `IPatternCustomTypeSupport.getTypeEditor(id)`, returning an editor whose
`getValues(context)` computes the options for a `custom(<id>)` property.

*Work With* example: the `GridCustomRender` custom type returns the available grid controls.

```ts
class WorkWithCustomTypeSupport implements IPatternCustomTypeSupport {
  getTypeEditor(id: string) {
    return id === 'GridCustomRender' ? new GridCustomRenderEditor() : null;
  }
}
```

→ Deep dive: [`ai/EXTENSIBILITY-CUSTOM-TYPES.md`](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith/blob/main/ai/EXTENSIBILITY-CUSTOM-TYPES.md).

## 2. Captions — node labels

You implement `IPatternEditorHelper.customShowElement(element, ctx)`, returning
`{ handled, caption, icon }`. Return `{ handled: false }` for nodes you don't customize.
(The optional `icon` lets you override a node's icon inline; for icons that ship as bundled
resources use the dedicated hook in mechanism 4.)

*Work With* example: the `Modes` node shows `modes (Insert, Update, ...)` listing the enabled
modes.

```ts
customShowElement(element) {
  if (element.elementType === 'Modes')
    return { handled: true, caption: `modes (${enabledModes(element).join(', ')})` };
  return { handled: false };
}
```

→ Deep dive: [`ai/EXTENSIBILITY-CAPTIONS.md`](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith/blob/main/ai/EXTENSIBILITY-CAPTIONS.md).

## 3. Custom Actions — context commands

You implement `IPatternEditorHelper.getCommands(element)`, returning serializable commands
(usually subclasses of `PatternEditorCommandBase`). The command's `exec()` runs on click and
may mutate the instance tree.

*Work With* example: **"Add Filter Variable"** on the `FilterAttributes` node.

```ts
getCommands(element) {
  return element.elementType === 'FilterAttributes'
    ? [new AddFilterVariableCommand(element).toSerializable()].filter(Boolean)
    : [];
}
```

→ Deep dive: [`ai/EXTENSIBILITY-CUSTOM-ACTIONS.md`](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith/blob/main/ai/EXTENSIBILITY-CUSTOM-ACTIONS.md).

## 4. Node icons — bundled resource icons

You implement `IPatternEditorHelper.getNodeIcon(element, iconName?)`, returning the node's
icon as a self-contained resource (typically a `data:` URI). KB Editor resolves a node's icon
in order — the caption's inline `icon` (mechanism 2), then the schema's `Icon` when it exists
as a file next to the pattern, then this hook, then a built-in glyph — so returning `undefined`
falls through to the next source.

*Work With* example: the pattern declares its icons as .NET assembly resources
(`Icon="ObjectAttribute" IconResource="…,Artech.Patterns.WorkWith"`), which don't exist as
files on the client's KB. The extension bundles those images under `icons/` (named exactly like
the schema `Icon` values) and returns them here, so the tree shows the real icons with no
GeneXus SDK or .NET assemblies at runtime.

```ts
getNodeIcon(element, iconName) {
  if (!iconName || !/^[A-Za-z0-9_]+$/.test(iconName)) return undefined;
  const file = path.join(this.iconsDir, `${iconName}.ico`);
  return fs.existsSync(file)
    ? `data:image/x-icon;base64,${fs.readFileSync(file).toString('base64')}`
    : undefined;
}
```

→ Deep dive: the *Work With* example's `WorkWithEditorHelper.getNodeIcon` and its bundled
`icons/` folder.

## Optional: Variables

If your pattern exposes context variables in embedded code, implement
`IPatternVariableProvider` to feed KB Editor's autocomplete. → [`ai/EXTENSIBILITY-VARIABLES.md`](https://github.com/jmblamasuy/kbbridge-editor-pattern-genexus-workwith/blob/main/ai/EXTENSIBILITY-VARIABLES.md).

Next: [C# → TypeScript workflow](05-csharp-to-typescript-workflow.md).
