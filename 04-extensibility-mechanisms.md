# The three mechanisms

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

→ Deep dive: `ai/EXTENSIBILITY-CUSTOM-TYPES.md`.

## 2. Captions — node labels & icons

You implement `IPatternEditorHelper.customShowElement(element, ctx)`, returning
`{ handled, caption, icon }`. Return `{ handled: false }` for nodes you don't customize.

*Work With* example: the `Modes` node shows `modes (Insert, Update, ...)` listing the enabled
modes.

```ts
customShowElement(element) {
  if (element.elementType === 'Modes')
    return { handled: true, caption: `modes (${enabledModes(element).join(', ')})` };
  return { handled: false };
}
```

→ Deep dive: `ai/EXTENSIBILITY-CAPTIONS.md`.

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

→ Deep dive: `ai/EXTENSIBILITY-CUSTOM-ACTIONS.md`.

## Optional: Variables

If your pattern exposes context variables in embedded code, implement
`IPatternVariableProvider` to feed KB Editor's autocomplete. → `ai/EXTENSIBILITY-VARIABLES.md`.

Next: [C# → TypeScript workflow](05-csharp-to-typescript-workflow.md).
