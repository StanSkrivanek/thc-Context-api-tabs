<!-- src/lib/components/tabs/TabList.svelte -->
<script lang="ts">
	import type { Snippet } from 'svelte';
	import { getTabsContext } from './tabs-context.svelte';

	interface Props {
		/** Additional CSS class */
		class?: string;

		/** Tab trigger buttons */
		children: Snippet;
	}

	let { class: className = '', children }: Props = $props();

	const tabs = getTabsContext();

	/**
	 * Handles keyboard navigation within the tab list.
	 * Supports both horizontal (Left/Right) and vertical (Up/Down) navigation
	 * since the layout can change responsively via CSS container queries.
	 */
	function handleKeyDown(event: KeyboardEvent) {
		const currentIndex = tabs.tabIds.indexOf(tabs.activeTabId);
		if (currentIndex === -1) return;

		let newIndex: number | null = null;

		switch (event.key) {
			case 'ArrowLeft':
			case 'ArrowUp':
				// Move to previous tab, wrap to end
				newIndex = currentIndex > 0 ? currentIndex - 1 : tabs.tabIds.length - 1;
				break;

			case 'ArrowRight':
			case 'ArrowDown':
				// Move to next tab, wrap to start
				newIndex = currentIndex < tabs.tabIds.length - 1 ? currentIndex + 1 : 0;
				break;

			case 'Home':
				newIndex = 0;
				break;

			case 'End':
				newIndex = tabs.tabIds.length - 1;
				break;

			default:
				return; // Don't prevent default for other keys
		}

		if (newIndex !== null) {
			event.preventDefault();
			const newTabId = tabs.tabIds[newIndex];
			tabs.setActiveTab(newTabId);

			// Focus the newly active tab button
			const tabButton = document.getElementById(`tab-${newTabId}`);
			tabButton?.focus();
		}
	}
</script>

<div
	class="tab-list {className}"
	role="tablist"
	aria-orientation={tabs.orientation}
	onkeydown={handleKeyDown}
	tabindex="-1"
>
	{@render children()}
</div>

<style>
	.tab-list {
		display: inline-flex;
		flex-direction: row;
		flex-wrap: wrap;
		gap: 0.25rem;
		padding: 0.25rem;
		background: var(--color-muted);
		border-radius: var(--radius-md);
		border: 1px solid var(--color-border);
		width: fit-content;
	}

	/* Under 500px: stack tabs vertically (side-by-side layout with panel) */
	@container (max-width: 500px) {
		.tab-list {
			flex-direction: column;
			width: auto;
			flex-shrink: 0;
		}
	}

	/* Explicit vertical orientation from parent */
	:global([data-orientation='vertical']) .tab-list {
		flex-direction: column;
		width: 100%;
	}
</style>
