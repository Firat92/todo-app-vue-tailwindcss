<template>
	<main class="bg-slate-50 h-screen">
		<h1 class="bg-cyan-500 w-6/12 my-4 mx-auto p-4 text-center rounded-md"
			>Todo Application Vue</h1
		>
		<form @submit.prevent="addNewTodo" class="mx-auto w-6/12 flex flex-col">
			<div class="flex flex-start my-2">
				<label class="bg-yellow-400 p-2 rounded-md">New Todo</label>
				<input
					v-model="newTodo"
					name="newTodo"
					placeholder=" Write your new todo..."
					class="bg-slate-200 ml-4 flex-1 rounded-md"
				/>
			</div>
			<button class="bg-orange-500 p-2 hov">Add New Todo</button>
		</form>
		<div class="w-6/12 mx-auto flex justify-between my-4">
			<button @click="removeAllTodos" class="bg-pink-500 p-2 hov"
				>Remove All Todos</button
			>
			<button @click="markAllDone" class="bg-red-500 p-2 hov"
				>Mark All Done
			</button>
		</div>
		<ul class="w-6/12 mx-auto flex flex-col my-4">
			<li v-for="(todo, index) in todos" :key="todo.id" class="todo">
				<h3 :class="{ done: todo.done }" @click="toggleDone(todo)">
					{{ todo.content }}</h3
				>
				<div class="flex justify-between my-2">
					<button
						@click="removeTodo(index)"
						class="bg-purple-500 p-1 cursor-pointer hov"
						>Remove Todo</button
					>
					<button
						@click="markTodo(todo)"
						class="bg-rose-400 p-1 cursor-pointer hov"
						>Mark Todo</button
					>
				</div>
			</li>
		</ul>
	</main>
</template>

<script>
import { ref, onMounted, watch } from 'vue';

export default {
	setup() {
		const newTodo = ref('');
		const todos = ref([]);

		function addNewTodo() {
			if (newTodo.value === '') {
				return;
			}
			todos.value.push({
				id: Date.now(),
				done: false,
				content: newTodo.value,
			});
			newTodo.value = '';
		}

		function toggleDone(todo) {
			todo.done = !todo.done;
		}

		function markTodo(todo) {
			toggleDone(todo);
		}

		function removeTodo(index) {
			todos.value.splice(index, 1);
		}

		function markAllDone() {
			todos.value.forEach((todo) => (todo.done = !todo.done));
		}

		function removeAllTodos() {
			todos.value = [];
		}

		watch(newTodo, (newVal) => {
			localStorage.setItem('newTodo', newVal);
		});

		watch(
			todos,
			(newVal) => {
				localStorage.setItem('todos', JSON.stringify(newVal));
			},
			{
				deep: true,
			}
		);

		onMounted(() => {
			newTodo.value = localStorage.getItem('newTodo') || '';
			todos.value = JSON.parse(localStorage.getItem('todos')) || [];
		});

		return {
			newTodo,
			todos,
			addNewTodo,
			toggleDone,
			removeTodo,
			removeAllTodos,
			markAllDone,
			markTodo,
		};
	},
};
</script>
