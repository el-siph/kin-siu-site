<script>
	import { PrismicRichText } from '@prismicio/svelte';
	export let events, title;
</script>

<div class:hidden={events.length < 1} class="my-12 mx-4">
	<h1 class="text-2xl font-bold">
		{title}
	</h1>
	{#each events as item}
		<div class="grid grid-cols-12 gap-12 mb-8">
			<div class="col-span-4">
				<div
					class="event-list-img w-auto h-48"
					style="background-image: url('{item.image.url}');"
				/>
			</div>
			<div class="col-span-7 flex flex-col">
				<section>
					<PrismicRichText field={item.title} />
					<p>{new Date(item.date).toLocaleDateString()}</p>
				</section>
				<section class="mt-10">
					<a
						class="px-2 py-1 bg-blue-500 hover:bg-blue-400 text-white"
						href={item.link.url}
						target={item.link.target}>Event Details</a
					>
				</section>
			</div>
		</div>
	{/each}
</div>

<style>
	.event-list-img {
		background-size: cover;
		background-position: center;
	}
</style>
