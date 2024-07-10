<script>
	import EventListDisplay from '$lib/components/EventListDisplay.svelte';

	/** @type {import("@prismicio/client").Content.EventListSlice} */
	export let slice;

	const eventList = slice.primary.events.sort((a, b) => Date.parse(b.date) - Date.parse(a.date));
	const pastEvents = eventList.filter((event) => Date.parse(event.date) < Date.now());
	const upcomingEvents = eventList.filter((event) => Date.parse(event.date) >= Date.now());
</script>

<section data-slice-type={slice.slice_type} data-slice-variation={slice.variation}>
	<EventListDisplay title={'Upcoming'} events={upcomingEvents} />
	{#if slice.primary.show_past_events}
		<EventListDisplay title={'Past Events'} events={pastEvents} />
	{/if}
</section>
