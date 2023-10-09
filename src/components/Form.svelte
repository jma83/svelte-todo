<script>
	import { createEventDispatcher } from 'svelte';
	export let inputValue = '';
	export let allDone = false;
	const dispatch = createEventDispatcher();

	const handleSubmit = (e) => {
		submitCurrentValue(e.target.todoInput.value);
	};

	const handleChange = (todoId) => {
		dispatch('change', { todoId });
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
		type="checkbox"
		value="Mark all"
		alt="Mark All"
		checked={allDone}
		on:change={handleChange}
	/>
	<input
		type="text"
		name="todoInput"
		bind:value={inputValue}
		placeholder="What needs to be done?"
		autofocus
	/>
	<button type="submit">Add</button>
</form>
