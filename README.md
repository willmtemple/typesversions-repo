# typesVersions does not apply in nested imports

This repository is meant to illustrate how type-declaration modules do not apply `typesVersions` mappings to nested imports.

To run the repro, enter the `base` directory and build the package

```
$ npm install
$ npm run build
```

The `dep` package is set up with a type-mapping that, if the `typesVersions` feature worked according to my expectations, would result in the compiler using different definitions of `const enum Foo` imported into `base`. Specifically, there are two versions of the enum within the `dep` package. One that defines `Foo.X` to be `"X value latest"` and one that defines it to be `"X value 3.1"`. However, despite compiling `base` with TypeScript 3.1.8, the result is the use of the "latest" value rather than the "3.1" value, because `typesVersions` does not influence the import of `"types/latest/index.d.ts"` within `dep/dep.shims.d.ts`. Instead, the companion shim `dep/dep.shims-3_1.d.ts` must be mapped using `typesVersions`, meaning that not only do we need duplicate module type declarartions, we need duplicate shims as well.
