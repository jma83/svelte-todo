<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	export let checked = false;
	export let id = '';

	const handleChange = () => {
		dispatch('change', id);
	};
</script>

<label class="relative inline-flex items-center cursor-pointer">
	<input
		type="checkbox"
		alt="Mark as done"
		id="switch-{id}"
		name="switch-{id}"
		{checked}
		on:change={handleChange}
		value=""
		class="sr-only peer"
	/>
	<span
		class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"
	/>
</label>
