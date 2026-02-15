<script lang="ts">
	import { Tab, TabList, TabPanel, Tabs } from '$lib/components/tabs';
	import { Accessibility, Palette, Smartphone, Zap } from 'lucide-svelte';

	function handleTabChange(tabId: string) {
		console.log('Tab changed to:', tabId);
	}
</script>

<div class="page">
	<div class="container">
		<header class="hero">
			<h1>Svelte 5 Tabs Component</h1>
			<p class="subtitle">
				A modern, accessible tabs component built with Svelte 5's powerful Context API and reactive
				primitives.
			</p>

			<div class="features">
				<div class="feature">
					<div class="feature-header">
						<Zap size={24} />
						<h3>Svelte 5 Context API</h3>
					</div>
					<p>
						Built using Svelte 5's new Context API with <code>$state</code> and
						<code>$derived</code> for reactive state management.
					</p>
				</div>
				<div class="feature">
					<div class="feature-header">
						<Accessibility size={24} />
						<h3>Fully Accessible</h3>
					</div>
					<p>
						Complete keyboard navigation, ARIA attributes, and screen reader support out of the box.
					</p>
				</div>
				<div class="feature">
					<div class="feature-header">
						<Palette size={24} />
						<h3>Modern Styling</h3>
					</div>
					<p>
						ShadCN UI-inspired design with pure CSS variables for easy theming and customization.
					</p>
				</div>
				<div class="feature">
					<div class="feature-header">
						<Smartphone size={24} />
						<h3>Responsive</h3>
					</div>
					<p>Supports both horizontal and vertical orientations with smooth transitions.</p>
				</div>
			</div>
		</header>

		<section class="demo">
			<h2>Interactive Demo</h2>
			<p>
				Try the tabs below. Use arrow keys to navigate, Tab to focus, and Enter/Space to activate.
			</p>

			<Tabs defaultTab="overview" onTabChange={handleTabChange} orientation="horizontal">
				<TabList>
					<Tab id="overview">Overview</Tab>
					<Tab id="features">Features</Tab>
					<Tab id="pricing">Pricing</Tab>
					<Tab id="faq" disabled>FAQ (Coming Soon)</Tab>
				</TabList>

				<TabPanel id="overview">
					<h3>Product Overview</h3>
					<p>Welcome to our amazing product. Here's what you need to know...</p>
				</TabPanel>

				<TabPanel id="features">
					<h3>Key Features</h3>
					<ul>
						<li>Feature one with detailed explanation</li>
						<li>Feature two that makes life easier</li>
						<li>Feature three for power users</li>
					</ul>
				</TabPanel>

				<TabPanel id="pricing">
					<h3>Pricing Plans</h3>
					<p>Choose the plan that fits your needs.</p>
				</TabPanel>

				<TabPanel id="faq">
					<h3>Frequently Asked Questions</h3>
					<p>Coming soon!</p>
				</TabPanel>
			</Tabs>
		</section>

		<section class="technical-details">
			<h2>Svelte 5 Context API Implementation</h2>

			<p>
				This tabs component showcases Svelte 5's powerful new Context API, which provides a clean
				and type-safe way to share state between components without prop drilling.
			</p>

			<div class="code-examples">
				<div class="code-block">
					<h3>Context Creation with Reactive State</h3>
					<pre><code
							>// Using $state for reactive state management
let activeTabId = $state(defaultTab ?? '');
let tabIds = $state&lt;string[]&gt;([]);

// Using $derived for computed values
const orientation = $derived(options.orientation ?? 'horizontal');

// Context provides reactive getters
const context: TabsContext = &#123;
  get activeTabId() &#123; return activeTabId; &#125;,
  get orientation() &#123; return orientation; &#125;,
  // ... methods
&#125;;</code
						></pre>
				</div>

				<div class="code-block">
					<h3>Component Communication</h3>
					<pre><code
							>// Child components access context reactively
const tabs = getTabsContext();

// Reactive checks for active state
let isActive = $derived(tabs.activeTabId === id);

// State changes propagate automatically
function handleClick() &#123;
  if (!disabled) &#123;
    tabs.setActiveTab(id);
  &#125;
&#125;</code
						></pre>
				</div>
			</div>

			<h2>Key Benefits of Svelte 5 Context API</h2>
			<div class="benefits">
				<ul class="benefits-list">
					<li><strong>Type Safety:</strong> Full TypeScript support with proper interfaces</li>
					<li><strong>Reactive:</strong> Automatic updates when context state changes</li>
					<li>
						<strong>Performance:</strong> Fine-grained reactivity without unnecessary re-renders
					</li>
					<li><strong>SSR Compatible:</strong> Works seamlessly with server-side rendering</li>
					<li><strong>Composable:</strong> Easy to nest and combine multiple contexts</li>
				</ul>
			</div>
		</section>
	</div>
