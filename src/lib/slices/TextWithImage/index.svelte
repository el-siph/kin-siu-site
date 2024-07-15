<script lang="ts">
	import { isFilled, type Content } from '@prismicio/client';
	import { PrismicImage } from '@prismicio/svelte';

	import Bounded from '$lib/components/Bounded.svelte';
	import PrismicRichText from '$lib/components/PrismicRichText.svelte';

	export let slice: Content.TextWithImageSlice;
</script>

<Bounded
	as="section"
	class="bg-white"
	data-slice-type={slice.slice_type}
	data-slice-variation={slice.variation}
>
	<div
		class="grid grid-cols-1 items-center gap-8 md:grid-cols-2"
		class:hidden={slice.primary.flip_orientation}
	>
		<div>
			<PrismicRichText field={slice.primary.text} />
			{#if slice.primary.buttonLink}
				<a href={slice.primary.buttonLink.url}>
					<button class="bg-blue-300 hover:bg-blue-400 py-1 px-2 text-white"
						>{slice.primary.buttonText}</button
					>
				</a>
			{/if}
		</div>
		<div>
			{#if isFilled.image(slice.primary.image)}
				<div class="bg-gray-100">
					<PrismicImage field={slice.primary.image} sizes="100vw" class="w-full" />
				</div>
			{/if}
		</div>
	</div>

	<div
		class="grid grid-cols-1 items-center gap-8 md:grid-cols-2"
		class:hidden={!slice.primary.flip_orientation}
	>
		<div>
			{#if isFilled.image(slice.primary.image)}
				<div class="bg-gray-100">
					<PrismicImage field={slice.primary.image} sizes="100vw" class="w-full" />
				</div>
			{/if}
		</div>
		<div>
			<PrismicRichText field={slice.primary.text} />
			{#if slice.primary.buttonLink}
				<a href={slice.primary.buttonLink.url}>
					<button class="bg-blue-300 hover:bg-blue-400 py-1 px-2 text-white"
						>{slice.primary.buttonText}</button
					>
				</a>
			{/if}
		</div>
	</div>
</Bounded>
