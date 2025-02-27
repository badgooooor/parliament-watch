<script lang="ts">
	import { formatThaiDate } from '$lib/date-parser';
	import Quotes from 'carbon-icons-svelte/lib/Quotes.svelte';
	import { PromiseStatus } from '$models/promise';

	export let id: string;
	export let status: PromiseStatus;
	export let latestProgressDate: Date | undefined = undefined;
	export let party: { name: string; logo: string };
	export let statements: string[];
	export let keywords: string[];
	export let categories: string[];

	export let isList = false;

	$: style = (() => {
		switch (status) {
			case PromiseStatus.inProgress:
				return {
					tag: 'bg-yellow-20 text-black',
					footer: 'bg-yellow-10'
				};
			case PromiseStatus.fulfilled:
				return {
					tag: 'bg-green-50 text-white',
					footer: 'bg-green-10'
				};
			case PromiseStatus.unhonored:
				return {
					tag: 'bg-magenta-50 text-white',
					footer: 'bg-magenta-10'
				};
			default:
				return {
					tag: 'bg-gray-30 text-black',
					footer: 'bg-gray-20'
				};
		}
	})();
</script>

<a href="/promises/{id}" class="{isList ? 'flex-row' : 'flex-col'} flex h-full w-full">
	<div class="{isList ? 'w-1' : 'h-1 w-full'} {style.tag} shrink-0" />
	<div
		class="{isList
			? 'grid h-[117px] grid-cols-7 gap-2 p-4 '
			: 'flex flex-col'} group w-full shrink-0 cursor-pointer bg-ui-background duration-100 hover:bg-ui-03"
	>
		<div class="{isList ? 'col-span-3 flex-row gap-2' : 'flex-col px-6'} flex">
			<a href="/promises/explore?party={party.name}" class="shrink-0">
				<button class="flex items-center gap-2 {isList ? '' : 'py-4 '}">
					<img src={party.logo} alt="" class="h-8 w-8 rounded-full border border-gray-30" />
					{#if !isList}
						<p class="body-01 text-text-01">พรรค{party.name}</p>
					{/if}
				</button>
			</a>
			<div class="flex flex-col gap-2">
				{#if !isList}
					<div class="flex gap-2">
						<Quotes class="text-text-03" />
						<div
							class="w-full translate-y-[50%] border-t border-ui-03 duration-200 group-hover:border-ui-01"
						></div>
					</div>
				{/if}
				<div class="flex {isList ? '' : 'h-[165px] items-center justify-center'}">
					<p
						class="text-black {isList
							? 'line-clamp-3 max-h-[calc(2.9*1.5em)]'
							: 'line-clamp-[7] max-h-[calc(6.85*1.5em)]'} heading-compact-02 h-auto overflow-hidden leading-6"
					>
						{statements}
					</p>
				</div>
				{#if !isList}
					<div class="flex flex-row-reverse gap-2">
						<Quotes class="rotate-180 text-text-03" />
						<div
							class="w-full translate-y-[50%] border-t border-ui-03 duration-200 group-hover:border-ui-01"
						></div>
					</div>
				{/if}
			</div>
		</div>

		<div
			class="{isList ? 'grid grid-cols-2' : 'flex flex-col px-6 pb-4 pt-3'} col-span-2 gap-[5px]"
		>
			{#each [{ label: 'คีย์เวิร์ด', items: keywords }, { label: 'หมวด', items: categories }] as { label, items } (label)}
				<div class="{isList ? 'h-fit' : 'items-center'} flex flex-wrap gap-[2px]">
					{#if !isList}
						<p class="body-01 text-text-02">{label}</p>
					{/if}
					{#each items as item, itemIndex (itemIndex)}
						{#if item}
							<a
								href="/promises/explore?{label === 'คีย์เวิร์ด'
									? `keyword=${item}`
									: `category=${item}`}"
								class="leading-none"
							>
								<button
									class="label-01 rounded-full py-[1px] text-text-01 {label === 'คีย์เวิร์ด'
										? 'bg-gray-10'
										: 'border border-black'} px-2">{item}</button
								></a
							>
						{/if}
					{/each}
				</div>
			{/each}
		</div>

		<div
			class="col-span-2 {isList
				? ''
				: `${style.footer} px-6 py-4`} grid grid-cols-2 gap-2 text-black"
		>
			<div class="flex flex-col gap-1">
				{#if !isList}
					<p class="heading-01">สถานะ</p>
				{/if}
				<div class="{style.tag} label-01 w-fit rounded-full px-2 py-[3px]">
					{status}
				</div>
			</div>
			<div class="flex flex-col gap-1">
				{#if !isList}
					<p class="heading-01">อัปเดตล่าสุด</p>
				{/if}
				<div class="body-01">
					{latestProgressDate ? formatThaiDate(latestProgressDate) : '-'}
				</div>
			</div>
		</div>
	</div>
</a>
