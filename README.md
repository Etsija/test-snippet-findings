# test-snippet-findings

A test repository for validating [SCANOSS](https://www.scanoss.com/) snippet scanning behavior. It contains TypeScript files copied from well-known open source projects and deliberately mutilated — functions have been deleted so that SCANOSS reports them as **snippet findings** rather than whole-file license matches.

## Purpose

SCANOSS distinguishes between two types of matches:
- **License findings** — triggered when a whole file matches a known OSS file verbatim.
- **Snippet findings** — triggered when only a portion of a file matches OSS code.

By removing a significant share of functions from each copied file, the files no longer hash-match their originals as complete units, but the retained code blocks still produce snippet-level hits.

## Repository structure

The snippet files are split across the main repository and a Git submodule so that a single ORT scan produces snippet findings with **two distinct provenances** — useful for testing provenance-grouped UI views.

```
src/                          ← main repo (provenance 1)
  class-transformer__ClassTransformer.ts
  class-validator__MetadataStorage.ts
  class-validator__ValidationExecutor.ts

submodule/                    ← git submodule: Etsija/test-snippet-findings-sub (provenance 2)
  src/
    rxjs__bufferTime.ts
    tsyringe__dependency-container.ts
    typeorm__RelationLoader.ts
```

ORT creates a separate `ScanResult` (with its own `provenance`) for the root repository and for each submodule, which is what drives the grouping in the snippet findings data model.

---

## Source files

### `class-transformer__ClassTransformer.ts`

- **Original repo:** [typestack/class-transformer](https://github.com/typestack/class-transformer)
- **Original file:** `src/ClassTransformer.ts`
- **License:** MIT
- **Removed:**
  - `classToPlainFromExist` — converts a class instance to a plain object using an existing plain object as target
  - `classToClassFromExist` — copies a class instance into an existing class instance

---

### `class-validator__MetadataStorage.ts`

- **Original repo:** [typestack/class-validator](https://github.com/typestack/class-validator)
- **Original file:** `src/metadata/MetadataStorage.ts`
- **License:** MIT
- **Removed:**
  - `groupByPropertyName` — groups `ValidationMetadata` entries by their property name
  - `getTargetValidatorConstraints` — returns constraint metadata for a given target
  - `getMetadataStorage` — exported factory function that creates/returns the global metadata storage singleton

---

### `class-validator__ValidationExecutor.ts`

- **Original repo:** [typestack/class-validator](https://github.com/typestack/class-validator)
- **Original file:** `src/validation/ValidationExecutor.ts`
- **License:** MIT
- **Removed:**
  - `whitelist` — strips or rejects object properties that have no validation metadata
  - `stripEmptyErrors` — filters out `ValidationError` entries that have no constraints or children
  - `conditionalValidations` — evaluates conditional validation metadata to decide whether validation should run
  - `mapContexts` — copies user-supplied context objects onto the matching constraint keys of a `ValidationError`
  - `getConstraintType` — resolves the constraint type string from metadata or a custom validator

---

### `rxjs__bufferTime.ts`

- **Original repo:** [ReactiveX/rxjs](https://github.com/ReactiveX/rxjs)
- **Original file:** `packages/rxjs/src/internal/operators/bufferTime.ts`
- **License:** Apache-2.0
- **Removed:**
  - All three TypeScript overload signatures for `bufferTime`
  - The full JSDoc comment block (description, examples, `@param`/`@return` tags)
  - The `complete` handler inside `bufferTimeSubscriber` — which emitted all remaining active buffers on source completion

---

### `tsyringe__dependency-container.ts`

- **Original repo:** [microsoft/tsyringe](https://github.com/microsoft/tsyringe)
- **Original file:** `src/dependency-container.ts`
- **License:** MIT
- **Removed:**
  - `registerInstance` — convenience method to register a pre-existing value instance
  - `registerSingleton` — registers a class or token with `Lifecycle.Singleton`
  - `beforeResolution` — registers a pre-resolution interceptor callback
  - `afterResolution` — registers a post-resolution interceptor callback
  - `dispose` — disposes the container and all registered `Disposable` instances
  - `getAllRegistrations` — looks up all registrations for a token, walking up to the parent container
  - `resolveParams` — resolves constructor parameter tokens, handling transform and multi descriptors
  - `ensureNotDisposed` — guard that throws if the container has already been disposed

---

### `typeorm__RelationLoader.ts`

- **Original repo:** [typeorm/typeorm](https://github.com/typeorm/typeorm)
- **Original file:** `src/query-builder/RelationLoader.ts`
- **License:** MIT
- **Removed:**
  - `loadManyToManyOwner` — builds a query to load the related side of a many-to-many owner relation via a junction table
  - `loadManyToManyNotOwner` — same for the inverse (non-owner) side of a many-to-many relation
  - `enableLazyLoad` — wraps an entity property with `Object.defineProperty` getters/setters to lazily fetch relation data on first access
