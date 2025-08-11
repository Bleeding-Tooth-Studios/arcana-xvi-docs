# Systems Structure

Systems are singletons, which are [Flamework's controllers and services](https://flamework.fireboltofdeath.dev/docs/guides/creating-a-singleton).

## Basic Service

```ts
import { Service, OnTick } from '@flamework/core';

@Service()
export class MyService implements OnTick {
	onTick(dt: number) {
		print('My service is ticking', dt);
	}
}
```

## Basic Controller

```ts
import { Controller, OnRender } from '@flamework/core';

@Controller()
export class MyController implements OnRender {
	onRender(dt: number) {
		print('My controller is rendering', dt);
	}
}
```

## Systems

Systems are when services and controllers are seen as a pair.

You can generate a system with the [plop generator.](https://bleeding-tooth-studios.github.io/arcana-xvi-docs/docs/intro/plopfiles)

`npx plop system`

They communicate to their equivalents using shared remotes.
