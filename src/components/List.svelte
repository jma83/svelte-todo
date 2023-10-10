<script>
	import '$lib/types.js';
	import Toggle from './Toggle.svelte';
	import ToggleAll from './ToggleAll.svelte';

	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	/**
	 * @type {TodoList}
	 */
	export let items = [];
	export let allDone = false;
	let activeId = null;

	const handleChange = (todoId) => {
		dispatch('changeItem', { todoId });
	};

	const handleChangeAll = () => {
		dispatch('changeAll');
	};

	const handleDelete = (todoId) => {
		dispatch('delete', { todoId });
	};

	const handleActiveItem = (todoId) => {
		activeId = todoId;
	};

	const handleSubmit = (e, todoId) => {
		e.preventDefault();
		activeId = null;
		dispatch('updateTodo', { todoId, value: e.target.todoInput.value });
	};
</script>

{#if items?.length > 0}
	<ToggleAll bind:allDone on:changeAll={handleChangeAll} />
{/if}
<div class="bg-white rounded-lg shadow-lg mt-2">
	<ul class="list-none p-0">
		{#each items as item (item.id)}
			<li>
				<div class="px-2 py-3 gap-3">
					<Toggle
						bind:id={item.id}
						bind:checked={item.isDone}
						on:change={() => handleChange(item.id)}
					/>
					{#if activeId !== item.id}
						<label for="isDone{item.id}" on:dblclick={() => handleActiveItem(item.id)}
							>{item.value}</label
						>
					{:else}
						<form on:submit={(e) => handleSubmit(e, item.id)}>
							<input class="h-7" type="text" value={item.value} name="todoInput" />
						</form>
					{/if}
				</div>
				<button
					type="button"
					class="bg-gray-400 hover:bg-red-500 w-6 h-6 rounded-full text-white mr-2"
					on:click={() => handleDelete(item.id)}>x</button
				>
			</li>
		{/each}
	</ul>
</div>

<style>
	li {
		@apply flex justify-between items-center;
	}

	li:hover {
		@apply bg-blue-100;
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
