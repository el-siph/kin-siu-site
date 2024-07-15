<script lang="ts">
	import { PrismicRichText } from '@prismicio/svelte';
	import { createEventDispatcher } from 'svelte';

	export let items;
	export let currentlyPlayingId;

	const dispatch = createEventDispatcher();

	function changeVideo(newVideoId: string) {
		dispatch('changeVideo', {
			newVideoId
		});
	}
</script>

<div class:hidden={items.length < 1} class="my-12 mx-4">
	<div class="grid grid-cols-12 gap-4">
		{#each items as item}
			<div class="col-span-12 sm:col-span-4 flex flex-col mb-8">
				<section>
					<PrismicRichText field={item.title} />
					<PrismicRichText field={item.description} />
				</section>
				<p class="mb-2">
					{#if currentlyPlayingId === item.embed.embed_url}
						<button class="bg-orange-500 opacity-70 text-white py-1 px-2 cursor-default"
							>&#9658; Playing</button
						>
					{:else}
						<button
							class="hover:bg-orange-500 hover:text-white py-1 px-2 transition-all ease-in-out"
							on:click={() => changeVideo(item.embed.embed_url)}>&#9658; Play</button
						>
					{/if}
				</p>
			</div>
		{/each}
	</div>
</div>

<style>
</style>
