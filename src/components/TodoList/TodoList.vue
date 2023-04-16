<script>
import TodoItem from './TodoItem/TodoItem.vue'
import TodoFooter from './TodoFooter/TodoFooter.vue'

let id = 0

export default {
    components: {
        TodoItem,
        TodoFooter
    },
    data() {
        return {
            newTodo: '',
            todos: [
                { id: ++id, todo: 'My first task', completed: false },
                { id: ++id, todo: 'My second task', completed: false },
                { id: ++id, todo: 'My third task', completed: false }
            ],
            hideCompleted: true,
            count: 3,
            completedCount: 0,
        }
    },
    mounted() {
        this.getLocalTodos();
    },
    watch: {
        todos: {
            deep: true,
            handler: function (updatedTodos) {
                this.updateCount()
                this.updateCompletedCount()
                localStorage.setItem('todo_list', JSON.stringify(updatedTodos));
            }
        },
        hideCompleted: function () {
            localStorage.setItem('hide_completed_todos', JSON.stringify(this.hideCompleted))
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
        saveTodo(id, updateTodo) {
            let todo = this.getTodoById(id)
            todo.todo = updateTodo
        },
        clearAll() {
            this.todos = []
        },
        clearCompleted() {
            this.todos = this.todos.filter(t => t.completed === false)
        },
        toggleCompletedVisibility() {
            this.hideCompleted = !this.hideCompleted
        },
        updateCount() {
            this.count = this.todos.length
        },
        updateCompletedCount() {
            this.completedCount = this.todos.filter(t => t.completed === true).length
        },
        getTodoById(id) {
            return this.todos.find(t => t.id === id)
        },
        getLocalTodos() {
            let localTodoList = localStorage.getItem('todo_list')
            if (localTodoList) this.todos = JSON.parse(localTodoList)

            let localHideCompleted = localStorage.getItem('hide_completed_todos')
            if (localHideCompleted) this.hideCompleted = JSON.parse(localHideCompleted)
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
        <TodoItem v-for="todo in todos" :id="todo.id" :todo="todo.todo" :completed="todo.completed"
            :hideCompleted="hideCompleted" @toggleTodo="toggleTodo" @deleteTodo="deleteTodo" @saveTodo="saveTodo" />
    </ul>

    <div>
        <TodoFooter :count="count" :completedCount="completedCount" :hideCompleted="hideCompleted" @clearAll="clearAll"
            @clearCompleted="clearCompleted" @toggleCompletedVisibility="toggleCompletedVisibility" />
    </div>
</template>

<style scoped>
.todolist {
    list-style-type: none
}
</style>