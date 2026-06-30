# GeneXus pattern concepts

A GeneXus pattern ships a small set of definition files; KB Editor reads them to know the
tree shape and property editors. Your extension supplies behavior for the dynamic parts.

```
Patterns/<PatternType>/
├── Definitions/
│   ├── <PatternType>.Pattern          main definition (version, generated objects, templates)
│   ├── <PatternType>Instance.xml      instance schema: element types, attributes, children
│   ├── <PatternType>CustomTypes.xml   custom type ids and their base data types
│   └── <PatternType>Settings.xml      settings schema
└── Source/ …                          the pattern's .NET implementation (your reference)
```

## Instance schema

`*Instance.xml` defines each **ElementType** (a kind of node), its **attributes**
(properties) and its allowed **child elements**. A `.gxPattern` instance is a tree of these.

```xml
<ElementType Name="Modes" Caption="Ins: {0}, Upd: {1}" CaptionParameters="Insert;Update">
  <Attributes>
    <Attribute Name="Insert" Type="enum{true;false;default}" DefaultValue="default" />
    <Attribute Name="Export" Type="enum{true;false;default}" DefaultValue="default" />
  </Attributes>
</ElementType>
```

### Attribute types and their editors

| `Type=` | Editor |
|---|---|
| `string` / `bool` / `int` | text / checkbox / number |
| `enum{A;B;C}` | static dropdown |
| `reference(KBObjectType)` | KB object picker |
| `code(Events)` | embedded code editor |
| **`custom(<Id>)`** | **dynamic dropdown — your provider supplies the values** |

## Settings vs instance

Patterns also have **settings** (`*Settings.xml`, edited as `.gxPatternSettings`). Custom
types can appear in either — e.g. Work With's `GridCustomRender` custom type is used by the
**settings** property `CustomRender`. The mechanism is the same: KB Editor calls your
`getTypeEditor(<Id>)` regardless of where the property lives.

## How it maps to your code

- `ElementType Name` → `PatternInstanceElement.elementType` (branch on this).
- instance node tag → `PatternInstanceElement.tag`.
- `custom(<Id>)` → KB Editor calls `getTypeEditor("<Id>")` on your support.
- `Caption` + `CaptionParameters` → the default label; override it via `customShowElement`.

> You normally **don't ship** the schema — it comes from the user's GeneXus install/KB. Your
> extension provides the providers keyed by pattern type.

Next: [Extensibility architecture](03-extensibility-architecture.md).
