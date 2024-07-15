<script lang="ts">
	import { SliceZone } from '@prismicio/svelte';
	import { components } from '$lib/slices';
	import ContactForm from '$lib/components/ContactForm.svelte';

	export let data;
</script>

<SliceZone slices={data.page.data.slices} {components} />

<ContactForm />
