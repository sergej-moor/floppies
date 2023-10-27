<script lang="ts">
	import { goto } from '$app/navigation';
	import ArtworkCard from '$lib/ArtworkCard.svelte';
	import { inview } from 'svelte-inview';
	import type { ObserverEventDetails } from 'svelte-inview';

	let dayInView = 1;
	import 'iconify-icon';

	let artworks = [
		{
			textColor: 'text-[#0000FF]',
			bgColor: 'bg-[#FFFFee]',
			day: 1
		},
		{
			textColor: 'text-[#FF00FF]',
			bgColor: 'bg-[#AEFFee]',
			day: 2
		},
		{
			textColor: 'text-[#FF00FF]',
			bgColor: 'bg-[#AEF002]',
			day: 3
		}
	];
</script>

<div
	class="fixed bottom-4 right-4 z-50 p-2 px-4 border-b-2 border-black bg-white mix-blend-difference"
>
	<a class="block" href={'#' + (dayInView + 1).toString()}
		><iconfiy-icon icon="mdi:ab-testing" />>
	</a>
</div>

<div class="snap-y snap-mandatory h-screen max-h-screen overflow-auto -mx-4 px-4 scroll-smooth">
	{#each artworks as card}
		<div class="snap-start h-screen">
			<ArtworkCard
				textColor={card.textColor}
				backgroundColor={card.bgColor}
				day={card.day}
				on:updateDayInView={(e) => {
					dayInView = e.detail.day;
					goto('#' + dayInView.toString());
				}}
			/>
		</div>
	{/each}
</div>
<!-- 	use:inview
			on:inview_change={(event) => {
				const { inView, entry, scrollDirection, observer, node } = event.detail;
				if (inView) dayInView = card.day;
				console.log(card.day)
			}}	 -->
