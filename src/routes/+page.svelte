<script>
	import List from '../components/List.svelte';
	import Form from '../components/Form.svelte';
	import { v4 as uuidv4 } from 'uuid';
	import '$lib/types.js';
	import Filters from '../components/Filters.svelte';
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	const FILTER_ALL = 'all';
	const FILTER_DONE = 'done';
	const FILTER_ACTIVE = 'active';

	/**
	 * @type {TodoList}
	 */
	let itemsList = [];
	let todoValue = '';
	let allDone = false;
	let currentFilter = 'all';
	let filters = [FILTER_ALL, FILTER_DONE, FILTER_ACTIVE];
	let filteredList = [];

	onMount(() => {
		const unsubscribe = page.subscribe((page) => {
			currentFilter = page.url.searchParams.get('filter') || FILTER_ALL;
			allDone = false;
			updateFilteredList();
		});

		return unsubscribe;
	});

	const updateFilteredList = () => {
		switch (currentFilter) {
			case FILTER_ACTIVE:
				filteredList = itemsList.filter((item) => !item.isDone);
				break;
			case FILTER_DONE:
				filteredList = itemsList.filter((item) => item.isDone);
				break;
			default:
				filteredList = [...itemsList];
		}
	};

	const handleSubmit = (value) => {
		todoValue = '';
		itemsList = [...itemsList, { value: value.detail.todoValue, id: uuidv4(), isDone: false }];
		updateFilteredList();
	};

	const handleChange = (value) => {
		itemsList = itemsList.map((item) =>
			item.id === value.detail.todoId ? { ...item, isDone: !item.isDone } : item
		);
		updateFilteredList();
	};

	const handleUpdateTodo = (value) => {
		console.log('handle update', value);
		itemsList = itemsList.map((item) =>
			item.id === value.detail.todoId ? { ...item, value: value.detail.value } : item
		);
		console.log('itemsList', itemsList);
		updateFilteredList();
	};

	const handleChangeAll = () => {
		allDone = !allDone;
		itemsList = itemsList.map((item) => ({ ...item, isDone: allDone }));
		updateFilteredList();
	};

	const handleDelete = (value) => {
		itemsList = itemsList.filter((item) => item.id !== value.detail.todoId);
		updateFilteredList();
	};

	const clearDoneTasks = () => {
		itemsList = itemsList.filter((item) => !item.isDone);
		updateFilteredList();
	};
</script>

<h1>TODO LIST!</h1>
<div>
	<Form bind:inputValue={todoValue} on:submit={handleSubmit} />
	<List
		bind:items={filteredList}
		bind:allDone
		on:changeItem={handleChange}
		on:changeAll={handleChangeAll}
		on:updateTodo={handleUpdateTodo}
		on:delete={handleDelete}
	/>
	{#if itemsList?.length !== 0 && filteredList?.length === 0}
		<span class="p-1 font-semibold">No results for current filters 🙁</span>
	{/if}
	{#if itemsList?.length !== 0}
		<Filters
			bind:filters
			bind:currentFilter
			bind:items={itemsList}
			on:clearDone={clearDoneTasks}
		/>
	{/if}
</div>
