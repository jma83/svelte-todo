<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let filters = [];
	export let currentFilter = 'all';
	export let items = [];

	const handleClick = () => {
		dispatch('clearDone');
	};
</script>

<section class="mt-6 flex justify-between items-center gap-4 flex-wrap md:flex-nowrap">
	<div class="flex justify-start items-center gap-4">
		{#each filters as filter}
			<a href="?filter={filter}" class="hover:underline" class:active={filter === currentFilter}
				>{filter}</a
			>
		{/each}
	</div>
	{#if items.filter((item) => item.isDone)?.length > 0}
		<button
			type="button"
			class="py-2 px-5 text-sm font-medium text-gray-900 bg-white rounded-lg border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:outline-none focus:z-10 focus:ring-4 focus:ring-gray-200"
			on:click={handleClick}>🗑 Clear done tasks</button
		>
	{/if}
</section>

<style>
	.active {
		@apply text-blue-500;
	}

	a:first-letter {
		text-transform: uppercase;
	}
</style>
