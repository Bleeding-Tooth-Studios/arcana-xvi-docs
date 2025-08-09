---
sidebar_position: 1
---

# Plop Files

Arcana XVI uses Plop, a file generator for Node.js. This allows us to generate boilerplate code for new components and systems.

## Generators

Plop uses "generators" to create new files. Generators are defined in the `plopfile.js` file.
Arcana XVI has the following generators:

### Component Generator

Generates a new component.

```bash
plop component
```

or

```bash
npx plop component
```

Plop will prompt you for:

-   The folder category of the component (if left empty, it will be the root of the components folder)
-   The name of the component

And then generate 3 files:

-   `src/client/components/{folder}/Client{name}.ts`
-   `src/server/components/{folder}/Server{name}.ts`
-   `src/shared/components/{folder}/Shared{name}.ts`

Both the client and server components will inherit from the shared component, and uses [@rbxts/flamework-shared-components](https://www.npmjs.com/package/@rbxts/shared-components-flamework) to handle replication.

### System Generator

Generates a new system.

```bash
plop system
```

or

```bash
npx plop system
```

Systems are a service / controller singleton pairing. They are responsible for handling the logic of the game.

Plop will prompt you for:

-   The folder category of the system (if left empty, it will be the root of the systems folder)
-   The name of the system
