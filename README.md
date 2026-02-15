# Svelte 5 Tabs Component

A modern, accessible tabs component built with **Svelte 5's Context API** and reactive primitives (`$state`, `$derived`, `$effect`).

## Features

- **Svelte 5 Context API** - Uses `setContext`/`getContext` with Symbol keys for collision-free context sharing
- **Reactive State** - Built with `$state` and `$derived` runes for fine-grained reactivity
- **Fully Accessible** - Complete ARIA attributes, keyboard navigation (arrow keys, Home/End), and screen reader support
- **Compound Components** - Flexible `Tabs`, `TabList`, `Tab`, and `TabPanel` composition pattern
- **TypeScript** - Full type safety with exported interfaces
- **Responsive** - Supports horizontal and vertical orientations
- **Customizable** - CSS variables for easy theming

## Installation

```bash
pnpm install
```

## Usage

```svelte
<script lang="ts">
	import { Tabs, TabList, Tab, TabPanel } from '$lib/components/tabs';
</script>

<Tabs defaultTab="overview" orientation="horizontal">
	<TabList>
		<Tab id="overview">Overview</Tab>
		<Tab id="features">Features</Tab>
		<Tab id="pricing">Pricing</Tab>
		<Tab id="faq" disabled>FAQ</Tab>
	</TabList>

	<TabPanel id="overview">
		<p>Overview content here...</p>
	</TabPanel>

	<TabPanel id="features">
		<p>Features content here...</p>
	</TabPanel>

	<TabPanel id="pricing">
		<p>Pricing content here...</p>
	</TabPanel>

	<TabPanel id="faq">
		<p>FAQ content here...</p>
	</TabPanel>
</Tabs>
```

## Svelte 5 Context API

This component demonstrates the **Svelte 5 Context API** pattern for sharing state between compound components:

### Creating Context

```typescript
// tabs-context.svelte.ts
import { setContext, getContext } from 'svelte';

const TABS_KEY = Symbol('tabs');

export function createTabsContext(options: TabsOptions): TabsContext {
	// Reactive state using $state rune
	let tabIds = $state<string[]>([]);
	let activeTabId = $state(options.defaultTab ?? '');

	// Derived state using $derived rune
	const orientation = $derived(options.orientation ?? 'horizontal');

	const context: TabsContext = {
		get activeTabId() {
			return activeTabId;
		},
		get orientation() {
			return orientation;
		},
		get tabIds() {
			return tabIds;
		},

		registerTab(id: string) {
			tabIds = [...tabIds, id];
			if (!activeTabId) activeTabId = id;
		},

		setActiveTab(id: string) {
			activeTabId = id;
			options.onTabChange?.(id);
		},

		isActive(id: string) {
			return activeTabId === id;
		}
	};

	setContext(TABS_KEY, context);
	return context;
}

export function getTabsContext(): TabsContext {
	return getContext<TabsContext>(TABS_KEY);
}
```

### Using Context in Child Components

```svelte
<!-- Tab.svelte -->
<script lang="ts">
	import { onMount } from 'svelte';
	import { getTabsContext } from './tabs-context.svelte';

	let { id, children }: Props = $props();

	const tabs = getTabsContext();

	// Reactive derived state
	let isActive = $derived(tabs.isActive(id));

	onMount(() => {
		tabs.registerTab(id);
		return () => tabs.unregisterTab(id);
	});
</script>

<button role="tab" aria-selected={isActive} onclick={() => tabs.setActiveTab(id)}>
	{@render children()}
</button>
```

## API Reference

### `<Tabs>`

| Prop          | Type                         | Default        | Description                      |
| ------------- | ---------------------------- | -------------- | -------------------------------- |
| `defaultTab`  | `string`                     | -              | ID of the initially active tab   |
| `orientation` | `'horizontal' \| 'vertical'` | `'horizontal'` | Tab layout orientation           |
| `onTabChange` | `(tabId: string) => void`    | -              | Callback when active tab changes |
| `class`       | `string`                     | `''`           | Additional CSS class             |

### `<TabList>`

| Prop    | Type     | Default | Description          |
| ------- | -------- | ------- | -------------------- |
| `class` | `string` | `''`    | Additional CSS class |

### `<Tab>`

| Prop       | Type      | Default      | Description             |
| ---------- | --------- | ------------ | ----------------------- |
| `id`       | `string`  | **required** | Unique tab identifier   |
| `disabled` | `boolean` | `false`      | Whether tab is disabled |
| `class`    | `string`  | `''`         | Additional CSS class    |

### `<TabPanel>`

| Prop    | Type     | Default      | Description                  |
| ------- | -------- | ------------ | ---------------------------- |
| `id`    | `string` | **required** | Must match associated Tab id |
| `class` | `string` | `''`         | Additional CSS class         |

## Keyboard Navigation

| Key               | Action                      |
| ----------------- | --------------------------- |
| `Tab`             | Move focus to/from tab list |
| `←` / `→`         | Navigate tabs (horizontal)  |
| `↑` / `↓`         | Navigate tabs (vertical)    |
| `Home`            | Jump to first tab           |
| `End`             | Jump to last tab            |
| `Enter` / `Space` | Activate focused tab        |

## Development

```bash
# Start dev server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview
```

## Acknowledgments

- Built as a companion demo for **The Hackpile Chronicles** article on Svelte 5's Context API
- Thanks to the Svelte for their excellent tools and documentation

## License

This is a demonstration project for educational purposes.

## Contributing

This is a demo project, but feel free to fork and customize for your needs!

---

**Built with** ❤️ **using Svelte 5 and SvelteKit **
