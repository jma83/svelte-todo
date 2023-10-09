<script>
	import '../types';

	/**
	 * @type {TodoList}
	 */
	export let items = [];
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	const handleChange = (todoId) => {
		dispatch('change', { todoId });
	};

	const handleDelete = (todoId) => {
		dispatch('delete', { todoId });
	};
</script>

<ul>
	{#each items as item (item.id)}
		<li>
			<div>
				<input
					type="checkbox"
					name="isDone{item.id}"
					id="isDone{item.id}"
					checked={item.isDone}
					on:change={() => handleChange(item.id)}
				/>
				<label for="isDone{item.id}">{item.value}</label>
			</div>
			<button type="button" on:click={() => handleDelete(item.id)}>X</button>
		</li>
	{/each}
</ul>

<style>
	ul {
		list-style: none;
		padding: 0;
	}

	li {
		display: flex;
		justify-content: space-between;
		margin: 0.5rem 0;
		border: 1px solid #ccc;
		background-color: #f9f9f9;
	}

	li > div:hover {
		background-color: #ececec;
	}

	li > div {
		width: 100%;
		display: flex;
	}

	li > button {
		display: none;
		visibility: hidden;
	}

	li:hover > button {
		display: block;
		visibility: visible;
	}

	label {
		display: flex;
		width: 100%;
		padding: 1rem;
	}
</style>
