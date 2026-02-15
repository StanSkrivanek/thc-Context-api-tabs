<script lang="ts">
	import { Tab, TabList, TabPanel, Tabs } from '$lib/components/tabs';
	function handleTabChange(tabId: string) {
		console.log('Tab changed to:', tabId);
	}
</script>

<div class="page">
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
</div>

<style>
	.page {
		padding: 20px;
		font-family: Arial, sans-serif;
	}
	h3 {
		margin-top: 0;
	}
	ul {
		padding-left: 20px;
	}
</style>
