<script lang="ts">
	import { type TimeLine, getDateData, compareDate } from './TimeLine';
	import TimeItem from './TimeItem.svelte';
	import TimeLineToolTip from './TimeLineToolTip.svelte';
	import { shortMonthNames } from '$lib/date-parser';

	export let timeLineData: TimeLine[];
	export let selectedDate: Date;
	export let startedAt: Date | undefined;
	export let endedAt: Date | undefined;
	export let handleSelectDate: (date: Date) => void;

	$: max = Math.max(...timeLineData.map((d) => Math.max(d.in, d.out)));
	$: dateData = getDateData(timeLineData, startedAt, endedAt);
</script>

<div class="no-scrollbar relative flex h-full flex-col overflow-y-auto px-[44px]">
	{#each dateData as year}
		{#each year.months as month}
			<div>
				<div class="absolute -ml-7 w-6 text-text-02">
					<p class="label-01">{shortMonthNames[month.id]}</p>
					<p class="label-01">
						{month.id === 0 ? (year.year + 543).toString().slice(-2) : ''}
					</p>
				</div>
			</div>
			{#each month.days as day}
				<div class="{compareDate(selectedDate, day.date) ? 'sticky bottom-0 top-0' : ''} flex">
					<div class="flex w-[65px] flex-none flex-col">
						<TimeItem {day} {selectedDate} {max} {handleSelectDate} align="vertical" />
					</div>
					{#if compareDate(selectedDate, day.date)}
						<div class="z-1000 relative flex grow justify-between">
							<div class="w-full border-t-[0.4px]" />
							<div class="absolute right-0 w-fit rounded-sm bg-interactive-02 px-4 py-2">
								<TimeLineToolTip {day} {selectedDate} />
							</div>
						</div>
					{/if}
				</div>
			{/each}
		{/each}
	{/each}
</div>

<style>
	.no-scrollbar::-webkit-scrollbar {
		display: none;
	}
	.no-scrollbar {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}
</style>
