<template>
    <div id="add-tasks">
        <form>
            <h2>Add New Task</h2>

            <label>Assignee:</label>
            <label class="mdc-text-field  mdc-text-field--filled  mdc-text-field--no-label input-field mb-2">
              <span class="mdc-text-field__ripple" />
              <input class="mdc-text-field__input" type="text" placeholder="Assignee Name" aria-label="Label" 
                v-model.lazy="task.author" requiredd>
              <span class="mdc-line-ripple" />
            </label>

            <label>Module:</label>
            <select class="input-field mb-2" v-model.lazy="task.module">
                <option v-for="module_ in modulesList" :key="module_">{{ module_ }}</option>
            </select>

            <label>Due Date:</label>
            <input class="input-field mb-2"  type="date" v-model.lazy="task.dueDate" required>
            
            <label>Content:</label>
            <label class="mdc-text-field  mdc-text-field--filled  mdc-text-field--no-label input-field mb-2">
              <span class="mdc-text-field__ripple" />
              <input class="mdc-text-field__input" type="textarea" rows="4" placeholder="Enter Task" aria-label="Label" 
                v-model.lazy="task.content" required>
              <span class="mdc-line-ripple" />
            </label>
            
            <button class="mdc-button mdc-button--outlined mb-2" @click.prevent="submit">
              <span class="mdc-button__ripple"></span>
              <span class="mdc-button__label">Add Task</span>
            </button>
            <button class="mdc-button mdc-button--outlined mb-2" @click.prevent="$emit('close-form')" >
              <span class="mdc-button__ripple"></span>
              <span class="mdc-button__label">Cancel</span>
            </button>
            
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

<style scoped>
.mb-2{
    margin-bottom: 1rem;
}

form{
    display: flex;
    flex-direction: column;
    width: 50%;
    margin: 0 auto;
}

.input-field{
    height: 46px;
    background-color: whitesmoke;
    border: none;
}
.input-field:hover{
    background-color: rgb(231, 231, 231);
}
</style>
