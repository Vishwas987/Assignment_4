<template>
    <div id="add-tasks">
        <form>
            <h2>Add New Task</h2>
            <label>Author:</label>
            <input type="text" placeholder="Author name" v-model.lazy="task.author" required />
            <label>Module:</label>
            <select v-model.lazy="task.module">
                <option v-for="module_ in modulesList" :key="module_">{{ module_ }}</option>
            </select>
            <label>Due Date:</label>
            <input type="date" v-model.lazy="task.dueDate" required />
            <hr />
            <label>Content:</label>
            <input type="text" placeholder="Enter Task" v-model.lazy="task.content" required />
            <hr />
            <button @click.prevent="submit">Add Task</button>
            <button @click.prevent="$emit('close-form')" >Cancel</button>
        </form>
    </div>
</template>

<script>
export default{
    name: 'AddTasks',
    data(){
        return {
            task: {
                author: '',
                module: '',
                content: '',
                dueDate: '',
                done: false
            },
            modulesList: ['IT', 'New Hire Paperwork', 'Culture Orientation', 'Other']
        }
    },
    methods: {
        submit(){
            //console.log(this.task);
            let tmp_task = {
                author: this.task.author,
                module: this.task.module,
                content: this.task.content,
                dueDate: this.task.dueDate,
                done: false
            };

            this.$emit('add-new-task', tmp_task);
        }
    }

}
</script>