<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let inputValue = '';

	const handleSubmit = (e) => {
		e.preventDefault();
		submitCurrentValue(e.target.todoInput.value);
	};

	const submitCurrentValue = (value) => {
		if (value.length === 0) {
			return;
		}
		dispatch('submit', { todoValue: value });
	};
</script>

<form on:submit={handleSubmit}>
	<input
		type="text"
		name="todoInput"
		bind:value={inputValue}
		placeholder="What needs to be done?"
		autofocus
	/>
	<button type="submit">Add</button>
</form>

<style>
	button {
		@apply text-white bg-blue-700 font-medium rounded-lg text-sm px-4 py-2 mr-2 mb-2;
	}

	button:hover {
		@apply bg-blue-800;
	}

	button:focus {
		@apply ring-4 ring-blue-300 outline-none;
	}
</style>
