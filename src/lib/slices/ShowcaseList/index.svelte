<script lang="ts">
	import ShowcaseListDisplay from '$lib/components/ShowcaseListDisplay.svelte';
	import type { Content } from '@prismicio/client';

	export let slice: Content.ShowcaseListSlice;

	import YoutubeEmbed from '$lib/components/YouTubeEmbed.svelte';

	let player;
	let currentlyPlayingId = '';

	function extractVideoId(url: string) {
		var regExp = /^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#&?]*).*/;
		var match = url.match(regExp);
		return match && match[7].length == 11 ? match[7] : false;
	}

	const toggle = (newId: string) => {
		const videoId = extractVideoId(newId);
		currentlyPlayingId = newId;
		player.loadVideoById(videoId);
	};
</script>

<section data-slice-type={slice.slice_type} data-slice-variation={slice.variation}>
	<div class:hidden={currentlyPlayingId === ''}><YoutubeEmbed bind:player /></div>
	<div
		class="min-w-max h-60 flex flex-col justify-center align-middle"
		class:hidden={currentlyPlayingId !== ''}
	>
		<h1 class="text-center text-2xl bold">Select a Video Below</h1>
	</div>

	<ShowcaseListDisplay
		items={slice.primary.showcaselistitem}
		{currentlyPlayingId}
		on:changeVideo={(e) => toggle(e.detail.newVideoId)}
	/>
</section>
