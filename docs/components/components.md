# Ordinary & Internal Components

Components are the building blocks of the Arcana XVI Framework.

## Ordinary Component Structure

Components in Arcana XVI are identical to [Flamework's](https://flamework.fireboltofdeath.dev/docs/additional-modules/components/creating-a-component) components, with the exception of the `Shared`, `Client`, and `Server` suffixes.

The `Shared` component is the base component, and is inherited by the `Client` and `Server` components.

The `Client` and `Server` components are inherited from the `Shared` component, and are used to replicate the component across the server and client.

You can automatically create a shared component by using the [plop generator.](https://bleeding-tooth-studios.github.io/arcana-xvi-docs/docs/intro/plopfiles)

For example:

```ts
// shared
interface ValueStorageState {
	value: number;
}

@Component()
export class SharedValueStorageComponent extends SharedComponent<ValueStorageState> {
	protected state = {
		value: 0,
	};
}

// server
@Component({
	tag: 'ValueStorageComponent',
})
export class ServerValueStorageComponent extends ValueStorageComponent implements OnStart {
	public onStart() {
		task.spawn(() => {
			while (task.wait(3)) {
				this.increment();
			}
		});
	}

	@Action()
	private increment() {
		return {
			...this.state,
			value: this.state.value + 1,
		};
	}
}

// client
@Component({
	tag: 'ValueStorageComponent',
})
export class ClientValueStorageComponent extends ValueStorageComponent {
	@Subscribe((state) => state.value)
	private onIncrement(newValue: number) {
		print(`new value: ${newValue}`);
	}
}
```

More on how shared components work are in the [shared components documentation.](https://www.npmjs.com/package/@rbxts/shared-components-flamework)

### Client Components

Client components represent components that are attached to entities clientside.

### Server Components

Server components represent components that are attached to entities serverside.

### Shared Components

Shared components represent components that are shared between the server and client.
They are not directly implemented as attachable components, but contain a shared `state` and `remotes` field that is inherited by the client and server components.

## Internal Component Structure

This comes with the exception of components with a \_ suffix, which are internal components and usually do not have a `tag` field.
Usually, they are attached to the entity by other components via their constructor. This is done to fragment responsibilities and keep the codebase clean.
Internal components do not typically follow the same structure as ordinary components.

```ts
@Component({
	tag: 'PlayerActor',
})
export class ClientPlayerActor extends SharedPlayerActor {
	viewmodel: CharacterViewmodel;

	constructor() {
		super();
		this.viewmodel = this.components.addComponent<CharacterViewmodel>(this.instance);
	}
}
```

```ts
@Component({})
export class CharacterViewmodel extends BaseComponent<{}, CharacterRigR6> implements OnStart, OnRender {
	onStart() {
		// do stuff
	}

	onRender() {
		// do other stuff
	}
}
```
