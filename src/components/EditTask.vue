<template>
    <div id="edit-task">
            <form>
                <label>Due Date:</label>
                <input type="date" v-model.lazy="task.dueDate" required />

                <label>Content:</label>
                <input type="text" placeholder="Enter Task" v-model.lazy="task.content" required />
                
                <button @click.prevent="submit">Save</button>
                <button @click.prevent="$emit('close-edit-form')" >Cancel</button>
            </form>
        </div>
</template>

<script>
export default{
    name: 'EditTask',
    props: {
        oldTask: Object
    },
    data(){
        return {
            task: {
                dueDate: this.oldTask.dueDate,
                content: this.oldTask.content,
            }
        }
    },
    methods: {
        submit(){
            let tmp_task = {
                content: this.task.content,
                dueDate: this.task.dueDate,
                id: this.oldTask.id
            };

            this.$emit('edit-task', tmp_task);
        }
    }
}
</script>

<style scoped>
#edit-task{
    width: 100%;
}
</style>
