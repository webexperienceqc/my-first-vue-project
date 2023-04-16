<script>
import TodoItem from './TodoItem/TodoItem.vue'

let id = 0

export default {
    components: {
        TodoItem
    },
    data() {
        return {
            newTodo: '',
            todos: [
                { id: ++id, todo: 'My first task', completed: false },
                { id: ++id, todo: 'My second task', completed: false },
                { id: ++id, todo: 'My third task', complted: false }
            ]
        }
    },
    methods: {
        addTodo() {
            if (!this.newTodo) return
            this.todos.push({ id: ++id, todo: this.newTodo, completed: false })
            this.newTodo = ''
        },
        toggleTodo(id) {
            let todo = this.getTodoById(id)
            todo.completed = !todo.completed
        },
        deleteTodo(id) {
            this.todos = this.todos.filter(t => t.id !== id)
        },
        getTodoById(id) {
            return this.todos.find(t => t.id === id)
        }
    }
}
</script>
<template>
    <form @submit.prevent="addTodo">
        <input v-model="newTodo" type="text" placeholder="Add a todo">
        <button>Add Todo</button>
    </form>

    <ul class="todolist">
        <TodoItem v-for="todo in todos" :id="todo.id" :todo="todo.todo" :completed="todo.completed" @toggleTodo="toggleTodo"
            @deleteTodo="deleteTodo" />
    </ul>
</template>

<style scoped>
.todolist {
    list-style-type: none
}
</style>