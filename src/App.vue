<template>
	<div class="todos-wrapper">
		<div class="todos-container">
			<div class="illustration">
				<Illustration />
			</div>
			<div class="todos">
				<h1 class="title">Vue 3 Boodschappenlijstje</h1>
				<form class="form" @submit.prevent="addTodo()">
					<input
						v-model="newTodo"
						name="newTodo"
						autocomplete="off"
						placeholder="Typ hier"
					/>
					<button>Toevoegen</button>
				</form>
				<ul class="list">
					<li class="item" v-for="(todo, index) in todos" :key="index">
						<span
							class="cursor-pointer"
							:class="{'line-through': todo.done}"
							@click="doneTodo(todo)"
							>{{ todo.description }}</span
						>
						<button @click="removeTodo(index)"><Trash /></button>
					</li>
				</ul>
				<h3 v-if="!todos.length">Wat heb je nodig?</h3>
			</div>
		</div>
	</div>
</template>

<script>
import {ref} from "vue";
import Illustration from "./components/Illustration.vue";
import Trash from "./components/trash.vue";
export default {
	name: "App",
	setup() {
		const newTodo = ref("");
		const defaultData = [
			{
				done: false,
				description: "Aardappelen",
			},
		];
		const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
		const todos = ref(todosData);
		const addTodo = () => {
			if (newTodo.value) {
				todos.value.push({
					done: false,
					description: newTodo.value,
				});
				newTodo.value = "";
			}
			saveData();
		};

		const doneTodo = (todo) => {
			todo.done = !todo.done;
			saveData();
		};

		const removeTodo = (index) => {
			todos.value.splice(index, 1);
			saveData();
		};

		const saveData = () => {
			const storageData = JSON.stringify(todos.value);
			localStorage.setItem("todos", storageData);
		};

		return {
			todos,
			newTodo,
			addTodo,
			doneTodo,
			removeTodo,
			saveData,
		};
	},
	components: {
		Illustration,
		Trash,
	},
};
</script>

<style scoped>
.todos-wrapper {
	@apply container mx-auto text-center h-screen flex items-center justify-center;
}
.todos-container {
	@apply flex justify-center h-[600px];
}
.illustration {
	@apply w-1/3 mr-20;
}
.todos {
	@apply w-1/3 h-full text-center;
}
h1 {
	@apply py-10 text-3xl font-bold;
}
h2 {
	@apply text-xl;
}
.form {
	@apply flex w-full;
}
.form input {
	@apply w-full py-2 border-b-2 border-gray-400 focus:border-indigo-500 text-gray-600 placeholder-gray-400 outline-none;
}
.form button {
	@apply w-36 bg-indigo-500 hover:bg-indigo-700 transition-all duration-200 rounded-md ml-5 text-white;
}
.list {
	@apply py-5;
}
.item {
	@apply flex justify-between p-2 even:bg-indigo-100 rounded-md;
}
.item button {
	@apply hover:text-red-500 transition-all duration-200;
}
</style>
