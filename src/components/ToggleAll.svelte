<script>
	import Toggle from './Toggle.svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	const handleChangeAll = () => {
		dispatch('changeAll');
	};

	export let allDone = false;
</script>

<div class="flex items-center gap-3 p-2 bg-gray-700 text-white mt-4 rounded-lg">
	<Toggle bind:checked={allDone} id="all" on:change={handleChangeAll} />
	<span class="font-semibold">All done! 💯</span>
</div>
