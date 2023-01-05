# Storybook Framework Qwik

This is a framework to allow using [Storybook](https://storybook.js.org/) with [Qwik](https://qwik.builder.io/).

## Limitations

- This has only been tested with brand new Qwik applications and component libraries. This also hasn't been tested with a static build of storybook.
- Story is completely reloaded when component is changed (no hot module repacement)
- There is no automation yet for easily scaffolding storybook in a Qwik project.

## Usage

- In an existing Qwik project, run `npx storybook@next init --type html --parser ts` (Storybook 7 is required)
- install this package with `npm install storybook-framework-qwik -D`
- Tell storybook to use this package by adding it to main.js, and prevent it from picking up unsupported mdx files by removing `*.mdx` from `stories` (See [issue #17](https://github.com/literalpie/storybook-framework-qwik/issues/17))

```
module.exports = {
  stories: ["../src/**/*.stories.@(js|jsx|ts|tsx)"],
  addons: [...],
  framework: {
    name: "storybook-framework-qwik",
  },
  docs: {...},
};

```

- A stories folder with incompatible HTML stories will be created by the first step. Remove them, or replace them with Qwik stories:

```
import Header from "./header";

export default {
  title: "Story Example",
  component: Header,
};

export const Default = {};
```

- If desired, import global app styles in `preview.js`: `import "../src/global.css";`

## Qwik City

If using QwikCity features in your components, you may want to import the qwikCityDecorator, which wraps stories in `MockQwikCityProvider`. This can be added to all stories by exporting the decorator in a decorators array in `.storybook/preview.ts`:

```ts
import { qwikCityDecorator } from 'storybook-framework-qwik/qwik-city-decorator';
export const decorators = [qwikCityDecorator];
```

You can also add the decorator to individual stories or story files.

Because this framework is shipped only as an ESM module, this may require that you add `"type": "module"` to your `package.json` (or create a package.json inside your .storybook folder to only make this setting apply to storybook).

## Credit
Many parts of this package are based on code that I got from [this PR](https://github.com/BuilderIO/qwik/pull/2381), which got some ideas from [this discussion](https://github.com/BuilderIO/qwik/discussions/787).
