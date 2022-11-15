<template>
    <div>
        <p id="module-title">{{title}}</p>
        <SingleTask v-for="task in tasks" :key="task.id" :task="task" @checkbox-changed="$emit('checkbox-changed', task.id)" 
        @delete-task="$emit('delete-task', task.id)" @edit-task="editTask" />
    </div>
</template>

<script>
import SingleTask from './SingleTask.vue';
export default{
    name: 'ShowTask',
    props: {
        tasks: Array,
        title: String
    },
    components: {
        SingleTask
    },
    methods: {
        editTask(newTask){
            this.$emit('edit-task', newTask);
        }
    },
    emits: ['checkbox-changed', 'delete-task', 'edit-task']
}
</script>

<style scoped>
#module-title{
    width: fit-content;
    margin: 0 0 5px;
    padding: 0.25rem 0.5rem;
    color: grey;
    background-color: rgb(249, 248, 248);
    border: 1px solid grey;
    font-size: 12px;
}
</style>
