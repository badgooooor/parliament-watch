<script lang="ts">
	import SearchInput from '$components/SearchInput/SearchInput.svelte';
	import SearchResult from '$components/SearchResult/SearchResult.svelte';
	import type { SearchIndexCategory, SearchResults } from '$models/search';
	import { Search } from 'carbon-components-svelte';
	import type { ComponentType } from 'svelte';

	export let id: string;
	export let title: string;
	export let icon: ComponentType;
	export let searchPlaceholder: string;
	export let searchCategories: SearchIndexCategory[] = [];
	let className = '';
	export { className as class };

	let searchResults: SearchResults | null;
</script>

<section {id} class={className}>
	<div class="mx-auto flex max-w-[1280px] flex-col gap-6 px-4 py-[72px] text-text-01">
		<div class="flex flex-col items-start gap-2 md:flex-row">
			<div class="flex items-center gap-2 md:flex-1">
				<svelte:component this={icon} width="32" height="32" />
				<h2 class="fluid-heading-05">{title}</h2>
			</div>
			<div class="body-01 flex items-center self-stretch md:flex-1">
				<slot name="description" />
			</div>
		</div>
		{#if searchCategories.length > 0}
			<div class="relative">
				<SearchInput
					as={Search}
					size="lg"
					placeholder={searchPlaceholder}
					categories={searchCategories}
					bind:searchResults
				/>
				{#if searchResults}
					<SearchResult {searchResults} class="absolute left-0 z-10 w-full" />
				{/if}
			</div>
		{/if}
		<slot />
	</div>
</section>
