# bun-plugin-dts

A Bun plugin for generating `.d.ts` files.

## Install

```bash
bun add -d bun-plugin-dts
```

## Usage

```ts
import dts from 'bun-plugin-dts'

await Bun.build({
  entrypoints: ['./src/index.ts'],
  outdir: './dist',
  plugins: [
    dts()
  ],
})
```

## Options

This plugin utilizes [dts-bundle-generator](https://github.com/timocov/dts-bundle-generator) internally, allowing you to easily customize its behavior by passing specific options for dts-bundle-generator.

```ts
type Options = {
  libraries?: LibrariesOptions;
  /**
   * Fail if generated dts contains class declaration.
   */
  failOnClass?: boolean;
  output?: OutputOptions;
  compilationOptions?: CompilationOptions;
}

interface LibrariesOptions {
  /**
   * Array of package names from node_modules to inline typings from.
   * Used types will be inlined into the output file.
   */
  inlinedLibraries?: string[];
  /**
   * Array of package names from node_modules to import typings from.
   * Used types will be imported using `import { First, Second } from 'library-name';`.
   * By default all libraries will be imported (except inlined libraries and libraries from @types).
   */
  importedLibraries?: string[];
  /**
   * Array of package names from @types to import typings from via the triple-slash reference directive.
   * By default all packages are allowed and will be used according to their usages.
   */
  allowedTypesLibraries?: string[];
}

interface OutputOptions {
  /**
   * Sort output nodes in ascendant order.
   */
  sortNodes?: boolean;
  /**
   * Name of the UMD module.
   * If specified then `export as namespace ModuleName;` will be emitted.
   */
  umdModuleName?: string;
  /**
   * Enables inlining of `declare global` statements contained in files which should be inlined (all local files and packages from inlined libraries).
   */
  inlineDeclareGlobals?: boolean;
  /**
   * Enables inlining of `declare module` statements of the global modules
   * (e.g. `declare module 'external-module' {}`, but NOT `declare module './internal-module' {}`)
   * contained in files which should be inlined (all local files and packages from inlined libraries)
   */
  inlineDeclareExternals?: boolean;
  /**
   * Allows remove "Generated by dts-bundle-generator" comment from the output
   */
  noBanner?: boolean;
  /**
   * Enables stripping the `const` keyword from every direct-exported (or re-exported) from entry file `const enum`.
   * This allows you "avoid" the issue described in https://github.com/microsoft/TypeScript/issues/37774.
   */
  respectPreserveConstEnum?: boolean;
  /**
   * By default all interfaces, types and const enums are marked as exported even if they aren't exported directly.
   * This option allows you to disable this behavior so a node will be exported if it is exported from root source file only.
   */
  exportReferencedTypes?: boolean;
}

interface CompilationOptions {
  /**
   * Allows disable resolving of symlinks to the original path.
   * By default following is enabled.
   * @see https://github.com/timocov/dts-bundle-generator/issues/39
   */
  followSymlinks?: boolean;
  /**
   * Path to the tsconfig file that will be used for the compilation.
   */
  preferredConfigPath?: string;
}
```

## License

MIT