</div>

<style>
	.page {
		padding: 2rem;
		font-family: 'regular', Arial, sans-serif;
		min-height: 100vh;
	}

	.container {
		max-width: 1100px;
		margin: 0 auto;
	}

	.hero {
		text-align: center;
		margin-bottom: 4rem;
		padding: 3rem 0;
		position: relative;
	}

	.hero h1 {
		font-size: 3rem;
		font-weight: 800;
		margin-bottom: 1.5rem;
		color: orangered;
		line-height: 1.1;
	}

	.subtitle {
		font-size: 1.375rem;
		color: var(--color-muted-foreground);
		margin-bottom: 3rem;
		line-height: 1.6;
		max-width: 700px;
		margin-left: auto;
		margin-right: auto;
		font-weight: 400;
	}

	.features {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
		gap: 2rem;
		margin-top: 3rem;
		perspective: 1000px;
	}

	.feature {
		padding: 2rem;
		border: 1px solid var(--color-border);
		border-radius: 0.75rem;
		color: orangered;
		position: relative;
		overflow: hidden;
	}

	.feature:hover {
		/* transform: translateY(-4px); */
		border-color: color-mix(in oklab, var(--color-primary) 22%, var(--color-border));
	}

	.feature-header {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.75rem;
		margin-bottom: 1rem;
		color: orangered;
	}

	.feature-header :global(svg) {
		color: var(--color-primary);
		flex-shrink: 0;
		transition: transform 0.3s ease;
		stroke: orangered;
	}

	.feature h3 {
		font-size: 1.25rem;
		font-weight: 600;
		margin: 0;
		color: orangered;
		line-height: 1.2;
	}

	.feature p {
		color: var(--color-muted-foreground);
		line-height: 1.6;
		margin: 0;
		font-size: 0.95rem;
	}

	.feature code {
		background-color: var(--color-muted);
		padding: 0.125rem 0.25rem;
		border-radius: 0.25rem;
		font-size: 0.875em;
		font-weight: 500;
	}

	.demo {
		margin-top: 4rem;
		padding: 2.5rem;
		background: var(--color-background);
		border: 1px solid var(--color-border);
		border-radius: 1rem;
		box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.05);
	}

	.demo h2 {
		font-size: 2.25rem;
		font-weight: 700;
		margin-bottom: 1rem;
		color: orangered;
		text-align: center;
	}

	.demo > p {
		color: var(--color-muted-foreground);
		margin-bottom: 2.5rem;
		font-size: 1.125rem;
		text-align: center;
		font-weight: 500;
	}

	.technical-details {
		margin-top: 4rem;
		padding-top: 3rem;
		border-top: 1px solid var(--color-border);
	}

	.technical-details h2 {
		font-size: 2rem;
		font-weight: 600;
		margin-bottom: 1rem;
		color: orangered;
	}

	.technical-details > p {
		font-size: 1.1rem;
		color: var(--color-muted-foreground);
		line-height: 1.6;
		margin-bottom: 2rem;
	}

	.code-examples {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
		gap: 2rem;
		margin-bottom: 3rem;
	}

	.code-block {
		border: 1px solid var(--color-border);
		border-radius: 0.5rem;
		background-color: var(--color-background);
		overflow: hidden;
	}

	.code-block h3 {
		background-color: var(--color-muted);
		padding: 1rem;
		margin: 0;
		font-size: 1.125rem;
		font-weight: 600;
		color: var(--color-foreground);
		border-bottom: 1px solid var(--color-border);
	}

	.code-block pre {
		margin: 0;
		padding: 1rem;
		background-color: color-mix(in oklab, var(--color-muted) 55%, transparent);
		overflow-x: auto;
	}

	.code-block code {
		font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
		font-size: 0.875rem;
		line-height: 1.5;
		color: var(--color-foreground);
	}

	.benefits {
		background-color: color-mix(in oklab, var(--color-muted) 35%, transparent);
		padding: 2rem;
		border-radius: 0.5rem;
		border: 1px solid var(--color-border);
	}

	.benefits ul {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	.benefits li {
		padding: 0.75rem 0;
		border-bottom: 1px solid var(--color-border);
	}

	.benefits li:last-child {
		border-bottom: none;
	}

	.benefits strong {
		color: orangered;
		font-weight: 600;
	}

	h3 {
		margin-top: 0;
	}
	ul {
		padding-left: 20px;
	}
</style>
