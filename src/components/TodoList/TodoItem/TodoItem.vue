<script>
export default {
    props: ['id', 'todo', 'completed'],
    methods: {
        toggleTodo() {
            this.$emit('toggleTodo', this.id)
        },
        deleteTodo() {
            this.$emit('deleteTodo', this.id)
        }
    }
}
</script>
<template>
    <li :class="{ completed: completed }">
        <span>
            <input type="checkbox" @change="toggleTodo">
        </span>
        {{ todo }}
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