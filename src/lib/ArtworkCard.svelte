<script lang="ts">
	import { inview } from 'svelte-inview';
	import { createEventDispatcher } from 'svelte';
	export let textColor = 'text-[#FF36FF]';
	export let backgroundColor = 'bg-[#FFF000]';
	let image = '/placeholder.gif';
	let title = 'Superformula Part One';
	export let day = 1;

	const dispatch = createEventDispatcher();
</script>

<div class="relative">
	<div class={`h-screen pt-16 -mx-4 p-4 block ${backgroundColor} ${textColor}`} id={day.toString()}>
		<div class="">
			<img src={image} class="w-full" alt="" />
			<div class="border-b-black border-b-2" />

			<h1
				class="text-3xl font-extrabold"
				use:inview
				on:inview_enter={() => {
					dispatch('updateDayInView', { day });
				}}
			>
				{title.toLocaleUpperCase()}
			</h1>
			<div class="text-2xl">{'Day'.toUpperCase()} {day.toString().padStart(3, '0')}</div>
			<p>
				Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor
				invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam
				et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est
				Lorem ipsum dolor sit amet.
			</p>

			<a href={'#' + day} class="underline my-4 block">Take a look ---></a>
		</div>
	</div>
</div>
