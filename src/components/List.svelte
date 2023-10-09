<script>
	import '$lib/types.js';
	import Toggle from './Toggle.svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	/**
	 * @type {TodoList}
	 */
	export let items = [];
	export let allDone = false;

	const handleChange = (todoId) => {
		dispatch('changeItem', { todoId });
	};

	const handleChangeAll = () => {
		dispatch('changeAll');
	};

	const handleDelete = (todoId) => {
		dispatch('delete', { todoId });
	};
</script>

{#if items != null && items.length > 0}
	<div class="flex items-center gap-2 p-2">
		<Toggle bind:checked={allDone} id="all" on:change={handleChangeAll} /><span>Mark all</span>
	</div>
{/if}

<ul class="list-none p-0">
	{#each items as item (item.id)}
		<li>
			<div class="p-2 gap-2">
				<Toggle bind:id={item.id} bind:checked={item.isDone} on:change={() => handleChange(item.id)} />
				<label for="isDone{item.id}">{item.value}</label>
			</div>
			<button type="button" on:click={() => handleDelete(item.id)}>X</button>
		</li>
	{/each}
</ul>

<style>
	li {
		@apply flex justify-between;
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
	}
</style>
