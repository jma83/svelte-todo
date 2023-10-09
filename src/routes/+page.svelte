<script>
	import List from '../components/List.svelte';
	import Form from '../components/Form.svelte';
	import { v4 as uuidv4 } from 'uuid';
	import '../types';

	/**
	 * @type {TodoList}
	 */
	let itemsList = [];
	let todoValue = '';
	let allDone = false;

	const handleSubmit = (value) => {
		todoValue = '';
		itemsList = [...itemsList, { value: value.detail.todoValue, id: uuidv4(), isDone: false }];
	};

	const handleChange = (value) => {
		itemsList = itemsList.map((item) =>
			item.id === value.detail.todoId ? { ...item, isDone: !item.isDone } : item
		);
	};

	const handleChangeAll = () => {
		allDone = !allDone;
		itemsList = itemsList.map((item) => ({ ...item, isDone: allDone }));
	};

	const handleDelete = (value) => {
		itemsList = itemsList.filter((item) => item.id !== value.detail.todoId);
	};
</script>

<h1>TODO!</h1>
<div>
	<Form
		bind:inputValue={todoValue}
		bind:allDone
		on:submit={handleSubmit}
		on:change={handleChangeAll}
	/>
	<List bind:items={itemsList} on:change={handleChange} on:delete={handleDelete} />
	{#if itemsList?.length !== 0}
		<span>Filtros</span>
	{/if}
</div>
