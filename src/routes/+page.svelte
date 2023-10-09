<script>
	import List from '../components/List.svelte';
	import Form from '../components/Form.svelte';
	import { v4 as uuidv4 } from 'uuid';
	import '$lib/types.js';
	import Filters from '../components/Filters.svelte';
	import { onMount, afterUpdate } from 'svelte';
	import { page, updated } from '$app/stores';

	/**
	 * @type {TodoList}
	 */
	let itemsList = [];
	let todoValue = '';
	let allDone = false;
	let filter = 'all';
	let filteredList = [];

	onMount(() => {
		const unsubscribe = page.subscribe((page) => {
			filter = page.url.searchParams.get('filter');
			updateFilteredList();
		});

		return unsubscribe;
	});

	const updateFilteredList = () => {
		filteredList = filter === 'completed' ? itemsList.filter((item) => item.isDone) : filter === 'active' ? itemsList.filter((item) => !item.isDone) : [...itemsList];
	}

	const handleSubmit = (value) => {
		todoValue = '';
		itemsList = [...itemsList, { value: value.detail.todoValue, id: uuidv4(), isDone: false }];
		updateFilteredList();
	};

	const handleChange = (value) => {
		console.log("handleChange1", value.detail.todoId)
		console.log("handleChange2", value)
		itemsList = itemsList.map((item) =>
			item.id === value.detail.todoId ? { ...item, isDone: !item.isDone } : item
		);
		console.log("itemsList", itemsList)
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
</script>

<h1>TODO!</h1>
<div>
	<Form bind:inputValue={todoValue} on:submit={handleSubmit} />
	<List
		bind:items={filteredList}
		bind:allDone
		on:changeItem={handleChange}
		on:changeAll={handleChangeAll}
		on:delete={handleDelete}
	/>
	{#if itemsList?.length !== 0}
		<Filters />
	{/if}
</div>
