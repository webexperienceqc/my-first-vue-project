<script>

export default {
    props: ['count', 'completedCount', 'hideCompleted'],
    emits: ['clearAll', 'clearCompleted', 'toggleCompletedVisibility'],
    methods: {
        clearAll() {
            this.$emit('clearAll')
        },
        clearCompleted() {
            this.$emit('clearCompleted')
        },
        toggleCompletedVisibility() {
            this.$emit('toggleCompletedVisibility')
        }
    }
}
</script>
<template>
    <p>{{ count }} Total Tasks</p>
    <p>{{ completedCount }} Completed Tasks <span v-if="hideCompleted">(hidden)</span></p>
    <p>{{ count - completedCount }} Pending Tasks</p>
    <button @click="clearAll">Clear All Tasks</button>
    <button @click="clearCompleted">Clear Completed Tasks</button>
    <button v-if="hideCompleted" @click="toggleCompletedVisibility">Show Completed Tasks</button>
    <button v-else @click="toggleCompletedVisibility">Hide Completed Tasks</button>
</template>

