<template>
	<div>
		<h2>Todo Application</h2>
		<router-link to="/">Home</router-link>
		<hr />
		<AddTodo @add-todo="addTodo" />
		<select v-model="filter">
			<option value="all">All</option>
			<option value="completed">Completed</option>
			<option value="not-completed">Not Completed</option>
		</select>
		<hr />
		<Loader v-if="loading" />
		<TodoList
			v-else-if="filteredTodos.length"
			v-bind:todos="filteredTodos"
			@remove-todo="removeTodo"
		/>
		<p v-else>No todos!</p>
	</div>
</template>

<script>
import TodoList from '@/components/TodoList';
import AddTodo from '@/components/AddTodo';
import Loader from '@/components/Loader';
export default {
	name: 'App',
	data() {
		return {
			todos: [],
			loading: true,
			filter: 'all',
		};
	},
	mounted() {
		fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
			.then(response => response.json())
			.then(json => {
				setTimeout(() => {
					this.todos = json;
					this.loading = false;
				}, 1500);
			});
	},
	components: {
		TodoList,
		AddTodo,
		Loader,
	},
	// watch: {
	// 	filter(value) {
	// 		console.log(value);
	// 	},
	// },
	computed: {
		filteredTodos() {
			switch (this.filter) {
				case 'completed':
					return this.todos.filter(t => t.completed);
				case 'not-completed':
					return this.todos.filter(t => !t.completed);
				default:
					return this.todos;
			}
		},
	},
	methods: {
		removeTodo(id) {
			this.todos = this.todos.filter(t => t.id !== id);
		},
		addTodo(todo) {
			this.todos.push(todo);
		},
	},
};
</script>
