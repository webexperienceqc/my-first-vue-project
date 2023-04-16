<script>


export default {
    data() {
        return {
            editing: false
        }
    },
    props: ['id', 'todo', 'completed', 'hideCompleted'],
    emits: ['toggleTodo', 'deleteTodo', 'saveTodo'],
    methods: {
        toggleTodo() {
            this.$emit('toggleTodo', this.id)
        },
        deleteTodo() {
            this.$emit('deleteTodo', this.id)
        },
        editTodo() {
            this.editing = true;
        },
        saveTodo() {
            this.$emit('saveTodo', this.id, this.$refs.updatedTodo.value)
            this.editing = false
        },
        showCompleted() {
            return this.hideCompleted ? !this.completed : true
        }
    }
}
</script>
<template >
    <li v-show="showCompleted()" :class="{ completed: completed }">
        <span>
            <input type="checkbox" @change="toggleTodo" :value="completed" :checked="completed">
        </span>
        <input v-if="editing" type="text" :value="todo" ref="updatedTodo">
        <span v-else>{{ todo }}</span>
        <span>
            <button v-if="editing" @click="saveTodo">Save</button>
            <button v-else @click="editTodo">Edit</button>
        </span>
        <span>
            <button @click="deleteTodo">x</button>
        </span>
    </li>
</template>

<style scoped>
.completed {
    text-decoration: line-through;
}
</style>