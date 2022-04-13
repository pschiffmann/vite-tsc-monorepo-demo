# vite tsc monorepo demo

This repository demonstrates how I set up my development environment for the following use case:
I'm developing an app (`packages/my-app`).
The app depends on other libraries (`packages/@scope-2/my-lib`) that are also maintained by me.
I use the app as the test environment for changes I make to the libraries.
For development, I want to serve `my-app` with a dev server ([vite](https://vitejs.dev/)).
When I change a source file in `my-lib`, I want the changes to be visible after a few ms in my browser.

Here is how I solved this:

- I run TypeScript in [build mode](https://www.typescriptlang.org/docs/handbook/project-references.html) and watch mode with `tsc -b -w`. In another shell, I run the vite dev server.
- References in `my-app/tsconfig.json` tell TypeScript to also watch `my-lib` and recompile files on changes.
- TypeScript watches the `src/` directory and writes to `dist/`.
- I use package.json [exports](https://nodejs.org/api/packages.html#package-entry-points) to rewrite imports for `my-lib/some-file` to `my-lib/dist/some-file.js`.
  TypeScript can only resolve types correctly if [moduleResolution](https://www.typescriptlang.org/tsconfig#moduleResolution) is set to `nodenext`.
