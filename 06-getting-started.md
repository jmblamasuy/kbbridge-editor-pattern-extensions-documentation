# Getting started

## 1. Prerequisites

- **KB Editor** (`kbbridge.genexus-visual-editor`) installed in VS Code / VSCodium.
- Node.js 18+ and npm.

## 2. Get a project

- New pattern from scratch → clone **`kbbridge-editor-pattern-starter`**.
- Learning by example → clone **`kbbridge-editor-pattern-genexus-workwith`** (the Work With port).

The SDK (`@kbbridge/genexus-sdk`) is already **vendored** in both, so there is nothing extra
to install for it.

## 3. Build

```bash
npm install        # tooling + links the vendored SDK
npm run compile    # tsc → out/
npm run bundle     # vendored SDK → node_modules + out/node_modules
```

## 4. Implement (starter only)

Open `src/extension.ts` → `registerProviders()` and implement the mechanisms you need
(`ai/START-HERE.md` walks you through each). Then rebuild.

## 5. Package & install

```bash
npm run package    # → <name>-<version>.vsix
```

Install the `.vsix` where KB Editor lives, **Reload Window**, and open a `.gxPattern`
instance of your pattern type.

## 6. Verify

- A `custom(<Id>)` property shows a populated dropdown.
- Your customized node captions appear.
- Your context commands appear on right-click and run.

Check your extension's **OutputChannel** for logs.

## Where to go next

- The full build guide and exact SDK signatures: each project's **`ai/`** folder
  (`START-HERE.md`, `SDK-REFERENCE.md`, `EXTENSIBILITY-*.md`, `DEPLOY.md`).
- Publish your extension on the **`kbbridge-editor-pattern-extensions-releases`** download platform.
